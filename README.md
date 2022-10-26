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
### Instalasi Bind
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

# Kendala
## Insiden Docker
Sekitar pukul 13.00, tiba-tiba GNS3 saya (Hilmi) tidak bisa tersambung dengan Docker yang menyebabkan progress kelompok kami tersendat.

<img src="https://user-images.githubusercontent.com/70790033/198060051-431c5ab1-2707-4459-827a-347ec8492801.png" width="800">

Alhamdulillah, saya sudah mengekspor file dengan topologi, beserta konfigurasi dan scriptnya. Saya kemudian mengirim file tersebut ke grup LINE kelompok. Progress dilanjutkan oleh Naufal Faadhilah yang memulai project lagi from scratch. Oleh karena itu, terdapat dua file project di repository kelompok kami. 
 
