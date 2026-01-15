Feste IP auf eth0 einstellen

```
nmcli device status
nmcli connection show

nmcli connection modify <Namen der Verbindung> ipv4.addresses 192.168.1.42/24
nmcli connection modify <Namen der Verbindung> ipv4.method manual

nmcli connection down <Namen der Verbindung>
nmcli connection up <Namen der Verbindung>
```
