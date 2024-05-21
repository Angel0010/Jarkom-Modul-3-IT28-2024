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
Irulan (DNS Server)
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf

echo ‘
options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    };

    // dnssec-validation auto;
    allow-query { any; };
    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};
‘ > /etc/bind/named.conf.options
apt-get update
apt-get install bind9 -y 
```
Chani (Database Server)
```
echo nameserver 192.247.3.2 > /etc/resolv.conf

apt-get update
apt-get install mariadb-server -y
service mysql star
```
- Lalu jangan lupa untuk mengganti [bind-address] pada file /etc/mysql/mariadb.conf.d/50-server.cnf menjadi 0.0.0.0 dan jangan lupa untuk melakukan restart mysql kembali 
Stilgar (Load Balancer)
```
echo nameserver 192.247.3.2 > /etc/resolv.conf
apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start
```
Vladimir, Rabban, Feyd (PHP Worker)
```
echo nameserver 192.247.3.2 > /etc/resolv.conf

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
Leto, Duncan, Jessica (Laravel Worker)
```
echo nameserver 192.247.3.2 > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y
# Test connection from worker to database
# mariadb --host=192.247.2.1 --port=3306   --user=kelompokit28 --password=passwordit28 dbkelompokit28 -e "SHOW DATABASES;"
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli   php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl unzip wget -y
apt-get install nginx -y

service nginx start
service php8.0-fpm start
```
Dmitri, Paul (Client)
```
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

## Soal 0-1
Masukkan code berikut ke dalam file irulan
```
echo 'zone "harkonen.it28.com" {
    type master;
    file "/etc/bind/sites/harkonen.it28.com";
};
zone "atreides.it28.com" {
    type master;
    file "/etc/bind/sites/atreides.it28.com";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/harkonen.it28.com
cp /etc/bind/db.local /etc/bind/sites/atreides.it28.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     harkonen.it28.com. root.harkonen.it28.com. (
                        2023281401      ; Serial
                        604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                        604800 )       ; Negative Cache TTL
;
@       IN      NS      harkonen.it28.com.
@       IN      A       192.247.1.2    ; IP Vladimir
www     IN      CNAME   harkonen.it28.com.' > /etc/bind/sites/harkonen.it28.com



echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     atreides.it28.com. root.atreides.it28.com. (
                              2023281401         ; Serial
                              604800              ; Refresh
                              86400              ; Retry
                            2419200              ; Expire
                              604800 )            ; Negative Cache TTL
;
@       IN      NS      atreides.it28.com.
@       IN      A       192.247.2.2    ; IP Leto
www     IN      CNAME   atreides.it28.com.' > /etc/bind/sites/atreides.it28.com

echo 'options {
    directory "/var/cache/bind";

    forwarders {
        192.168.122.1;
    };

    // dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;    # conform to RFC1035
    listen-on-v6 { any; };
};' > /etc/bind/named.conf.options


service bind9 restart
```
Untuk hasilnya dapat dilihat pada foto berikut
![Screenshot 2024-05-21 204601](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/0c716a16-057a-47cd-b6b4-7e2dba8c6dba)
