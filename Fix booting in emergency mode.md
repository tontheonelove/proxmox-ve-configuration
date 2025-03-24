# Fix booting in emergency mode (Physical Server)
```
nano nano /etc/fstab
```

```
insert  # front #UUID=XXXXXXXX /boot/efi vfat defaults 0 1
```
