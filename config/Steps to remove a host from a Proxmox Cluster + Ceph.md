#  Steps to remove a host from a Proxmox Cluster + Ceph

## 1. Check the Cluster and Ceph status.

### On a working node

```````
pvecm status
ceph -s
```````

## 2. Delete the Ceph OSD of the Host to be removed.

### For all OSDs on the Host
```````
ceph osd out <osd-id>
ceph osd crush remove <osd-name>
ceph auth del osd.<osd-id>
ceph osd rm <osd-id>
```````

## 3. Delete Ceph Monitor/Manager (if any) on that Host.

### If there is a monitor
```````
ceph mon remove <hostname>
```````

### If there is amanager
```````
ceph mgr fail <hostname>
ceph mgr rm <hostname>
```````

## 4. Remove the Host from the Proxmox Cluster.

```````
pvecm delnode <hostname>
```````


## 5. Check the results.

### Check if the host is missing from the cluster.
```````
pvecm nodes
```````

### Verify that Ceph returns HEALTH_OK.
```````
ceph -s
```````


## ⚠️ Caution

If a cluster has only 3 nodes and one node is removed, it will leave 2 nodes, which may cause quorum issues. Add a new node before removing it.


Do not pvecm delnode before deleting the Ceph OSD/Monitor/Manager, or Ceph will encounter HEALTH_WARN/ERR.

