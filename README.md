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
Masukkan code berikut ke dalam terminal file irulan!
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
Untuk hasilnya dapat dilihat pada foto berikut!
![Screenshot 2024-05-21 204601](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/0c716a16-057a-47cd-b6b4-7e2dba8c6dba)

## Soal 2
Kemudian untuk soal nomor 2, masukkan code di terminal file irulan juga.
```
echo 'subnet 192.247.1.0 netmask 255.255.255.0 {
    range 192.247.1.14 192.247.1.28;
    range 192.247.1.49 192.247.1.70;
    option routers 192.247.1.1;
}

subnet 192.247.3.0 netmask 255.255.255.0 {
}

subnet 192.247.4.0 netmask 255.255.255.0 {

}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server start
```
Untuk hasilnya dapat dilihat pada foto berikut!
![Screenshot 2024-05-21 205524](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/895d4bdd-976d-4f5f-af13-3447188f00f3)
![Screenshot 2024-05-21 205525](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/0a9457e6-1e11-4b7e-a7b3-b6f2721b583e)

## Soal 3-5
Untuk soal selanjutnya dikerjakan di terminal mohiam dengan script sebagai berikut! 
```
apt-get update
apt install isc-dhcp-server -y

echo "INTERFACESv4=\"eth0\"
INTERFACESv6=\"\"" > /etc/default/isc-dhcp-server


echo "subnet 192.247.1.0 netmask 255.255.255.0 {
    range 192.247.1.14 192.247.1.28;
    range 192.247.1.49 192.247.1.70;         // Range IP Client House Harkonen
    option routers 192.247.1.1;
    option broadcast-address 192.247.1.255;
    option domain-name-servers 192.247.3.2;
    default-lease-time 300;
    max-lease-time 5220;  // Durasi DHCP Server yang melalui House Harkonen
}

subnet 192.247.2.0 netmask 255.255.255.0 {
    range 192.247.2.15 192.247.2.25;
    range 192.247.2.200 192.247.2.210;       // // Range IP Client House Atreides
    option routers 192.247.2.1;
    option broadcast-address 192.247.2.255;
    option domain-name-servers 192.247.3.2;
    default-lease-time 1200;
    max-lease-time 5220;   // Durasi DHCP Server yang melalui House Atreides

subnet 192.247.3.0 netmask 255.255.255.0 {
    option routers 192.247.3.1;
    option broadcast-address 192.247.3.255;
}

subnet 192.247.4.0 netmask 255.255.255.0 {
    option routers 192.247.4.1;
    option broadcast-address 192.247.4.255;
}" >/etc/dhcp/dhcpd.conf

rm -f /var/run/dhcpd.pid
service isc-dhcp-server restart
service isc-dhcp-server status
```
Untuk hasilnya google bisa di ping melalui client Dmitri dengan IP Irulan.
![Screenshot 2024-05-21 210611](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/6acd9054-f99a-47a3-8922-a4dd32a92a4c)

## Soal 6
Soal nomor 6 dikerjakan pada PHP Worker yaitu di Node Vladimir, Rabban, dan Feyd. Sehingga, masukkan script berikut kedalam masing-masing file di tiap node PHP Worker di terminal tiap PHP Worker!
* Vladimir
```
echo 'nameserver 192.247.3.2' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-$apt update
apt install wget

service nginx start
service php7.3-fpm start

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1lmnXJUbyx1JDt2OA5z_1dEowxozfkn30' -O /var/www/harkonen.com
unzip -o /var/www/harkonen.com -d /var/www/
rm /var/www/harkonen.com
mv /var/www/modul-3 /var/www/harkonen.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/harkonen.com
ln -s /etc/nginx/sites-available/harkonen.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/harkonen.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/harkonen.com

service nginx restart
  ```

* Rabban
```
echo 'nameserver 192.247.3.2' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-$apt update
apt install wget

service nginx start
service php7.3-fpm start

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1lmnXJUbyx1JDt2OA5z_1dEowxozfkn30' -O /var/www/harkonen.com
unzip -o /var/www/harkonen.com -d /var/www/
rm /var/www/harkonen.com
mv /var/www/modul-3 /var/www/harkonen.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/harkonen.com
ln -s /etc/nginx/sites-available/harkonen.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/harkonen.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/harkonen.com

service nginx restart
```

* Feyd
```
echo 'nameserver 192.247.3.2' > /etc/resolv.conf
apt-get update
apt-get install nginx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install lynx -y
apt-get install htop -y
apt-get install apache2-utils -y
apt-get install php7.3-fpm php7.3-common php7.3-mysql php7.3-gmp php7.3-curl php7.3-intl php7.3-mbstring php7.3-xmlrpc php7.3-gd php7.3-xml php7.3-$apt update
apt install wget

service nginx start
service php7.3-fpm start

wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1lmnXJUbyx1JDt2OA5z_1dEowxozfkn30' -O /var/www/harkonen.com
unzip -o /var/www/harkonen.com -d /var/www/
rm /var/www/harkonen.com
mv /var/www/modul-3 /var/www/harkonen.com

cp /etc/nginx/sites-available/default /etc/nginx/sites-available/harkonen.com
ln -s /etc/nginx/sites-available/harkonen.com /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default

echo 'server {
    listen 80;
    server_name _;

    root /var/www/harkonen.com;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}' > /etc/nginx/sites-available/harkonen.com

service nginx restart
```
Kemudian jalankan dengan command `lynx localhost` pada masing-masing terminal PHP Worker! lalu berikut adalah dokumentasi hasilnya.
* Vladimir
  ![vladimir](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/d605793a-26bd-41e9-9054-7095e9657ad5)

* Rabban
  ![rabban](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/da76cf4e-9471-4209-b467-9c3cc074d7a7)

* Feys
  ![feys](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/e04bc218-15d6-4844-9571-ac230d53a7d6)

## Soal 7
Untuk soal 7, praktikan diminta agar Stilgar dari fremen dapat dapat bekerja sama dengan maksimal, lalu lakukan testing dengan 5000 request dan 150 request/second. Berikut adalah langkah-langkahnya:
1. Kembali ke terminal irulan dan buka file scriptnya yang tadi telah dikerjakan
2. Masukkan script tambahan berikut!
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     atreides.it28.com. root.atreides.it28.com. (
                         2024051601     ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      atreides.it28.com.
@       IN      A       192.247.4.3 ; Stilgar' > /etc/bind/atreides/atreides.it28.com


echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     harkonen.it28.com. root.harkonen.it28.com. (
                     2024051601         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      harkonen.it28.com.
@       IN      A       192.247.4.3 ; Stilgar' > /etc/bind/harkonen/harkonen.it28.com

```
3. Lalu kembali jalankan filenya dan kemudian masuk ke terminal stilgar
4. Kemudian masukkan script baru pada file stilgarnya sebagai berikut:
```
echo 'nameserver 192.247.3.2' > /etc/resolv.conf

apt-get update
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start


cp /etc/nginx/sites-available/default /etc/nginx/sites-available/lb_php

   echo ' upstream worker {
    server 192.247.1.2;
    server 192.247.1.3;
    server 192.247.1.4;
}

server {
    listen 80;
    server_name harkonen.it28.com www.harkonen.it28.com;

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
5. Kemudian jalankan filenya dan masuk ke node Dmitri
6. Untuk menjalankan soal nomor 7, gunakan command  `ab -n 5000 -c 150 http://192.247.4.3/` dan berikut adalah hasilnya:
![dmitri](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/f3504a94-5703-4797-963f-1834916b3f65)
Request per Second yang diterima dari pengujian adalah sebesar  1390.72 [#/sec] (mean).

## Soal 8
Untuk soal 8, praktikan diminta untuk menuliskan peta tercepat menuju spice. Sehingga praktikan diminta untuk membuat analisis hasil testing dengan 500 request dan 50 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- Nama Algoritma Load Balancer
- Report hasil testing pada Apache Benchmark
- Grafik request per second untuk masing masing algoritma. 
- Analisis (8).
  
Berikut adalah langkah-langkahnya:
1. Kembali masuk ke terminal stilgar
2. Masuk kembali ke file stilgar yang sudah dikerjakan tadi dan tambahkan script-script berikut!
```
upstream roundrobin_worker {
    server 192.247.1.2;
    server 192.247.1.3;
    server 192.247.1.4;
}

upstream leastconn_worker {
    least_conn;
    server 192.247.1.2;
    server 192.247.1.3;
    server 192.247.1.4;
}

upstream weightedrr_worker {
    server 192.247.1.2 weight=3;
    server 192.247.1.3 weight=2;
    server 192.247.1.4 weight=1;
}

upstream iphash_worker {
    ip_hash;
    server 192.247.1.2;
    server 192.247.1.3;
    server 192.247.1.4;
}
upstream hash_worker {
    hash $request_uri consistent;
    server 192.247.1.2;
    server 192.247.1.3;
    server 192.247.1.4;
}

```

```
location / {
        proxy_pass http://worker;
        auth_basic "Restricted Content";
        auth_basic_user_file /etc/nginx/supersecret/htpasswd;
    }
    location /roundrobin {
        proxy_pass http://roundrobin_worker;
    }
    location /leastconn {
        proxy_pass http://leastconn_worker;
    }
    location /weightedrr {
        proxy_pass http://weightedrr_worker;
    }
    location /iphash {
        proxy_pass http://iphash_worker;
    }
    location /hash {
        proxy_pass http://hash_worker;
    }
} 

```
Dua script diatas berguna untuk memasukkan algoritma load balancer agar bisa diuji pada client 

3. Jalankan kembali filenya kemudian kembali ke terminal client
4. Untuk menjalankannya, gunakan command `ab -n 5000 -c 150 http://192.247.4.3/wkwkwk/` dengan wkwkwk dengan diganti nama algoritma load balancer yang ingin diuji, contoh round robin.

Berikut hasil dari masing masing algoritma load balancer yang diuji pada salah satu client:
1. Round Robin
   ![roundrobin](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/8b92a828-34ba-4e2a-ae85-d234b511e06d)
   Hasil Request per Second = 267.31 [#/sec] (mean)

2. Least Connection
   ![leastconnection](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/c3f6777a-be7f-4d68-bfcf-af03b63e8b36)
   Hasil Request per Second = 274.13 [#/sec] (mean)

3. Weighted Round Robin
   ![weightedroundrobin](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/5cf9a8d9-bff8-4a64-a57d-08b4f2186fed)
    Hasil Request per Second =  291.57 [#/sec] (mean)

4. IP Hash
   ![iphash](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/d5600669-61f4-4a87-947a-3d162bc86210)
   Hasil Request per Second = 311.14 [#/sec] (mean)

5. Generic Hash
   ![generichash](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/ac18fbd0-4c1e-4b32-9878-d4bea3a3d5b3)
    Hasil Request per Second = 250.67 [#/sec] (mean)

   Dan berikut adalah grafik dari masing-masing algoritma yang telah diuji:
   ![output](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/8cabd7d6-bf10-4172-948a-e67f961e89ac).
   
   Jadi, IP Hash adalah algoritma dengan Request per Second tertinggi sehingga algoritma ini yang paling tepat untuk menerima request yang berat.



## Soal 13
Untuk soal 13, praktikan diminta untuk mengatur para pekerja House atreides di atreides.yyy.com. Lalu mengatur semua data yang diperlukan pada Chani dan harus dapat diakses oleh Leto, Duncan, dan Jessica.
Berikut adalah script jawaban soal nomor 13, dikerjakan pada terminal Chani!
```
# Db akan diakses oleh 3 worker, maka 
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
cd /etc/mysql/mariadb.conf.d/50-server.cnf

# Changes
bind-address            = 0.0.0.0
service mysql restart

read -sp 'Enter MySQL root password: ' MYSQL_ROOT_PASSWORD
echo

# Execute MySQL commands
mysql -u root -p"$MYSQL_ROOT_PASSWORD" <<EOF

CREATE USER 'kelompokit28'@'%' IDENTIFIED BY 'passwordit28';
CREATE USER 'kelompokit28'@'localhost' IDENTIFIED BY 'passwordit28';
CREATE DATABASE dbkelompokit28;
GRANT ALL PRIVILEGES ON *.* TO 'kelompokit28'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'kelompokit28'@'localhost';
FLUSH PRIVILEGES;
EOF

echo “dbkelompokit28 created”

```
Dan berikut output yang dihasilkan:
![Screenshot 2024-05-21 214955](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/6a777432-7d9c-447e-8780-989dfa7513c9)

Dan untuk menguji di Laravel Workernya, gunakan command `mysql -h 192.247.4.3 -P 3306 -u kelompokit28 -p`.

Berikut adalah dokumentasi hasil pengujiannya pada masing-masing Laravel Worker:
* Leto
  ![Screenshot 2024-05-21 215010](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/2efad859-7f91-4937-9dcf-94ffa3024079)

* Duncan
  ![Screenshot 2024-05-21 215024](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/f26132fc-f094-4ee8-a7e8-232580fecc7c)

* Jessica
  ![Screenshot 2024-05-21 215031](https://github.com/Angel0010/Jarkom-Modul-3-IT28-2024/assets/124378481/ad83ef2d-08b5-476c-befc-f97e06bf5c9a)


## Soal 14
Untuk soal 14, praktikan diminta untuk melakukan instalasi PHP8.0 dan Composer. 
Berikut adalah script dari jawaban soal nomor 14

* script14.sh
```
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer

apt-get install git -y
cd /var/www && git clone https://github.com/martuafernando/laravel-praktikum-jarkom
cd /var/www/laravel-praktikum-jarkom && composer update

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
DB_HOST=192.247.4.2
DB_PORT=3306
DB_DATABASE=dbkelompokit28
DB_USERNAME=kelompokit28
DB_PASSWORD=passwordit28

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

* Leto
```
Nano laravel.sh
echo 'server {
    listen 8001;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }

    error_log /var/log/nginx/leto_error.log;
    access_log /var/log/nginx/leto_access.log;
}' > /etc/nginx/sites-available/laravel-leto

ln -s /etc/nginx/sites-available/laravel-leto /etc/nginx/sites-enabled/

# Test the Nginx configuration for syntax errors
nginx -t

# If the configuration test is successful, restart Nginx using the service command
if [ $? -eq 0 ]; then
    service nginx restart
else
    echo "Nginx configuration test failed. Please check the error messages above."
fi
```

* Duncan
```
Nano laraveldun.sh
echo 'server {
    listen 8002;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }

    error_log /var/log/nginx/duncan_error.log;
    access_log /var/log/nginx/duncan_access.log;
}' > /etc/nginx/sites-available/laravel-duncan

ln -s /etc/nginx/sites-available/laravel-duncan /etc/nginx/sites-enabled/

# Test the Nginx configuration for syntax errors
nginx -t

# If the configuration test is successful, restart Nginx using the service command
if [ $? -eq 0 ]; then
    service nginx restart
else
    echo "Nginx configuration test failed. Please check the error messages above."
fi
```

* Jessica
```
Nano laraveljes.sh
echo 'server {
    listen 8003;

    root /var/www/laravel-praktikum-jarkom/public;

    index index.php index.html index.htm;
    server_name _;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # pass PHP scripts to FastCGI server
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }

    error_log /var/log/nginx/jessica_error.log;
    access_log /var/log/nginx/jessica_access.log;
}' > /etc/nginx/sites-available/laravel-jessica
ln -s /etc/nginx/sites-available/laravel-jessica /etc/nginx/sites-enabled/

# Test the Nginx configuration for syntax errors
nginx -t

# If the configuration test is successful, restart Nginx using the service command
if [ $? -eq 0 ]; then
    service nginx restart
else
    echo "Nginx configuration test failed. Please check the error messages above."
fi
```
Untuk Output masih error sehingga tidak ditampilkan.

## Kendala
   - Soal 9-12, 14(output)-20 belum terselesaikan

### Sekian Laporan Resmi Modul 3 Jarkom Kelompok IT28





