# How to config cache r/w for ceph osd

## 🔧 Requirements

- Proxmox VE installed and ready to use
- Ceph (Octopus or higher, Reef or Quincy recommended)
- M.2 SSD to be used as cache (fast and durable NVMe is recommended)
- HDD or SSD to be used as primary OSD


## 🧩 Key Components Description of Ceph Bluestore
- block: The primary device that stores data (e.g. HDD)
- block.db: Metadata database (should be on SSD/M.2 for speed)
- block.wal: Write-ahead log (WAL), this can be either separate or combined with block.db


## for bash

### for block.db only
```
pveceph osd create /dev/sda --block-db /dev/nvme0n1
```

### for block.db and block.wal
```
pveceph osd create /dev/sda --block-db /dev/nvme0n1 --block-wal /dev/nvme0n1
```


## for UI

### for block.db only

<img src= /pic/s1.png />

----------------------------------------

### for block.db and block.wal

<img src= /pic/s2.png />





