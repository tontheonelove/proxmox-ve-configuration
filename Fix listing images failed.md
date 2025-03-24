# Fix listing images failed/ (2) No such file or directory (500)

```
#rbd ls -l [cephpool]  (default)

#rbd -p [cephpool] list  (for missing vm list)

#rbd rm vm-[id]-disk-[id] -p [cephpool]
```
