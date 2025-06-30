# 🔰How to Saparate ceph pool🔰


##  ✅Example 

There are 3 hosts with Proxmox VE installed and Ceph installed
There are 2 types of OSD:
- SATA OSD (slow) → Required for Pool A
- M.2/NVMe OSD (fast) → Required for Pool B

### 1. Install Ceph and create OSD

Install OSD for all 3 hosts, clearly separating device types, such as:

- 💻Host 1:

/dev/sdX → OSD SATA

/dev/nvme0n1 → OSD NVMe

- 💻Host 2:

/dev/sdY → OSD SATA

/dev/nvme1n1 → OSD NVMe

- 💻Host 3:

/dev/sdZ → OSD SATA

/dev/nvme2n1 → OSD NVMe

…
