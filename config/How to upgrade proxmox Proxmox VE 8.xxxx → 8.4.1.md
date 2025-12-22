# How to upgrade proxmox Proxmox VE 8.xxxx → 8.4.1

## Cluster + Ceph (3 node and more)

### Summary before we begin (read this, it's very important):

✅ Do not delete the cluster.

✅ Ceph will not crash if done correctly.

❌ Do not upgrade all 3 hosts simultaneously.

✅ VMs/CT can continue running (if there is quorum).

⚠️ At least 2 nodes must have quorum at all times.


### 1. Check the cluster status.

```
pvecm status
```

## * Do not upgrade if you do not have enough quorum.

### 2. Check Ceph status.

```
ceph -s
```
✅ HEALTH_OK

### * If the OSD is down, please fix it first.

### 3. Backup (Highly Recommended)

Backup critical VMs/CTs.

Or take a snapshot if your storage allows.



### Upgrade Order (Very Important)

#### Recommended Order:

1. The node with no critical VMs or the fewest VMs.

2. The second node.

3. The final node (Master/Primary).


## Upgrade Procedure per Host (One machine at a time):

1️⃣ Live migrate VMs/CTs out of that node.

Move all VMs/CTs to another node (or at least maintain the quorum).

2️⃣ Turn off the Ceph OSD on that node (if it's a Ceph node). (Config From GUI)

3️⃣ Update package list

````
apt update
````

4️⃣ Upgrade Proxmox

````
apt full-upgrade
````

If prompted:

✅ Keep config → Select Keep local version

✅ Service restart → Answer Yes


6️⃣ After booting back up, check the version.

````
pveversion
````

7️⃣ Turn Ceph OSD back on (Config From GUI).

````
ceph -s
````

8️⃣ Move the VM back to/to the next node.


## ‼️ Impact on Cluster & Ceph

✔️ Cluster config will not be lost

✔️ No need to recreate the cluster

✔️ API/GUI will work normally

⚠️ During a reboot, one node will be lost → Quorum is required.


## Strictly Prohibited ❌

❌ Do not upgrade multiple nodes simultaneously.

❌ Do not reboot the entire cluster at once.

❌ Do not upgrade if the quorum is insufficient.

❌ Do not upgrade if Ceph health = ERROR.


## Short Checklist Before You Begin:

✅pvecm status → quorum yes

✅ceph -s → health ok

✅Backup completed

✅Upgrade host by host





