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

# List IP
| Node | Kategori | IP |
| --- | --- | --- |
| Arakis | Router (DHCP Relay) | DHCP |
| Mohiam | DHCP Server | 192.247.3.3 |
| Irulan | DNS Server | 192.247.3.2 |
| Chani | Database Server | 192.247.4.3 |
| Stilgar | Load Balancer | 192.247.4.2 |
| Leto | Laravel Worker | 192.247.2.2 |
| Duncan | Laravel Worker | 192.247.2.3 |
| Jessica | Laravel Worker | 192.247.2.4 |
| Vladimir | PHP Worker | 192.247.1.2 |
| Rabban | PHP Worker | 192.247.1.3 |
| Feyd | PHP Worker | 192.247.1.4 |
| Dmitri | Client | DHCP |
| Paul | Client | DHCP |

## Topologi 
![Gambar WhatsApp 2024-05-17 pukul 18 57 22_6413b365](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/131789727/71a0cb45-e68c-4236-8ddc-2725eba1a195)

## Setup yang dilakukam sebelum pengerjaan soal shift 
### Buatlah ```~/.bashrc``` pada setiap node 
Arakis (Router (DHCP Relay))
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.247.0.0/16
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt install isc-dhcp-relay -y
```
- Masukkan IP Mohiam (```192.247.3.3```) pada inputan DHCP relay should forward request to
- Masukkan ``` eth1 eth2 eth3 eth4``` pada inputan DHCP relay should listen on
- Pada file ``` /etc/sysctl.conf ```, uncomment script ``` net.ipv4.ip_forward=1 ```
- Lakukan ```service isc-dhcp-relay restart```
Mohiam (DHCP Server)
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf 
apt-get update 
apt-get install isc-dhcp-server 
service isc-dhcp-server status 
echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server 
```
- Buka file ```/etc/dhcp/dhcpd.conf``` dan ubah script menjadi seperti ini
  ```
  ddns-update-style none ;
	
	subnet 192.247.1.0 netmask 255.255.255.0 {
  }
  subnet 192.247.2.0 netmask 255.255.255.0 {
  }
  subnet 192.247.3.0 netmask 255.255.255.0 { 
  }
  subnet 192.247.4.0 netmask 255.255.255.0 {
  }
  ```
- Lakukan ``` service isc-dhcp-server start ```


