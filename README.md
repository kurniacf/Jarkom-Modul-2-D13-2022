# Praktikum Jaringan Komputer Modul 2 Kelompok D13

# Jarkom-Modul-2-D13-2022

| **No** | **Nama**                  | **NRP**    |
| ------ | ------------------------- | ---------- |
| 1      | Marcellino Mahesa Janitra | 5025201105 |
| 2      | Kurnia Cahya Febryanto    | 5025201073 |
| 3      | Abisha Kean Tuana Sirait  | 5025201052 |

---

## Daftar Isi

- [Nomor 1](#nomor-1)
- [Nomor 2](#nomor-2)

---

## Nomor 1

WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet

### Konfigurasi Node

Konfigurasi sesuai modul GNS. Menggunakan IP Prefix 192.191 -> IP Kelompok D13.

### Konfigurasi Ostania

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.191.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.191.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.191.3.1
	netmask 255.255.255.0
```

### Konfigurasi SSS

```
auto eth0
iface eth0 inet static
	address 192.191.1.2
	netmask 255.255.255.0
	gateway 192.191.1.1
```

### Konfigurasi Garden

```
auto eth0
iface eth0 inet static
	address 192.191.1.3
	netmask 255.255.255.0
	gateway 192.191.1.1
```

### Konfigurasi Berlint

```
auto eth0
iface eth0 inet static
	address 192.191.2.2
	netmask 255.255.255.0
	gateway 192.191.2.1
```

### Konfigurasi Eden

```
auto eth0
iface eth0 inet static
	address 192.191.2.3
	netmask 255.255.255.0
	gateway 192.191.2.1
```

### Konfigurasi WISE

```
auto eth0
iface eth0 inet static
	address 192.191.3.2
	netmask 255.255.255.0
	gateway 192.191.3.1
```

### Setting di Web Console

#### Untuk Ostania

```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.191.0.0/16

cat /etc/resolv.conf
```

#### Untuk Node Lain

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Lalu `ping google.com` dan liat hasilnya telah terkoneksi.</br>
<img src="https://user-images.githubusercontent.com/70510279/197745918-5d085bc7-6876-4507-8f69-357c68b1f0db.png" alt="Terkoneksi Internet" width="500"/>

</br>

Hasil Screenshot Node yang telah dibuat:

<img src="https://user-images.githubusercontent.com/70510279/197742613-e9a0134a-eee0-435f-91b0-c5024187ce93.png" alt="Gambar semua node" width="1000"/>

</br>

## Nomor 2

Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise

### Setting domain WISE

#### Install Bind

```
apt-get update

apt-get install bind9 -y
```

#### Setting Domain

```
nano /etc/bind/named.conf.local

zone "wise.d13.com" {
	type master;
	file "/etc/bind/wise/wise.d13.com";
};

mkdir /etc/bind/wise

cp /etc/bind/db.local /etc/bind/wise/wise.d13.com

nano /etc/bind/wise/wise.d13.com

isi ini
wise.d13.com. IN SOA wise.d13.com. root.wise.d13.com. (
    2022100601 ; serial -> 2 ; serial
    604800 ; refresh
    86400 ; retry
    2419200 ; expire
    604800 ; minimum ttl
)
192.191.3.1 ; IP WISE


service bind9 restart
```

### Setting Nameserver pada Client (SSS & Garden)

```
nano /etc/resolv.conf

nameserver 192.191.3.1 ; IP WISE

ping wise.d13.com -c 5
```

## Nomor 3

Setelah itu ia juga ingin membuat subdomain eden.wise.yyy.com dengan alias www.eden.wise.yyy.com yang diatur DNS-nya di WISE dan mengarah ke Eden
