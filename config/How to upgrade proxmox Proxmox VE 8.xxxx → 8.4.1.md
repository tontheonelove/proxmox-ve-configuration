# How to upgrade proxmox Proxmox VE 8.xxxx → 8.4.1

## Cluster + Ceph (3 node and more)

### Summary before we begin (read this, it's very important):

✅ Do not delete the cluster.

✅ Ceph will not crash if done correctly.

❌ Do not upgrade all 3 hosts simultaneously.

✅ VMs/CT can continue running (if there is quorum).

⚠️ At least 2 nodes must have quorum at all times.


## Phase 1: เตรียมระบบ (ทำครั้งเดียวบน Node ใดก็ได้)ล็อกสถานะ Ceph เพื่อไม่ให้ข้อมูลย้ายไปมาขณะ Reboot:Bashceph osd set noout

```
ceph osd set nobackfill
ceph osd set norecover
```

เช็กความพร้อมครั้งสุดท้าย: เข้า GUI ดูว่าทุกอย่างเป็นสีเขียว (Health OK)Phase 2: เริ่มอัปเกรดทีละ Node (ทำจาก Node 1 -> 2 -> 3)ทำกับ Node ที่เลือก 

(สมมติเริ่มที่ Node 1):ย้าย VM ออก: ใช้เมนู Bulk Migrate ย้าย VM ทั้งหมดจาก Node 1 ไปไว้ที่ Node 2 และ 3 (Live Migration)อัปเดต Repository: ตรวจสอบว่า 

/etc/apt/sources.list เป็น bookworm แล้วรันคำสั่งอัปเกรด:Bashapt update

```
apt dist-upgrade -y
```


Reboot: สั่ง reboot เพื่อให้ Kernel ใหม่ทำงานตรวจสอบหลัง Reboot: * เช็กเวอร์ชันด้วยคำสั่ง pveversion (ต้องเป็น 8.4)

เช็กเมนู Ceph > OSD ใน GUI ว่า OSD ของ Node นี้กลับมาเป็นสถานะ up และ in ทั้งหมดย้าย VM กลับ: ย้าย VM บางส่วนกลับมาที่ Node 1 เพื่อทดสอบความเรียบร้อย

[เมื่อ Node 1 ปกติแล้ว ให้สลับไปทำแบบเดียวกันกับ Node 2 และ Node 3 จนครบ]Phase 3: ปิดงาน (ทำหลังอัปเกรดครบ 3 Nodes)

ปลดล็อก Ceph: เพื่อให้ระบบกลับมาทำงานเต็มรูปแบบ:Bashceph osd unset noout

```
ceph osd unset nobackfill
ceph osd unset norecover
```

ตรวจสอบสุขภาพรวม: 
รัน 
```
ceph -s
```
ต้องเห็นสถานะเป็น HEALTH_OKสรุปคำสั่งสำคัญที่ต้องใช้:

คำสั่งวัตถุประสงค์

eph -sเช็กสุขภาพ 

Cephpveversion -vเช็กเวอร์ชัน 

Proxmox อย่างละเอียด

apt update && apt dist-upgrade -y   คำสั่งอัปเกรดหลักpvecm statusเช็กสถานะการเชื่อมต่อของ Cluster



