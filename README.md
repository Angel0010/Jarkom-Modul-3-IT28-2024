# Jarkom-Modul-3-IT28-2024

## Anggota Kelompok IT28

| Nama  | NRP | 
| ----------- | ----------- |
| Angella Christie | 5027221047 | 
| Ahmad Fauzan Daniswara | 5027221057 |

# Prefix IP 
`192.247`

# Configurasi 
Arakis (Router (DHCP RELAY))
```
auto eth1
iface eth1 inet static
address 192.247.1.0
netmask 255.255.255.0

auto eth2
iface eth2 inet static
address 192.247.2.0
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 192.247.3.0
netmask 255.255.255.0

auto eth4
iface eth4 inet static
address 192.247.4.0
netmask 255.255.255.0
```
Mohiam (DHCP Server)
```
auto eth0
iface eth0 inet static
address 192.247.3.3
netmask 255.255.255.0
gateway 192.247.3.0
```
Irulan (DNS Server)
```
auto eth0
iface eth0 inet static
address 192.247.3.2
netmask 255.255.255.0
gateway 192.247.3.0
```
Chani (Database Server)
```
auto eth0
iface eth0 inet static
address 192.247.4.3
netmask 255.255.255.0
gateway 192.247.4.0
```
Stilgar (Load Balancer)
```
auto eth0
iface eth0 inet static
address 192.247.4.2
netmask 255.255.255.0
gateway 192.247.4.0
```
Leto (Laravel Worker)
```
auto eth0
iface eth0 inet static
address 192.247.2.2
netmask 255.255.255.0
gateway 192.247.2.0
```
Duncan (Laravel Worker)
```
auto eth0
iface eth0 inet static
address 192.247.2.3
netmask 255.255.255.0
gateway 192.247.2.0
```
Jessica (Laravel Worker)
```
auto eth0
iface eth0 inet static
address 192.247.2.4
netmask 255.255.255.0
gateway 192.247.2.0
```
Vladimir (PHP Worker)
```
auto eth0
iface eth0 inet static
address 192.247.1.2
netmask 255.255.255.0
gateway 192.247.1.0
```
Rabban (PHP Worker)
```
auto eth0
iface eth0 inet static
address 192.247.1.3
netmask 255.255.255.0
gateway 192.247.1.0
```
Feyd (PHP Worker)
```
auto eth0
iface eth0 inet static
address 192.247.1.4
netmask 255.255.255.0
gateway 192.247.1.0
```
Dmitri & Paul (Client)
```
auto eth0
iface eth0 inet dhcp
```
## Topologi 
![Gambar WhatsApp 2024-05-17 pukul 18 57 22_6413b365](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/131789727/71a0cb45-e68c-4236-8ddc-2725eba1a195)

## Setup yang dilakukam sebelum pengerjaan soal shift 
