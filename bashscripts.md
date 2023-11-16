```
nano /root/.bashrc
```

AuraRouter (DHCP Relay)

```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.63.0.0/16

echo '
  nameserver 192.168.122.1
    ' > /etc/resolv.conf

cat /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

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
  INTERFACESv4="eth0";
    ' > /etc/default/isc-dhcp-server

echo '
option domain-name "example.org;
option domain-name-servers ns1.example.org, ns2.example.org;
ddns-update-style none;

  subnet 10.63.1.0 netmask 255.255.255.0 {
}

  subnet 10.63.3.0 netmask 255.255.255.0 {
    range 10.63.3.16 10.63.3.32;
    range 10.63.3.64 10.63.3.80;
    option routers 10.63.3.1;
    option broadcast-address 10.63.3.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 180;
    max-lease-time 5760;
}

  subnet 10.63.4.0 netmask 255.255.255.0 {
    range 10.63.4.12 10.63.4.20;
    range 10.63.4.160 10.63.4.168;
    option routers 10.63.4.1;
    option broadcast-address 10.63.4.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 720;
    max-lease-time 5760;
} ' > /etc/dhcp/dhcpd.conf

rm -r /var/run/dhcpd.pid

service isc-dhcp-server stop
service isc-dhcp-server start
service isc-dhcp-server restart
service isc-dhcp-server status

```

HeiterDNSServer

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
apt-get update -y
apt install bind9 -y
service bind9 status

echo '
  zone "riegel.canyon.i09.com" {
    type master;
    file "/etc/bind/riegel.canyon.i09.com";
}; ' > /etc/bind/named.conf.local

#zone "1.63.10.in-addr.arpa" {
#    type master;
#    file "/etc/bind/jarkom/1.63.10.in-addr.arpa";
#};' > /etc/bind/named.conf.local

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.i09.com. root.riegel.canyon.i09.com. (
                        2023110101    ; Serial
                        604800        ; Refresh
                        86400         ; Retry
                        2419200       ; Expire
                        604800 )      ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.i09.com.
@               IN      A       10.63.1.3 ;
www             IN      CNAME   riegel.canyon.i09.com. > /etc/bind/jarkom/riegel.canyon.i09.com

echo '
  forwarders {
  192.168.122.1; //IP Aura
};
  //dnssec-validation auto;
  allow-query{any;}; > /etc/bind/named.conf.options

service bind9 restart

```
