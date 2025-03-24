# Using Custom NTP Servers

`````
nano /etc/chrony/chrony.conf:
`````

add your custom 
`````
server ntp1.example.com iburst
server ntp2.example.com iburst
server ntp3.example.com iburst
`````
`````
systemctl restart chronyd
`````
