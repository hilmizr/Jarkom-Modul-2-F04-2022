# Lapres Modul 2 Jaringan Komputer

# Anggota
- Hilmi Zharfan Rachmadi - 5025201268
- Ida Bagus Kade Rainata Putra Wibawa - 5025201235
- Naufal Faadhilah - 5025201221

# Jawaban
## Soal 1
### Topologi dan Konfigurasi

<img src="https://user-images.githubusercontent.com/70790033/198050565-af3d2f5d-5348-483c-a49a-aa6f75676b6b.png" width="800">

#### Konfigurasi Ostania
```bash
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.201.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.201.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.201.3.1
	netmask 255.255.255.0
```

#### Konfigurasi WISE
```
auto eth0
iface eth0 inet static
	address 192.201.3.2
	netmask 255.255.255.0
	gateway 192.201.3.1
```

#### Konfigurasi SSS
```
auto eth0
iface eth0 inet static
	address 192.201.1.2
	netmask 255.255.255.0
	gateway 192.201.1.1
```

#### Konfigurasi Garden
```
auto eth0
iface eth0 inet static
	address 192.201.1.3
	netmask 255.255.255.0
	gateway 192.201.1.1
```

#### Konfigurasi Berlint
```
auto eth0
iface eth0 inet static
	address 192.201.2.2
	netmask 255.255.255.0
	gateway 192.201.2.1
```

#### Konfigurasi Eden
```
auto eth0
iface eth0 inet static
	address 192.201.2.3
	netmask 255.255.255.0
	gateway 192.201.2.1
```

### Command

#### Di Ostania
Jalankan command:
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.201.0.0/16
cat /etc/resolv.conf
```
Agar lebih cepat, kami tuliskan command di atas ke dalam script 01.sh melalui nano. Untuk menjalankannya tinggal gunakan bash. 
```bash
nano /root/01.sh
bash /root/01.sh
```
<img src="https://user-images.githubusercontent.com/70790033/198056706-74493265-a6a5-4aa4-8e17-28e7894e6485.png" width="800">


#### Di Selain Ostania (WISE, SSS, Garden, Berlint, Eden)
Jalankan command:
```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Hasilnya sudah bisa ```ping google.com``` dari semua node

#### Dari Ostania
<img src="https://user-images.githubusercontent.com/70790033/198053635-a2ddf5a1-af98-486a-8521-f50171f6d87d.png" width="800">

#### Dari WISE
<img src="https://user-images.githubusercontent.com/70790033/198053848-63233617-364d-4135-a4b5-8a79a9d6bba9.png" width="800">

#### Dari SSS
<img src="https://user-images.githubusercontent.com/70790033/198054671-bac5bda6-97da-4ea9-81a3-392577065691.png" width="800">

#### Dari Garden
<img src="https://user-images.githubusercontent.com/70790033/198054927-3c9a624f-c0c5-4b00-95d1-4250ed84a8f9.png" width="800">

#### Dari Berlint
<img src="https://user-images.githubusercontent.com/70790033/198054098-232de5e8-c4ae-43ce-ae1c-8deadcf5097b.png" width="800">

#### Dari Eden
<img src="https://user-images.githubusercontent.com/70790033/198054404-5e636001-a6f5-4b54-a731-f178be629b5c.png" width="800">

Agar membuat lebih efisien, command di atas, beserta ```ping google.com```, juga kami masukkan ke dalam script 01.sh di root WISE, SSS, Garden, Berlint, Eden. 

<img src="https://user-images.githubusercontent.com/70790033/198057154-065a2ae8-d058-4030-854d-98cc015fbef7.png" width="800">

## Soal 2

#### Instalasi Bind

Instalasi Bind dilakukan di WISE dengan command
```bash
 apt-get update
 apt-get install bind9 -y
 ```
 Command di atas kami masukkan ke dalam 02_install.sh di root WISE
 
 <img src="https://user-images.githubusercontent.com/70790033/198058564-fcd300b7-c1f9-430d-836c-70e9e2431a00.png" width="800">
  
 Instalasi Bind sudah berhasil
 
  <img src="https://user-images.githubusercontent.com/70790033/198059117-f94d3dd8-7c4c-44f7-89bf-fac46021f2bd.png" width="800">
  
  <img src="https://user-images.githubusercontent.com/70790033/198059321-15ba55bf-6c5c-4edd-806f-368f44d51a7f.png" width="800">
  
## Soal 3

### Subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden

1. Edit file /etc/bind/wise/wise.f04.com pada WISE
2. Tambahkan subdomain untuk eden.wise.f04.com yang mengarah ke IP Eden

```bash
nano /etc/bind/wise/wise.f04.com
```

3. Tambahkan konfigurasi berikut

```bash
eden   IN      A       192.201.2.3
www.eden IN    CNAME   eden.wise.f04.com.
```

4. Restart service bind

```bash
service bin9 restart
```

5. Ping ke subdomain dengan perintah berikut (dari client)

```bash
ping eden.wise.f04.com -c 5

ATAU

host -t A eden.wise.f04.com
```

## Soal 4

### Reverse Domain untuk Domain Utama

1. Edit file /etc/bind/named.conf.local pada WISE

```bash
nano /etc/bind/named.conf.local
```

2. Tambahkan konfigurasi berikut ke named.conf.local, lalu tambahkan reverse dari 3 byte awal IP yang ingin di-reverse. Karena IPnya adalah 192.201.2, reversenya adalah 2.201.192

```bash
zone "2.201.192.in-addr.arpa" {
    type master;
    file "/etc/bind/wise/2.201.192.in-addr.arpa";
};
```

3. Copykan file db.local ke /etc/bind ke dalam folder wise yang baru dibuat dan ubah namanya menjadi 2.201.192.in-addr.arpa, lalu restart bind9

```bash
cp /etc/bind/db.local /etc/bind/wise/2.201.192.in-addr.arpa

service bind9 restart
```

4. Memastikan konfigurasi sudah benar dengan perintah (pada client)

```bash
apt-get update
apt-get install dnsutils
```

5. Kembalikan namserver di /etc/resolv.conf dengan IP WISE lalu cek dengan command

```bash
host -t PTR 192.201.2.2
```

## Soal 5

### Berlint Sebagai DNS Slave Untuk Domain Utama

1. Konfigurasi pada server WISE dengan meng-edit file /etc/bind/named.conf.local. Sesuaikan dengan syntax berikut

```bash
zone "wise.f04.com" {
    type master;
    notify yes;
    also-notify { 192.201.3.2; }; // IP Berlint 
    allow-transfer { 192.201.3.2; }; // IP Berlint 
    file "/etc/bind/wise/wise.f04.com";
};
```

2. Restart bind9

```bash
service bind9 restart
```

3. Masuk ke tahap konfigurasi server Berlint. Buka berlint lalu update package lists dan install aplikasi bind9 dengan menjalankan command

```bash
apt-get update
apt-get install bind9 -y
```

4. Buka file /etc/bind/named.conf.local pada Berlint dan tambahkan syntax berikut dan restart bind9

```bash
zone "wise.f04.com" {
    type slave;
    masters { 192.201.2.2; }; // Masukan IP WISE
    file "/var/lib/bind/wise.f04.com";
};

service bind9 restart
```

5. Testing ser WISE dengan mematikan service bind9

```bash
service bind9 stop
```
Pada client pastikan pengaturan nameserver mengarah ke IP WISE dan IP Berlint.
Lakukan ping ke wise.f04.com pada client. Jika ping berhasil maka konfigurasi DNS slave telah berhasil

## Soal 6
### Subdomain Khusus Untuk Operation operation.wise.yyy.com 

1. 

```bash
nano /etc/bind/wise/wise.f04.com
Kemudian edit file /etc/bind/named.conf.options pada WISE.
nano /etc/bind/named.conf.options
```

2. Edit file /etc/bind/named.conf.options pada WISE

```bash
nano /etc/bind/named.conf.options
```

3. Comment dnssec-validation auto dan tambahkan baris berikut pada /etc/bind/named.conf.options

```bash
allow-query{any;};
```

4. Edit file /etc/bind/named.conf.local menjadi seperti gambar di bawah dan restart bind9


```bash
zone "wise.f04.com" { type master; file "/etc/bind/wise/wise.f04.com"; 
allow-transfer { 192.200.3.2; }; // Masukan IP Berlint 
};

service bind9 restart
```

## Soal 7

### Subdomain melalui Berlint dengan akses strix.operation.wise.yyy.com yang mengarah ke Eden

1. Instalasi bind

```bash
apt-get update
apt-get install bind9 -y
```

2. Edit folder operation.wise.f04.com

```bash
nano /etc/bind/operation/operation.wise.f04.com
```

3. Restart bind 9 dan ping ke domain strix.operation.wise.f04.com

```bash
service bind9 restart
```

## Soal 8

### Konfigurasi Web Server 


## Soal 9

### Url www.wise.yyy.com/index.php/home menjadi www.wise.yyy.com/home


## Soal 10

### Penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com


## Soal 11

### folder /public hanya dapat melakukan directory listing saja


## Soal 12

### Error 404.html pada folder /error


## Soal 13

### Konfigurasi virtual host


## Soal 14

### www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500


## Soal 15

### Autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy 


## Soal 16 

### Akses otomatis ke www.wise.yyy.com (akses IP Eden)


## Soal 17

### Mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.pn


# Kendala
## Insiden Docker
Sekitar pukul 13.00, tiba-tiba GNS3 saya (Hilmi) tidak bisa tersambung dengan Docker yang menyebabkan progress kelompok kami tersendat.

<img src="https://user-images.githubusercontent.com/70790033/198060051-431c5ab1-2707-4459-827a-347ec8492801.png" width="800">

Alhamdulillah, saya sudah mengekspor file dengan topologi, beserta konfigurasi dan scriptnya. Saya kemudian mengirim file tersebut ke grup LINE kelompok. Progress dilanjutkan oleh Naufal Faadhilah yang memulai project lagi from scratch. Oleh karena itu, terdapat dua file project di repository kelompok kami. 
 
