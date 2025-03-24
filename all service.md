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
