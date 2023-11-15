```
nano /root/.bashrc
```

AuraRouter (DHCP Relay)

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
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;
ddns-update-style none;

  subnet 10.63.3.0 netmask 255.255.255.0 {
    range 10.63.3.16 10.63.3.32;
    range 10.63.3.64 10.63.3.80;
    option routers 10.63.3.1;
    option broadcast-address 10.63.1.255;
    option domain-name-servers 10.63.3.2;
    option domain-name-servers 10.63.3.3;
    default-lease-time 300;
    max-lease-time 6900;

}
subnet 10.63.4.0 netmask 255.255.255.0 {
    range 10.63.4.12 10.63.4.20;
    range 10.63.4.160 10.63.4.168;
    option routers 10.63.4.1;
    option broadcast-address 10.63.2.255;
    option domain-name-servers 10.63.4.2;
    option domain-name-servers 10.63.4.3;
    default-lease-time 600;
    max-lease-time 6900;

}
    ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server stop
service isc-dhcp-server start

```
