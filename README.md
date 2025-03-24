<h1 align="center">🔥Proxmox-ve-Configuration & Fix Issue Guide By Ton🔥</h1>

<img src= proxmox.png/>

### Config List Update 24 - 3 - 25
✅ [Load Balancer](loadbalance.md)

✅ [Custom NTP Server](Custom%20NTP%20Servers.md)

✅ [Config VLAN For Trunk Port](Config%20VLAN.md)

✅ [Migrate VM To Proxmox](Migrate%20VM%20to%20Proxmox.md)

🪛 [Fix Unable to wipe diskhas a holder (500)](Fix%20Unable%20to%20wipe%20diskhas%20a%20holder%20(500).md)

🪛 [FIX fails to boot error: dracut:/# Centos]()


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

# IMPORT OVA TO PROXMOX

upload ove to proxmox host  or NAS
```
#tar xvf openppm.ova
```
Create new guest
```
#qm importdisk 100 yourfile.vmdk local-lvm -format qcow2
```
---------------------------------------------

# Enable qemu-guest-agent

For linux  
```
#apt-get install qemu-guest-agent -y  (Ubuntu)

#yum install qemu-guest-agent -y (CentOS)

#systemctl start qemu-guest-agent

#systemctl enable qemu-guest-agent
```

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

# ERROR: migration aborted (duration 00:00:00): Can't connect to destination address using public key
```
#ssh -o "HostKeyAlias=hostname" root@ip-address issue host
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

# Detroy Ceph Pool (Force)
```
#pveceph pool destroy <pool-name> --force
```



