```
nano /root/.bashrc
```

AuraRouter (DHCP Relay)

```bash
apt-get update
apt-get install isc-dhcp-relay -y

echo '
  SERVERS="10.63.1.2"
  INTERFACES="eth1 eth2 eth3 eth4"
  OPTIONS=
    ' > /etc/default/isc-dhcp-relay

echo '
  net.ipv4.ip_forward=1
    ' > /etc/sysctl.conf

service isc-dhcp-relay start
```

HimmelDHCPServer

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
apt-get update -y
apt-get install isc-dhcp-server -y
dhcpd --version

echo '
  INTERFACES="eth0"
    ' > /etc/default/isc-dhcp-server

echo '
  subnet 10.63.1.0 netmask 255.255.255.0 {
}

  subnet 10.63.3.0 netmask 255.255.255.0 {
    range 10.63.3.16 10.63.3.32;
    range 10.63.3.64 10.63.3.80;
    option routers 10.63.3.1;
    option broadcast-address 10.63.3.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 600;
    max-lease-time 7200;
}

  subnet 10.63.4.0 netmask 255.255.255.0 {
    range 10.63.4.12 10.63.4.20;
    range 10.63.4.160 10.63.4.168;
    option routers 10.63.4.1;
    option broadcast-address 10.63.4.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 600;
    max-lease-time 7200;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server stop
service isc-dhcp-server start
service isc-dhcp-server status

```
