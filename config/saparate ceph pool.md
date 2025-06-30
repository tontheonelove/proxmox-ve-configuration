# 🔰How to Saparate ceph pool🔰


##  ✅Example 

There are 3 hosts with Proxmox VE installed and Ceph installed
There are 2 types of OSD:
- SATA OSD (slow) → Required for Pool A
- M.2/NVMe OSD (fast) → Required for Pool B

### 1. Install Ceph and create OSD

Install OSD for all 3 hosts, clearly separating device types, such as:

- 💻Host 1:

/dev/sdX → OSD SATA

/dev/nvme0n1 → OSD NVMe

- 💻Host 2:

/dev/sdY → OSD SATA

/dev/nvme1n1 → OSD NVMe

- 💻Host 3:

/dev/sdZ → OSD SATA

/dev/nvme2n1 → OSD NVMe


### 2. Check and set Device Class
After creating the OSD, check if each OSD is categorized into a device class:

```
ceph osd tree
ceph osd crush class ls
ceph osd crush class ls-osd hdd
ceph osd crush class ls-osd ssd
```

If you haven't already set the device class (e.g. SATA is called hdd and NVMe is ssd or nvme), you can customize it:

```
ceph osd crush set-device-class hdd <osd-id>
ceph osd crush set-device-class ssd <osd-id>
```


### 3. Create Crush Rule for each Device Class.

```
ceph osd crush rule create-replicated sata-rule default host hdd
ceph osd crush rule create-replicated nvme-rule default host ssd
```

### 4. Create Pool and set Crush Rule

## Pool for SATA OSD
```
ceph osd pool create sata-pool 128 128
ceph osd pool set sata-pool crush_rule sata-rule
```


## Pool for NVMe/M.2 OSD
```
ceph osd pool create nvme-pool 128 128
ceph osd pool set nvme-pool crush_rule nvme-rule
```

#### 128 128 is the PG/PGP value that you can adjust according to the OSD number.


## Result:✅

- sata-pool will only use OSD that is SATA (class: hdd)
- nvme-pool will only use OSD that is M.2/NVMe (class: ssd)


# supplement:📢

In Proxmox VE GUI (Web UI), when you create a Ceph Pool, there is no option to "directly select" whether this pool will use SATA or NVMe OSD right away, but you can indirectly control it by selecting a CRUSH rule that is tied to the device class you pre-defined (e.g. hdd, ssd, nvme) in the previous step.


## Steps in GUI to bind Pool to SATA or NVMe

#### 1. Go to GUI:

Ceph > Pools

#### 2.Click "Create" to create a new Pool

#### 3.Name the Pool

e.g. sata-pool or nvme-pool

#### 4.In "Crush Rule" field, you will have options:

e.g. replicated_rule (default)

or sata-rule, nvme-rule (the one you created)

#### 5.Select the CRUSH Rule you want to use (bound to OSD class):

- If sata-rule is selected → Only hdd class OSD will be used

- If nvme-rule is selected → Only ssd or nvme class OSD will be used

#### Click Create





