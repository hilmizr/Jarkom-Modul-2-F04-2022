# Lapres Modul 2 Jaringan Komputer

## Anggota

- Hilmi Zharfan Rachmadi - 5025201268
- Ida Bagus Kade Rainata Putra Wibawa - 5025201235
- Naufal Faadhilah - 5025201221

## Pembagian Tugas
- Hilmi Zharfan Rachmadi
  - Topologi & Konfigurasi
  - Nomor 1-5
- Ida Bagus Kade Rainata Putra Wibawa & Naufal Faadhilah
  - Revisi & Lapres

## Jawaban

### Soal 1

#### Topologi dan Konfigurasi

![Topologi](https://user-images.githubusercontent.com/70790033/198050565-af3d2f5d-5348-483c-a49a-aa6f75676b6b.png)

#### Konfigurasi Ostania

```conf
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

```conf
auto eth0
iface eth0 inet static
        address 192.201.3.2
        netmask 255.255.255.0
        gateway 192.201.3.1
```

#### Konfigurasi SSS

```conf
auto eth0
iface eth0 inet static
        address 192.201.1.2
        netmask 255.255.255.0
        gateway 192.201.1.1
```

#### Konfigurasi Garden

```conf
auto eth0
iface eth0 inet static
        address 192.201.1.3
        netmask 255.255.255.0
        gateway 192.201.1.1
```

#### Konfigurasi Berlint

```conf
auto eth0
iface eth0 inet static
        address 192.201.2.2
        netmask 255.255.255.0
        gateway 192.201.2.1
```

#### Konfigurasi Eden

```conf
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

![Konfigurasi di Ostania](https://user-images.githubusercontent.com/70790033/198056706-74493265-a6a5-4aa4-8e17-28e7894e6485.png)

#### Di Selain Ostania (WISE, SSS, Garden, Berlint, Eden)

Jalankan command:

```bash
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Hasilnya sudah bisa ```ping google.com``` dari semua node

#### Dari Ostania

![Konfigurasi dari Ostania](https://user-images.githubusercontent.com/70790033/198053635-a2ddf5a1-af98-486a-8521-f50171f6d87d.png)

#### Dari WISE

![Konfigurasi dari WISE](https://user-images.githubusercontent.com/70790033/198053848-63233617-364d-4135-a4b5-8a79a9d6bba9.png)

#### Dari SSS

![Konfigurasi dari SSS](https://user-images.githubusercontent.com/70790033/198054671-bac5bda6-97da-4ea9-81a3-392577065691.png)

#### Dari Garden

![Konfigurasi dari Garden](https://user-images.githubusercontent.com/70790033/198054927-3c9a624f-c0c5-4b00-95d1-4250ed84a8f9.png)

#### Dari Berlint

![Konfigurasi dari Berlint](https://user-images.githubusercontent.com/70790033/198054098-232de5e8-c4ae-43ce-ae1c-8deadcf5097b.png)

#### Dari Eden

![Konfigurasi dari Eden](https://user-images.githubusercontent.com/70790033/198054404-5e636001-a6f5-4b54-a731-f178be629b5c.png)

Agar membuat lebih efisien, command di atas, beserta ```ping google.com```, juga kami masukkan ke dalam script 01.sh di root WISE, SSS, Garden, Berlint, Eden.

![Script no 1](https://user-images.githubusercontent.com/70790033/198057154-065a2ae8-d058-4030-854d-98cc015fbef7.png)

### Soal 2

#### Instalasi Bind

Instalasi Bind dilakukan di WISE dengan command

```bash
apt-get update
apt-get install bind9 -y
```

Command di atas kami masukkan ke dalam 02_install.sh di root WISE

![Script install bind](https://user-images.githubusercontent.com/70790033/198058564-fcd300b7-c1f9-430d-836c-70e9e2431a00.png)

Instalasi Bind sudah berhasil

![Instalasi bind 1](https://user-images.githubusercontent.com/70790033/198059117-f94d3dd8-7c4c-44f7-89bf-fac46021f2bd.png)

![Instalasi bind 2](https://user-images.githubusercontent.com/70790033/198059321-15ba55bf-6c5c-4edd-806f-368f44d51a7f.png)
  
### Soal 3

#### Subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden

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
    ```

    atau

    ```bash
    host -t A eden.wise.f04.com
    ```

### Soal 4

#### Reverse Domain untuk Domain Utama

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
    host -t PTR 192.201.3.2
    ```

### Soal 5

#### Berlint Sebagai DNS Slave Untuk Domain Utama

1. Konfigurasi pada server WISE dengan meng-edit file /etc/bind/named.conf.local. Sesuaikan dengan syntax berikut

    ```bash
    zone "wise.f04.com" {
        type master;
        notify yes;
        also-notify { 192.201.2.2; }; // IP Berlint 
        allow-transfer { 192.201.2.2; }; // IP Berlint 
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
        masters { 192.201.3.2; }; // Masukan IP WISE
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

## Kendala

### Insiden Docker

Sekitar pukul 13.00, tiba-tiba GNS3 saya (Hilmi) tidak bisa tersambung dengan Docker yang menyebabkan progress kelompok kami tersendat.

![Docker tidak tersambung](https://user-images.githubusercontent.com/70790033/198060051-431c5ab1-2707-4459-827a-347ec8492801.png)

Alhamdulillah, saya sudah mengekspor file dengan topologi, beserta konfigurasi dan scriptnya. Saya kemudian mengirim file tersebut ke grup LINE kelompok. Progress dilanjutkan oleh Naufal Faadhilah yang memulai project lagi from scratch. Oleh karena itu, terdapat dua file project di repository kelompok kami.

### Ketidak-compatible perangkat laptop saat menginstall virtualbox

Teman saya Ida Bagus Kade Rainata Putra tidak mampu mengunduh virtualbox dan sejenisnya di perangkat kerasnya. Hal ini tentunya menghambat pengerjaan praktikum ini.
Adapun perangkatnya ialah: Mackbook M1.

### Bertepatan dengan ETS

Teman saya Naufal Faadhilah mendapati pada pekan perkuliahan minggu ke-9 terdapat 3 mata kuliah yang mengadakan ETS, sehingga tidak dapat membantu banyak dalam progress praktikum Jarkom kali ini.
