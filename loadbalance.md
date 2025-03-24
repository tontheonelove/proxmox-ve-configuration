## config loadbalance or boding interface

`````
nano /etc/network/interfaces
`````

- First Local Network (access mode) => ens18 and ens19 => bond0
- Seconsdary public network with Vlan (trunk mode) => ens20 and ens21 => bond1
- vmbr0 is Interface access vlan (from switch) for manage proxmox (local)
- vmbr1.18 is Interface Vlan 18 trunk mode (from switch) for manage proxmox (public)
`````
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

`````
