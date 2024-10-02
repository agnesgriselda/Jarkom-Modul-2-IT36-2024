# Jarkom-Modul-2-IT36-2024

## Laporan Resmi Modul 2 : DNS dan Webserver

### IT36

| Nama                        | NRP           |
|-----------------------------|---------------|
| Fico Simhanandi                | 5027231030   |
| Agnes Zenobia Griselda Petrina | 5027231034    |


## Daftar Isi

1. [Topologi](#topologi)
2. [Setup](#setup)
3. [Soal 1](#soal-1)
4. [Soal 2](#soal-2)
5. [Soal 3](#soal-3)
6. [Soal 4](#soal-4)
7. [Soal 5](#soal-5)
8. [Soal 6](#soal-6)
9. [Soal 7](#soal-7)
10. [Soal 8](#soal-8)
11. [Soal 9](#soal-9)
12. [Soal 10](#soal-10)
13. [Soal 11](#soal-11)
14. [Soal 12](#soal-12)
15. [Soal 13](#soal-13)
16. [Soal 14](#soal-14)

## Topologi
![Screenshot 2024-10-01 005906](https://github.com/user-attachments/assets/175b87a0-f6e8-40ea-95c3-c48d6ea81f78)

## Setup
### Nusantara (Router)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
   address 10.81.1.1
   netmask 255.255.255.0

auto eth2
iface eth2 inet static
   address 10.81.2.1
   netmask 255.255.255.0

auto eth3
iface eth3 inet static
   address 10.81.3.1
   netmask 255.255.255.0
```
### Sanjaya (Client)
```
auto eth0
iface eth0 inet static
   address 10.81.1.2
   netmask 255.255.255.0
   gateway 10.81.1.1
```

### Sriwijaya (DNS Master)
```
auto eth0
iface eth0 inet static
   address 10.81.1.3
   netmask 255.255.255.0
   gateway 10.81.1.1
```

### Tanjungkulai (Web server)
```
auto eth0
iface eth0 inet static
   address 10.81.1.4
   netmask 255.255.255.0
   gateway 10.81.1.1
```

### Bedahulu (Web server)
```
auto eth0
iface eth0 inet static
   address 10.81.1.5
   netmask 255.255.255.0
   gateway 10.81.1.1
```

### Majapahit (DNS Slave)
```
auto eth0
iface eth0 inet static
   address 10.81.2.2
   netmask 255.255.255.0
   gateway 10.81.2.1
```

### Anusapati (Client)
```
auto eth0
iface eth0 inet static
   address 10.81.2.3
   netmask 255.255.255.0
   gateway 10.81.2.1
```

### Kotalingga (Web server)
```
auto eth0
iface eth0 inet static
   address 10.81.2.4
   netmask 255.255.255.0
   gateway 10.81.2.1
```

### Jayanegara (Client)
```
auto eth0
iface eth0 inet static
   address 10.81.2.5
   netmask 255.255.255.0
   gateway 10.81.2.1
```

### Solok (Load Balancer)
```
auto eth0
iface eth0 inet static
   address 10.81.3.2
   netmask 255.255.255.0
   gateway 10.81.3.1
```

Masukkan command berikut ke `nano /root/.bashrc` untuk menghubungkan Nusantara ke internet.
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.81.0.0/16
```
Tambahkan command berikut pada DNS Master & Slave `nano /root/.bashrc`, untuk instalasi bind9.
```
apt-get update
apt-get install bind9 -y
```

Hubungkan Sriwijaya (DNS Master) dengan router menggunakan command :
```
echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf
```
pada `nano /root/.bashrc`

Hubungkan Majapahit (DNS Slave) dengan router menggunakan command :
```
echo ‘nameserver 192.168.122.1
nameserver 10.81.1.3’ > /etc/resolv.conf
```
pada `nano /root/.bashrc`


## Soal 1
Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave.

### Setup Client
`nano /etc/resolv.conf` pada semua client dan tambahkan command :
```
nameserver 192.168.122.1
```

### Testing
```
ping google -c 5
```
![Screenshot 2024-10-03 000405](https://github.com/user-attachments/assets/8db0f1fd-0532-4caa-874a-100c5cbbe19d)

![Screenshot 2024-10-03 000520](https://github.com/user-attachments/assets/4e0a93f1-8e5a-4baa-aa2a-67a1defa1217)

![Screenshot 2024-10-03 000622](https://github.com/user-attachments/assets/ae670222-788a-4853-b535-a20876ac10dc)


![Screenshot 2024-10-03 000736](https://github.com/user-attachments/assets/c3e0fec5-433e-43db-bc50-76ba918e6f10)

## Soal 2
Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

### Sriwijaya
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "sudarsana.it36.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it36.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it36.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it36.com. root.sudarsana.it36.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it36.com.
@       IN      A       10.81.3.2     ; IP Solok
www     IN      CNAME   sudarsana.it36.com.' > /etc/bind/jarkom/sudarsana.it36.com

service bind9 restart
```

Pastikan semua client terhubung dengan DNS Slave `nano /etc/resolv.conf`
```
nameserver 10.81.1.3
```

### Testing
```
ping sudarsana.it36.com
ping www.sudarsana.it36.com
```
![Screenshot 2024-10-02 191338](https://github.com/user-attachments/assets/96a5cddb-5c89-42f5-b0a3-051ba3a65f80)

![Screenshot 2024-10-02 193223](https://github.com/user-attachments/assets/95bf3363-29a8-455f-ba14-eb8c4c1c2641)

## Soal 3
Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

### Sriwijaya
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "pasopati.it36.com" {
	type master;
	file "/etc/bind/jarkom/pasopati.it36.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/pasopati.it36.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it36.com. root.pasopati.it36.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it36.com.
@       IN      A       10.81.2.4     ; IP Kotalingga
www     IN      CNAME   pasopati.it36.com.' > /etc/bind/jarkom/pasopati.it36.com

service bind9 restart
```

### Testing
```
ping pasopati.it36.com
ping www.pasopati.it36.com
```
![Screenshot 2024-10-02 191432](https://github.com/user-attachments/assets/d1eee887-27cf-4613-8a64-893208587b70)

## Soal 4
Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

### Sriwijaya
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "rujapala.it36.com" {
	type master;
	file "/etc/bind/jarkom/rujapala.it36.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/rujapala.it36.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it36.com. root.rujapala.it36.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it36.com.
@       IN      A       10.81.1.4     ; IP Tanjungkulai
www     IN      CNAME   rujapala.it36.com.' > /etc/bind/jarkom/rujapala.it36.com

service bind9 restart
```
### Testing

![Screenshot 2024-10-02 191536](https://github.com/user-attachments/assets/1c6a7d21-c7be-46df-aa3e-65ba0dd2183f)

## Soal 5
Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.

### Testing

![Screenshot 2024-10-02 191338](https://github.com/user-attachments/assets/7923bec8-274e-4bca-ae6c-1f1807c7d517)

![Screenshot 2024-10-02 191432](https://github.com/user-attachments/assets/61b0cbc5-a8ec-433e-8e07-eab0459b1a77)

![Screenshot 2024-10-02 191536](https://github.com/user-attachments/assets/179e6317-806a-4f82-94f0-9afd44e50b56)


## Soal 6
Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

## Soal 7
Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

## Soal 8
Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

### Sriwijaya
```
#!/bin/bash
apt update
apt install bind9 -y

echo 'zone "sudarsana.it36.com" {
	type master;
	file "/etc/bind/jarkom/sudarsana.it36.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/sudarsana.it36.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it36.com. root.sudarsana.it36.com. (
                        2024100104      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it36.com.
@       IN      A       10.81.3.2     ; IP Solok
www     IN      CNAME   sudarsana.it36.com.
cakra       IN      A       10.81.1.5     ; IP Bedahulu
www     IN      CNAME   cakra.sudarsana.it36.com.' > /etc/bind/jarkom/sudarsana.it36.com


service bind9 restart
```

### Testing
```
ping cakra.sudarsana.it36.com
ping www.cakra.sudarsana.it36.com
```
![Screenshot 2024-10-02 193223](https://github.com/user-attachments/assets/6a34e5d7-5923-4f5a-87b8-4f26d8a50160)


## Soal 9
Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

### Sriwijaya
```
!/bin/bash


echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it36.com. root.pasopati.it36.com. (
                        2                   ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it36.com.
@       IN      A       10.81.2.4     ; IP Kotalingga
www     IN      CNAME   pasopati.it36.com.
panah   IN      A       10.81.2.2     ; Delegasikan ke Majapahit
ns1     IN      A       10.81.2.2     ; Delegasikan ke Majapahit
panah   IN      NS      ns1' > /etc/bind/jarkom/pasopati.it36.com

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

### Majapahit
```
#!/bin/bash

echo '
options {
        directory "/var/cache/bind";

        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

echo 'zone "panah.pasopati.it36.com" {
	type master;
	file "/etc/bind/panah/panah.pasopati.it36.com";
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/panah

cp /etc/bind/db.local /etc/bind/panah/panah.pasopati.it36.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it36.com. root.panah.pasopati.it36.com. (
                        2                    ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it36.com.
@       IN      A       10.81.2.2     ; IP Majapahit
www.panah     IN      CNAME   panah.pasopati.it36.com.' > /etc/bind/panah/panah.pasopati.it36.com

service bind9 restart
```
Pastikan semua client terhubung dengan DNS Slave `nano /etc/resolv.conf`
```
nameserver 10.81.2.2
```

### Testing

![Screenshot 2024-10-02 202726](https://github.com/user-attachments/assets/a6052de9-d873-4474-bd97-6045ccce497b)

![Screenshot 2024-10-02 203000](https://github.com/user-attachments/assets/c120be9f-7b21-4f06-ac49-3f0e91e120dc)

![Screenshot 2024-10-02 203036](https://github.com/user-attachments/assets/1a6c6a18-a181-40be-a17a-7efb68a4b719)

![Screenshot 2024-10-02 203408](https://github.com/user-attachments/assets/197b1f47-3589-4fa5-8e6a-302e4c52922f)

![Screenshot 2024-10-02 203436](https://github.com/user-attachments/assets/fbb14dd4-1e3c-4258-941a-8a72aadbe0b4)

![Screenshot 2024-10-02 203513](https://github.com/user-attachments/assets/7e67d5e0-dbc5-4666-96a0-45d255b45a8c)


## Soal 10
Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.

### Majapahit
```
#!/bin/bash

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it36.com. root.panah.pasopati.it36.com. (
                                     2      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it36.com.
@       IN      A       10.81.2.4     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it36.com.
log     IN      A       10.81.2.4     ; IP Kotalingga
www.log IN      CNAME   panah.pasopati.it36.com.' > /etc/bind/panah/panah.pasopati.it36.com

service bind9 restart
```

### Testing

![Screenshot 2024-10-02 204106](https://github.com/user-attachments/assets/7d22875d-fc68-4eaf-b48c-3e9c85e711de)

![Screenshot 2024-10-02 204133](https://github.com/user-attachments/assets/170804fa-2c22-4d72-8a4f-671c325a07b0)


## Soal 11
Setelah pertempuran mereda, warga IT dapat kembali mengakses jaringan luar dan menikmati meme brainrot terbaru, tetapi hanya warga Majapahit saja yang dapat mengakses jaringan luar secara langsung. Buatlah konfigurasi agar warga IT yang berada diluar Majapahit dapat mengakses jaringan luar melalui DNS Server Majapahit.

### Sriwijaya
```
#!/bin/bash

echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1; // IP Nusantara
        };
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

### Majapahit
```
#!/bin/bash

echo '
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1; // IP Nusantara
        };
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

### Testing

![Screenshot 2024-10-02 204954](https://github.com/user-attachments/assets/8f11a509-ded5-457b-a1f0-a00825d6dded)

![Screenshot 2024-10-02 205045](https://github.com/user-attachments/assets/36efa377-e19b-4806-a3a9-153a5c513854)


## Soal 12
Karena pusat ingin sebuah laman web yang ingin digunakan untuk memantau kondisi kota lainnya maka deploy laman web ini (cek resource yg lb) pada Kotalingga menggunakan apache.

### Sriwijaya & Majapahit
```
#!/bin/bash

# tambahkan nameserver IP Kotalingga
echo '
nameserver 192.168.122.1
nameserver 10.81.2.4' > /etc/resolv.conf
```

### Sanjaya, Jayanegara, Anusapati
```
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Lynx belum terinstal, melakukan instalasi..."
    # Instalasi lynx
    apt-get update
    apt-get install lynx -y
else
    echo "lynx sudah terinstal."
fi
```

### Kotalingga
```
(Kotalingga)
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    # Instalasi unzip
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

unzip lb.zip

rm -rf /var/www/html/index.php

cp worker/index.php /var/www/html/index.php

service apache2 restart
```

### Testing
```
lynx http://10.81.2.4/index.php
```

![Screenshot 2024-10-02 213243](https://github.com/user-attachments/assets/e78ab0de-9228-4acb-a3b1-ab32351f1e1a)


## Soal 13
Karena Sriwijaya dan Majapahit memenangkan pertempuran ini dan memiliki banyak uang dari hasil penjarahan (sebanyak 35 juta, belum dipotong pajak) maka pusat meminta kita memasang load balancer untuk membagikan uangnya pada web nya, dengan Kotalingga, Bedahulu, Tanjungkulai sebagai worker dan Solok sebagai Load Balancer menggunakan apache sebagai web server nya dan load balancer nya.

### Sriwijaya & Majapahit
```
#!/bin/bash

# tambahkan nameserver IP Solok
echo '
nameserver 192.168.122.1
nameserver 10.81.3.2' > /etc/resolv.conf
```

### Kotalingga, Bedahulu, Tanjungkulai
```
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "Unzip belum terinstal, melakukan instalasi..."
    # Instalasi unzip
    apt-get update
    apt-get install unzip -y
else
    echo "unzip sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

curl -L -o lb.zip --insecure "https://drive.google.com/uc?export=download&id=1Sqf0TIiybYyUp5nyab4twy9svkgq8bi7"

unzip lb.zip

rm -rf /var/www/html/index.php

cp worker/index.php /var/www/html/index.php

service apache2 restart
```

### Solok
```
#!/bin/bash

if ! command -v named &> /dev/null
then
    echo "Apache2 belum terinstal, melakukan instalasi..."
    # Instalasi apache2
    apt-get update
    apt-get install apache2 -y
    apt-get install libapache2-mod-php7.0 -y
else
    echo "apache2 sudah terinstal."
fi

if ! command -v named &> /dev/null
then
    echo "PHP belum terinstal, melakukan instalasi..."
    # Melakukan instalasi php
    apt-get update
    apt-get install php -y
else
    echo "php sudah terinstal."
fi

# Enable apache2 module
a2enmod proxy_balancer
a2enmod proxy_http
a2enmod lbmethod_byrequests
echo '
<VirtualHost *:80>
    <Proxy balancer://serverpool>
        BalancerMember http://10.81.1.4/
        BalancerMember http://10.81.1.5/
        BalancerMember http://10.81.2.4/
        Proxyset lbmethod=byrequests
    </Proxy>
    ProxyPass / balancer://serverpool/
    ProxyPassReverse / balancer://serverpool/
</VirtualHost>' > /etc/apache2/sites-available/000-default.conf

service apache2 restart
```

### Testing
```
lynx http://10.81.3.2/index.php
```


https://github.com/user-attachments/assets/d62b7870-34cb-4548-9d26-99b4f8165a88



## Soal 14
Selama melakukan penjarahan mereka melihat bagaimana web server luar negeri, hal ini membuat mereka iri, dengki, sirik dan ingin flexing sehingga meminta agar web server dan load balancer nya diubah menjadi nginx.
