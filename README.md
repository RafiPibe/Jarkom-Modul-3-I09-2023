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

<h2>Problem 4</h2>

Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

We need to add configuration like ```option broadcast address``` and ```option domain-name-server``` so that the DNS can be connected.

```

subnet 10.63.3.0 netmask 255.255.255.0 {
    ...
    option broadcast-address 10.63.3.255;
    option domain-name-servers 10.63.1.2;
    ...
}

subnet 10.63.4.0 netmask 255.255.255.0 {
    option broadcast-address 10.63.4.255;
    option domain-name-servers 10.63.1.2;
} 

```

and use ```shell``` script 

```

echo 'subnet 10.63.1.0 netmask 255.255.255.0 {
}

subnet 10.63.2.0 netmask 255.255.255.0 {
}

subnet 10.63.3.0 netmask 255.255.255.0 {
    range 10.63.3.16 10.63.3.32;
    range 10.63.3.64 10.63.3.80;
    option routers 10.63.3.0;
    option broadcast-address 10.63.3.255;
    option domain-name-servers 10.63.1.2;
}

subnet 10.63.4.0 netmask 255.255.255.0 {
    range 10.63.4.12 10.63.4.20;
    range 10.63.4.160 10.63.4.168;
    option routers 10.63.4.0;
    option broadcast-address 10.63.4.255;
    option domain-name-servers 10.63.1.2;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server start

```

then we need to setup the DHCP relay

```

echo '# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.63.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3 eth4"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""' > /etc/default/isc-dhcp-relay

service isc-dhcp-relay start 

```

After all the process, uncomment ```net.ipv4.ip_forward=1``` in the ```/etc/sysctl.conf``` file. 

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


<h2>Problem 6</h2>

Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3. (6)

First of all, we need to setup all PHP worker. Then, we need to download and unzip the google drive file by using ```wget``` and ```unzip```

Setup for PHP Worker
```
echo 'nameserver 10.63.1.2' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-cli php7.3-zip -y

service nginx start
service php7.3-fpm start

```

```
cp /etc/nginx/sites-available/default /etc/nginx/sites-available/granz.channel.i09.com
ln -s /etc/nginx/sites-available/granz.channel.i09.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/granz.channel.i09.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;  # Sesuaikan versi PHP dan socket
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/granz.channel.i09.com

service nginx restart

```

```
wget -O '/var/www/granz.channel.i09.com' 'https://drive.google.com/u/0/uc?id=1ViSkRq7SmwZgdK64eRbr5Fm1EGCTPrU1&export=download'
unzip -o /var/www/granz.channel.i09.com -d /var/www/
rm /var/www/granz.channel.i09.com
mv /var/www/modul-3 /var/www/granz.channel.i09.com

```

<h2>Problem 7</h2>

Kepala suku dari Bredt Region memberikan resource server sebagai berikut: Lawine, 4GB, 2vCPU, dan 80 GB SSD. Linie, 2GB, 2vCPU, dan 50 GB SSD. Lugner 1GB, 1vCPU, dan 25 GB SSD. Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.

To complete the following question, we need to do some setup first. After done setuping, we can configure the load balancing on Eisen node. 

Setup for Load Balancer
```
echo 'nameserver 10.63.1.2' > /etc/resolv.conf
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start

```

```

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.i09.com. root.riegel.canyon.i09.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.i09.com.
@       IN      A       10.63.2.2     ; IP LB Eiken
www     IN      CNAME   riegel.canyon.i09.com.' > /etc/bind/sites/riegel.canyon.i09.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.i09.com. root.granz.channel.i09.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.i09.com.
@       IN      A       10.63.2.2     ; IP LB Eiken
www     IN      CNAME   granz.channel.i09.com.' > /etc/bind/sites/granz.channel.i09.com

```


In the Eisen node,

```

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

echo ' upstream worker {
    server 10.63.3.1;
    server 10.63.3.2;
    server 10.63.3.3;
}

server {
    listen 80;
    server_name granz.channel.i09.com www.granz.channel.i09.com;

    root /var/www/html;

    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        proxy_pass http://worker;
    }
} ' > /etc/nginx/sites-available/lb_php

ln -s /etc/nginx/sites-available/lb_php /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

service nginx restart

```

<h2>Problem 9</h2>
Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.



<h2>Problem 10</h2>
Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/


Script

```
mkdir /etc/nginx/rahasisakita
htpasswd -c /etc/nginx/rahasisakita/htpasswd netics

```

Enter the password ```ajk09```

Then, add this command 

```
auth_basic "Restricted Content";
auth_basic_user_file /etc/nginx/rahasisakita/htpasswd;

```

<h2>Problem 11</h2>
Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. (11) hint: (proxy_pass)

Script

```

location ~ /its {
    proxy_pass https://www.its.ac.id;
    proxy_set_header Host www.its.ac.id;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}

```


```

echo 'upstream worker {
    server 10.63.3.1;
    server 10.63.3.2;
    server 10.63.3.3;
}

server {
    listen 80;
    server_name granz.channel.i09.com www.granz.channel.i09.com;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        proxy_pass http://worker;
    }

    location ~ /its {
        proxy_pass https://www.its.ac.id;
        proxy_set_header Host www.its.ac.id;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}' > /etc/nginx/sites-available/lb_php

```

What this means is that when we access an endpoint containing /its it will be directed by proxy_pass to https://www.its.ac.id. So when testing on the Revolte client, use the following command

```

lynx www.granz.channel.i09.com/its

```

<h2>Problem 12</h2>
Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168.

Script

```

location / {
    allow 10.63.3.69;
    allow 10.63.3.70;
    allow 10.63.4.167;
    allow 10.63.4.168;
    deny all;
    proxy_pass http://worker;
}

```


Here we only allow a few IPs according to the terms of the question and we reject all IPs other than those specified in the question. To do the testing, this can be done by opening a client that gets IP 10.63.3.69 or 10.63.3.70 or 10.63.4.167 or 10.63.4.168


<h2>Problem 13</h2>
Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern.


Script
```
 
echo '# This group is read both by the client and the server
# use it for options that affect everything
[client-server]

# Import all .cnf files from configuration directory
!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mariadb.conf.d/

# Options affecting the MySQL server (mysqld)
[mysqld]
skip-networking=0
skip-bind-address
' > /etc/mysql/my.cnf

```

Change the bind address on ```/etc/mysql/mariadb.conf.d/50-server.cnf``` to 0.0.0.0

```

cd /etc/mysql/mariadb.conf.d/50-server.cnf

# Changes
bind-address            = 0.0.0.0

```

Then, run this command


```

mysql -u root -p
Enter password: 

CREATE USER 'kelompoki09'@'%' IDENTIFIED BY 'passwordi09';
CREATE USER 'kelompoki09'@'localhost' IDENTIFIED BY 'passwordi09';
CREATE DATABASE dbkelompoki09;
GRANT ALL PRIVILEGES ON *.* TO 'kelompoki09'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompoki09'@'localhost';
FLUSH PRIVILEGES;

```

<h2>Problem 14</h2>
Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

Script

```

wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer

```

Install git and clone the resources that have been given in the question

```

apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update

```

Then, configure on all worker

```

cd /var/www/laravel-praktikum-jarkom && cp .env.example .env
echo 'APP_NAME=Laravel
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=10.63.2.1
DB_PORT=3306
DB_DATABASE=dbkelompoki09
DB_USERNAME=kelompoki09
DB_PASSWORD=passwordi09

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=file
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=smtp
MAIL_HOST=mailpit
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_HOST=
PUSHER_PORT=443
PUSHER_SCHEME=https
PUSHER_APP_CLUSTER=mt1

VITE_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
VITE_PUSHER_HOST="${PUSHER_HOST}"
VITE_PUSHER_PORT="${PUSHER_PORT}"
VITE_PUSHER_SCHEME="${PUSHER_SCHEME}"
VITE_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"' > /var/www/laravel-praktikum-jarkom/.env
cd /var/www/laravel-praktikum-jarkom && php artisan key:generate
cd /var/www/laravel-praktikum-jarkom && php artisan config:cache
cd /var/www/laravel-praktikum-jarkom && php artisan migrate
cd /var/www/laravel-praktikum-jarkom && php artisan db:seed
cd /var/www/laravel-praktikum-jarkom && php artisan storage:link
cd /var/www/laravel-praktikum-jarkom && php artisan jwt:secret
cd /var/www/laravel-praktikum-jarkom && php artisan config:clear
chown -R www-data.www-data /var/www/laravel-praktikum-jarkom/storage

```

Configure the nginx into the following port

```

10.63.4.1:8001; # Fern 
10.63.4.2:8002; # Flamme
10.63.4.3:8003; # Frieren

```


<h2>Problem 15</h2>
Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. Untuk POST /api/auth/register


To work on this problem. It is necessary to carry out testing using Apache Benchmark on only one worker. Here we will use the Laravel Fern worker which will later become the worker that will be tested by the Revolte client. Before testing, we use the help of a .json file which will be used as the body which will be sent to the /api/auth/register endpoint later as follows


Script

```

echo '
{
  "username": "kelompoki09",
  "password": "passwordi09"
}' > register.json

```

Do this following command on ```Revolte```

```

ab -n 100 -c 10 -p register.json -T application/json http://10.63.4.1:8001/api/auth/register

```

<h2>Problem 16</h2>
Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. Untuk POST /api/auth/login

The method is the same as before, but we need to change ```login``` instead of ```register```

```

echo '
{
  "username": "kelompoki09",
  "password": "passwordi09"
}' > login.json

```

```

ab -n 100 -c 10 -p login.json -T application/json http://10.63.4.1:8001/api/auth/login

```

<h2>Problem 17</h2>
Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. Untuk GET /api/me


Script

Get the token before accessing the endpoint ```/api/me```

```

curl -X POST -H "Content-Type: application/json" -d @login.json http://10.63.4.1:8001/api/auth/login > login_output.txt

```

Set the token to global

```

token=$(cat login_output.txt | jq -r '.token')

```

To test, run this following command

```

ab -n 100 -c 10 -H "Authorization: Bearer $token" http://10.63.4.1:8001/api/me

```

<h2>Problem 18</h2>
Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

Script

```

echo 'upstream worker {
    server 10.63.4.1:8001;
    server 10.63.4.2:8002;
    server 10.63.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.i09.com www.riegel.canyon.i09.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-available/laravel-worker

ln -s /etc/nginx/sites-available/laravel-worker /etc/nginx/sites-enabled/laravel-worker

service nginx restart

```

<h2>Problem 19</h2>
Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan -> pm.max_children, pm.start_servers, pm.min_spare_servers, pm.max_spare_servers sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

To work on this question, there are several explanations as follows

```pm.max_children``` Specifies the maximum number of PHP workers (child processes) that can run simultaneously. This value should be adjusted to the server resource capacity. If it is too low, the server may not be able to handle many requests simultaneously, while if it is too high, it may cause overload and resource shortages.

```pm.start_servers``` Specifies the number of PHP workers that will be started automatically when PHP-FPM is first started or restarted. This helps in optimizing performance when the server is first started.

```pm.min_spare_servers``` Specifies the minimum number of PHP workers to keep running while the server is running. This helps keep the server responsive to requests even when traffic is low.

```pm.max_spare_servers``` Specifies the maximum number of PHP workers that can run but not handle requests. This number is scaled to the need to handle traffic spikes without adding too many resources when load is low.

There will be 4 configurations for the package manager process for each worker which will later be carried out for testing.


Script

Script 1

```

# Setup Awal
echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 5
pm.start_servers = 2
pm.min_spare_servers = 1
pm.max_spare_servers = 3' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

```

Script 2

```

echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 25
pm.start_servers = 5
pm.min_spare_servers = 3
pm.max_spare_servers = 10' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

```

Script 3

```

echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 50
pm.start_servers = 8
pm.min_spare_servers = 5
pm.max_spare_servers = 15' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

```

Script 4

```

echo '[www]
user = www-data
group = www-data
listen = /run/php/php8.0-fpm.sock
listen.owner = www-data
listen.group = www-data
php_admin_value[disable_functions] = exec,passthru,shell_exec,system
php_admin_flag[allow_url_fopen] = off

; Choose how the process manager will control the number of child processes.

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20' > /etc/php/8.0/fpm/pool.d/www.conf

service php8.0-fpm restart

```

<h2>Problem 20</h2>
Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

Because the process has been previously configured for each worker, specifically in the package manager, and it turns out that the results provided are not enough to improve worker performance. Therefore, an algorithm was added to the load balancer using Least-connection where this algorithm will prioritize the distribution of the lowest performance load. The master node will record all the load and performance of all nodes, and will prioritize the lowest load. So it is hoped that there will be no servers with low load.


Script

```

echo 'upstream worker {
    least_conn;
    server 10.63.4.1:8001;
    server 10.63.4.2:8002;
    server 10.63.4.3:8003;
}

server {
    listen 80;
    server_name riegel.canyon.i09.com www.riegel.canyon.i09.com;

    location / {
        proxy_pass http://worker;
    }
} 
' > /etc/nginx/sites-available/laravel-worker

service nginx restart

```

The setup that we use

```

pm = dynamic
pm.max_children = 75
pm.start_servers = 10
pm.min_spare_servers = 5
pm.max_spare_servers = 20

```
