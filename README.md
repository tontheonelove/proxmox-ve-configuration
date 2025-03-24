<h1 align="center">🔥Proxmox-ve-Configuration & Fix Issue Guide By Ton🔥</h1>

<img src= proxmox.png/>

### Config List Update 24 - 3 - 25
✅ [Load Balancer](loadbalance.md)

✅ [Custom NTP Server](Custom%20NTP%20Servers.md)

✅ [Config VLAN For Trunk Port](Config%20VLAN.md)

✅ [Migrate VM To Proxmox](Migrate%20VM%20to%20Proxmox.md)

✅ [Enable qemu-guest-agent](Enable%20qemu-guest-agent.md)

✅ [Detroy Ceph Pool (Force)](Detroy%20Ceph%20Pool%20(Force).md)

🪛 [Fix Unable to wipe diskhas a holder (500)](Fix%20Unable%20to%20wipe%20diskhas%20a%20holder%20(500).md)

🪛 [FIX fails to boot error: dracut:/# Centos](Fix%20fails%20to%20boot%20error%3A%20dracut%3A%20Centos.md)

🪛 [FIX ERROR: migration aborted (duration 00:00:00): Can't connect to destination address using public key](Can't%20connect%20to%20destination%20address%20using%20public%20key.md)


# Restart Service when Change host IP
```
systemctl restart pve-cluster && systemctl restart corosync
```
# Cluster Restart
```
systemctl start pve-cluster
```
# ceph restart 
```
systemctl restart ceph-mon.target
```
---------------------------------------------

# Import QCOW2 into Proxmox
```
qm importdisk 100 ubuntu.qcow2 local-lvm
```
---------------------------------------------


# Unlock VMID
```
#qm unlock (VMID)
```
# Fix PG Alert 
```
#restart all osd and monitor 
```
# Unmount an  NFSshare
```
#fusermount -uz /mnt/pve/NASxxx
```

# remove vm
```
#qm destroy (VMID)
```

# Removing the Node
```
#pvecm delnode nodename
```

# Fix booting in emergency mode
#nano nano /etc/fstab
```
insert  # front #UUID=XXXXXXXX /boot/efi vfat defaults 0 1
```

# Fix listing images failed/ (2) No such file or directory (500)

```
#rbd ls -l [cephpool]  (default)

#rbd -p [cephpool] list  (for missing vm list)

#rbd rm vm-[id]-disk-[id] -p [cephpool]
```




