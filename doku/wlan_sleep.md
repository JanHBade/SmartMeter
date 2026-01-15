Damit das WLAN nicht die Verbindung unterbricht

/etc/systemd/system/Ping.service

(liegt auch im ~ Verszeichnis, dann link dahin erstellen)
```
cat Ping
#!/bin/bash

while true ; do date ; ping -c1 192.168.42.1 ; sleep 360 ; done
```

```
cat Ping.service
[Unit]
Description=Ping
After=syslog.target

[Service]
Type=simple
User=pi
Group=pi
WorkingDirectory=/home/pi
ExecStart=/home/pi/Ping

[Install]
WantedBy=multi-user.target
```
