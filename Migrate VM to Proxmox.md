# Migrate VM to Proxmox

### For local Disk
```
qm importdisk 100 vmdkname.vmdk local-lvm --format raw 
```
### For NAS Disk
```
qm importdisk 100 vmdkname.vmdk NAS --format raw
```
### For Ceph Disk
```
qm importdisk 100 vmdkname.vmdk pool --format raw 
```
