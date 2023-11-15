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
