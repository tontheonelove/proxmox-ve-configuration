# proxmox-ve-configuration

Config Cluster 

# Config VLAN

#nano /etc/network/interfaces

#ADD THIS#  Example use VLAN 2  

auto vmbr0.2

iface vmbr0.2 inet static

        address 10.2.2.254/24
        
        gateway 10.2.2.1

OR  GUI Config

<img src=3704490324.png/>


#systemctl restart networking

--------------------------------------------


# /dev/sda has a holder | Solution

#pvs

#vgremove (your storage name)


---------------------------------------------
# Migrate VM to Proxmox

qm importdisk 100 vmdkname.vmdk local-lvm --format raw

---------------------------------------------

# fails to boot error: dracut:/# Centos 

boot to rescue  

dracut -f --regenerate-all

---------------------------------------------

# Import QCOW2 into Proxmox

qm importdisk 100 ubuntu.qcow2 local-lvm

---------------------------------------------
