## Fix The power button is broken.

### Error like This...
```````
Oct 06 11:43:19 node systemd-logind[1002]:New seat seat0.
Oct 06 11:43:19 node systemd-logind[1002]: Watching system buttons on /dev/input/event0 (Power Button)
Oct 06 11:43:19 node systemd[1]: Started systemd-logind.service - User Login Management.
Oct 06 11:44:28 node systemd-logind[1002]: New session 1 of user root.
Oct 06 11:45:11 node systemd-logind[1002]: Session 1 logged out. Waiting for processes to exit.
Oct 06 11:45:11 node systemd-logind[1002]: Removed session 1.
Oct 06 11:48:14 node systemd-logind[1002]: Power key pressed short.
Oct 06 11:48:14 node systemd-logind[1002]: Powering off...
Oct 06 11:48:14 node systemd-logind[1002]: System is powering down.
```````


## FIX
```````
#nano /etc/systemd/logind.conf
-----------------------
HandlePowerKey=ignore

#systemctl restart systemd-logind
```````





