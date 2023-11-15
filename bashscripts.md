```
nano /root/.bashrc
```

DHCP Relay

```bash
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.63.0.0/16

echo '
  SERVERS="10.63.1.2"
  INTERFACES="eth1 eth2 eth3 eth4"
  OPTIONS=
    ' > /etc/default/isc-dhcp-relay

echo '
  net.ipv4.ip_forward=1
    ' > /etc/sysctl.conf

service isc-dhcp-relay restart
```
