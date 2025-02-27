# proxmox-ve-configuration

# ceph restart 
```
systemctl restart ceph-mon.target
```

# config loadbalance or boding interface

#nano /etc/network/interfaces

First Local Network (access mode)  =>  ens18 and ens19  => bond0

Seconsdary public network with Vlan (trunk mode) => ens20 and ens21  => bond1

vmbr0  is Interface access vlan (from switch) for manage proxmox (local)

vmbr1.18  is Interface Vlan 18 trunk mode (from switch)  for manage proxmox (public)
```
auto lo
iface lo inet loopback

auto ens18
iface ens18 inet manual

auto ens19
iface ens19 inet manual

auto ens20
iface ens20 inet manual

auto ens21
iface ens21 inet manual

auto bond1
iface bond1 inet manual
        bond-slaves ens20 ens21
        bond-miimon 100
        bond-mode balance-rr

auto bond0
iface bond0 inet manual
        bond-slaves ens18 ens19
        bond-miimon 100
        bond-mode balance-rr

auto vmbr1
iface vmbr1 inet manual
        bridge-ports bond1
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094

auto vmbr0
iface vmbr0 inet static
        address xxx.xxx.xxx.xxx/24
        bridge-ports bond0
        bridge-stp off
        bridge-fd 0

auto vmbr1.18
iface vmbr1.18 inet static
        address xxx.xxx.xxx.xxx/24
        gateway xxx.xxx.xxx.xx
```

--------------------------------------------

# Config VLAN

#nano /etc/network/interfaces

#ADD THIS#  Example use VLAN 2  
```
auto vmbr0.2

iface vmbr0.2 inet static

        address 10.2.2.254/24
        
        gateway 10.2.2.1
```

---------------------------------------------

OR  GUI Config

<img src=3704490324.png/>


#systemctl restart networking

--------------------------------------------


# /dev/sda has a holder | Solution

#pvs

#vgremove (your storage name)


---------------------------------------------
# Migrate VM to Proxmox

qm importdisk 100 vmdkname.vmdk local-lvm --format raw    <-- For Node Disk

qm importdisk 100 vmdkname.vmdk NAS --format raw    <-- For NAS Disk

qm importdisk 100 vmdkname.vmdk pool --format raw    <-- For Ceph Disk

---------------------------------------------

# fails to boot error: dracut:/# Centos 

boot to rescue  

dracut -f --regenerate-all

---------------------------------------------

# Import QCOW2 into Proxmox

qm importdisk 100 ubuntu.qcow2 local-lvm

---------------------------------------------

# IMPORT OVA TO PROXMOX

upload ove to proxmox host  or NAS

#tar xvf openppm.ova

Create new guest

#qm importdisk 100 yourfile.vmdk local-lvm -format qcow2

---------------------------------------------

# Enable qemu-guest-agent

For linux  

#apt-get install qemu-guest-agent -y  (Ubuntu)

#yum install qemu-guest-agent -y (CentOS)

#systemctl start qemu-guest-agent

#systemctl enable qemu-guest-agent

# Unlock VMID

#qm unlock (VMID)

# Fix PG Alert 

#restart all osd and monitor 

# Unmount an  NFSshare
#fusermount -uz /mnt/pve/NASxxx

# remove vm
#qm destroy (VMID)

# ERROR: migration aborted (duration 00:00:00): Can't connect to destination address using public key
#ssh -o "HostKeyAlias=hostname" root@ip-address issue host

# Removing the Node
#pvecm delnode nodename

# Fix booting in emergency mode
#nano nano /etc/fstab

insert  # front #UUID=XXXXXXXX /boot/efi vfat defaults 0 1


# Fix listing images failed/ (2) No such file or directory (500)

#rbd ls -l [cephpool]  (default)

#rbd -p [cephpool] list  (for missing vm list)

#rbd rm vm-[id]-disk-[id] -p [cephpool]


# Detroy Ceph Pool (Force)

#pveceph pool destroy <pool-name> --force




