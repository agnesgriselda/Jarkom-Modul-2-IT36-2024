# Jarkom-Modul-2-IT36-2024

## Laporan Resmi Modul 3 : DHCP dan Reverse Proxy

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
17. [Soal 15](#soal-15)
18. [Soal 16](#soal-16)
19. [Soal 17](#soal-17)
20. [Soal 18](#soal-18)
21. [Soal 19](#soal-19)
22. [Soal 20](#soal-20)

## Topologi

![Screenshot 2024-10-21 191835](https://github.com/user-attachments/assets/e6780704-ccde-433f-a80a-bed814dad746)

## Setup

### Paradis (DHCP Relay)
```
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.81.1.0
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.81.2.0
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.81.3.0
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.81.4.0
	netmask 255.255.255.0
```

### Fritz (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.81.4.2
	netmask 255.255.255.0
	gateway 10.81.4.0
```

### Tybur (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.81.4.3
	netmask 255.255.255.0
	gateway 10.81.4.0
```

### Warhammer (Database Server)
```
auto eth0
iface eth0 inet static
	address 10.81.3.4
	netmask 255.255.255.0
	gateway 10.81.3.0
```

### Beast (Load Balancer Laravel)
```
auto eth0
iface eth0 inet static
	address 10.81.3.2
	netmask 255.255.255.0
	gateway 10.81.3.0
```

### Colossal (Load Balancer PHP)
```
auto eth0
iface eth0 inet static
	address 10.81.3.3
	netmask 255.255.255.0
	gateway 10.81.3.0
```

### Annie (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.81.1.2
	netmask 255.255.255.0
	gateway 10.81.1.0
```

### Bertholdt (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.81.1.3
	netmask 255.255.255.0
	gateway 10.81.1.0
```

### Reiner (Laravel Worker)
```
auto eth0
iface eth0 inet static
	address 10.81.1.4
	netmask 255.255.255.0
	gateway 10.81.1.0
```

### Armin (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.81.2.2
	netmask 255.255.255.0
	gateway 10.81.2.0
```

### Eren (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.81.2.3
	netmask 255.255.255.0
	gateway 10.81.2.0
```

### Mikasa (PHP Worker)
```
auto eth0
iface eth0 inet static
	address 10.81.2.4
	netmask 255.255.255.0
	gateway 10.81.2.0
```

### Zeke, Erwin (Client) 
```
auto eth0
iface eth0 inet dhcp
```


