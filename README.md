# proxmox-ve-configuration

# Config VLAN

#nano /etc/network/interfaces

#ADD THIS#  Example use VLAN 2  

auto vmbr0.2

iface vmbr0.2 inet static

        address 10.2.2.254/24
        
        gateway 10.2.2.1


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

#qm unlock <VMID>




