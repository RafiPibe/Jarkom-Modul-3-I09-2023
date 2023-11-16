<h1>Jarkom-Modul-3-I09-2023</h1>
<p>Praktikum Jaringan Komputer Modul 3 I09</p>
<p>Computer Network Practicum 3rd Module I09</p>

| Name                        | NRP        |
|-----------------------------|------------|
|Eric Azka Nugroho            | 5025211064 |
|Faraihan Rafi Adityawarman   | 5025211074 |

<h2>Prerequisites</h2>

Since we are using a new Docker Image, we have to change the new one first. This is done by adding a new Docker container template, and filling the new docker image name with `danielcristh0/debian-buster:1.1`.

<p align="center">
<img src="https://media.discordapp.net/attachments/1153305482438660178/1173612928054870036/image.png?ex=65649736&is=65522236&hm=d61e20dbe2710f57cbd87426734b1c89586f49ffa0036fc8a8a388544fe4a312&=&width=1200&height=701">
</p>
<p align="center">The Topology</p>

<h2>Configs</h2>
Router Network Config

```bash
auto eth0
iface eth0 inet dhcp
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.63.0.0/16

auto eth1
iface eth1 inet static
	address 10.63.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.63.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.63.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.63.4.1
	netmask 255.255.255.0
```

<h3>Switch1 Group (10.63.1.1)</h3>

HimmelDHCPServer
```bash
auto eth0
iface eth0 inet static
	address 10.63.1.2
	netmask 255.255.255.0
	gateway 10.63.1.1
```

HeiterDNSServer
```bash
auto eth0
iface eth0 inet static
	address 10.63.1.3
	netmask 255.255.255.0
	gateway 10.63.1.1
```

<h3>Switch2 Group (10.63.2.1)</h3>

DenkenDatabaseServer
```bash
auto eth0
iface eth0 inet static
	address 10.63.2.2
	netmask 255.255.255.0
	gateway 10.63.1.1
```

EisenLoadBalancer
```bash
auto eth0
iface eth0 inet static
	address 10.63.2.3
	netmask 255.255.255.0
	gateway 10.63.1.1
```

<h3>Switch3 Group</h3>

RevolteClient
```bash
auto eth0
iface eth0 inet dhcp
```

RichterClient
```bash
auto eth0
iface eth0 inet dhcp
```

LawlinePHPWorker
```bash
auto eth0
iface eth0 inet static
	address 10.63.3.4
	netmask 255.255.255.0
	gateway 10.63.1.1
```

LiniePHPWorker
```bash
auto eth0
iface eth0 inet static
	address 10.63.3.5
	netmask 255.255.255.0
	gateway 10.63.1.1
```

LognerPHPWorker
```bash
auto eth0
iface eth0 inet static
	address 10.63.3.6
	netmask 255.255.255.0
	gateway 10.63.1.1
```

<h3>Switch4 Group</h3>

SeinClient
```bash
auto eth0
iface eth0 inet dhcp
```

StarkClient
```bash
auto eth0
iface eth0 inet dhcp
```

FrierenLaravelWorker
```bash
auto eth0
iface eth0 inet static
	address 10.63.4.4
	netmask 255.255.255.0
	gateway 10.63.1.1
```

FlammeLaravelWorker
```bash
auto eth0
iface eth0 inet static
	address 10.63.4.5
	netmask 255.255.255.0
	gateway 10.63.1.1
```

FernLaravelWorker
```bash
auto eth0
iface eth0 inet static
	address 10.63.4.6
	netmask 255.255.255.0
	gateway 10.63.1.1
```

<h2>Problem 2</h2>

Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)


To get the IP range for Switch 3, we need to change its range in the subnet


```
subnet 10.63.3.0 netmask 255.255.255.0 {
    range 10.63.3.16 10.63.3.32;
    range 10.63.3.64 10.63.3.80;
    option routers 10.63.3.1;
    option broadcast-address 10.63.3.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 180;
    max-lease-time 5760;

```

Above, it can be seen that the subnet represent the Switch3. In the range, it can be seen that the IP has already been changed into [prefix ip].3.16 untuk [prefix ip].3.32. It is also seen from the second range that the IP has already been changed into 3.64 up until 3.80.


<h2>Problem 3</h2>

Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 

Same as problem 2, we need to change the IP range by changing its subnet

```
 subnet 10.63.4.0 netmask 255.255.255.0 {
    range 10.63.4.12 10.63.4.20;
    range 10.63.4.160 10.63.4.168;
    option routers 10.63.4.1;
    option broadcast-address 10.63.4.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 720;
    max-lease-time 5760;

```

Like problem 2, the first range has been changed into .4.12 until .4.20 and the second range has been changed into .4.160 until .4.168


<h2>Problem 5</h2>

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

In this problem, we were tasked to change switch3 to 3 minutes and switch 4 to 12 minutes with the maximum allocated for IP address lending for 96 minutes.

```
subnet 10.63.3.0 netmask 255.255.255.0 {
    range 10.63.3.16 10.63.3.32;
    range 10.63.3.64 10.63.3.80;
    option routers 10.63.3.1;
    option broadcast-address 10.63.3.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 180;
    max-lease-time 5760;

```

It can be seen from Switch3 that the default lease time is 180 which is equivalent to 3 minutes. 

```
 subnet 10.63.4.0 netmask 255.255.255.0 {
    range 10.63.4.12 10.63.4.20;
    range 10.63.4.160 10.63.4.168;
    option routers 10.63.4.1;
    option broadcast-address 10.63.4.255;
    option domain-name-servers 10.63.1.3;
    default-lease-time 720;
    max-lease-time 5760;

```

In switch4 the default lease time has been changed into 720 which is equivalent to 12 minutes. 


Both switch has the maximum lease time of 5760 which is equivalent to 96 minutes. 


