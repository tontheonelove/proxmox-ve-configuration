# Config VLAN

```
nano /etc/network/interfaces
```

### ADD THIS#  Example use VLAN 2  
```
auto vmbr0.2

iface vmbr0.2 inet static

        address 10.2.2.254/24
        
        gateway 10.2.2.1
```


---------------------------------------------

## OR  GUI Config

<img src=3704490324.png/>

```
#systemctl restart networking
```
--------------------------------------------
