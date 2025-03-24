# Fix booting in emergency mode
```
nano nano /etc/fstab
```

```
insert  # front #UUID=XXXXXXXX /boot/efi vfat defaults 0 1
```
