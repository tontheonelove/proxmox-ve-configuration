# Removing Host and Ceph OSD ✅

## 1. Remove the OSD of the broken host from the Ceph Cluster (from another host)

Use the command on another node in the cluster (that is still working):

```
ceph osd tree
```

Look at what OSD of the broken host is, e.g. osd.3, osd.7

Stop and delete the OSD (if the disk is broken or there is no way to recover it):

```
ceph osd out osd.<ID>
ceph osd crush remove osd.<ID>
ceph auth del osd.<ID>
ceph osd rm osd.<ID>
```

⚠️ If the OSD still has important data and the host can still recover, you should try to backup or migrate first.


## 2. Remove Host from Ceph Crush Map

```
ceph osd crush rm <hostname>
```

## 3. Remove the Host from the Proxmox Cluster (via a working node)

For example, the broken node name is pve-node5

```
pvecm delnode pve-node5
```

✅ Can only be deleted when the node is actually offline (not active in the cluster).

## 4. Check that the cluster has 4 nodes remaining and is working normally.

✅Check quorum:

```
pvecm status
```

✅Check Ceph Health:

```
ceph -s
```

📌 If the Disk is still good, but only the Host is broken
If the HDD that is the OSD is still usable and you want to move the OSD to a new machine, do the following:

Option: Move OSD Disk to a new machine

### 1.Place the disk (e.g. /dev/sdb) on another host.

### 2.Use the command:

```
ceph-volume lvm activate --all
```

Or:

```
ceph-volume lvm activate osd.<id> <UUID>
```

✅ Check if OSD is back online

✅ Then delete the broken host










