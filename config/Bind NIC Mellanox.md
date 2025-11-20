# How to Bind NIC Mellanox  (for directly use in vm)


## update 
`````
apt update && apt full-upgrade -y
`````

## Enable IOMMU (AMD) and load VFIO

`````
nano /etc/default/grub
`````

#### change this line

`````
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt"
`````

`````
update-grub
`````

### Load VFIO modules at boot
`````
nano /etc/modules
`````

### add this code

`````
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
`````

#### reboot


## Verify IOMMU and identify NICs

`````
dmesg | grep -e IOMMU -e AMD-Vi

`````


#### List NIC devices with IDs

`````
lspci -nn | grep -i ether
`````


#### Detach NIC drivers and bind to VFIO

`````
nano /etc/modprobe.d/blacklist-nics.conf
`````

#### add 
`````
blacklist mlx5_core
blacklist mlx5_ib
`````


#### Bind specific NIC IDs to vfio-pci
`````
nano /etc/modprobe.d/vfio.conf
`````
#### example   this exxample 15b3:1017
`````
options vfio-pci ids=15b3:1017 disable_vga=1
`````
`````
update-initramfs -u
`````
#### reboot


### Confirm devices are on VFIO and IOMMU groups are clean

`````
lspci -nnk | grep -A3 -i ether
`````

`````
Assign PCI devices to the Mikrotik VM (GUI)
Create the VM: ติดตั้ง Mikrotik x86 (CHR หรือ RouterOS x86) ตามปกติ
Add PCI devices:
ไปที่ VM → Hardware → Add → PCI Device
เลือก NIC ทีละใบ (ตาม PCI address)
ติ๊ก “All Functions” ถ้าเป็น multi-function device
สำหรับ NIC ไม่ต้องติ๊ก “Primary GPU”
Boot VM: ตรวจสอบว่า VM start ได้ ไม่มี error
`````









