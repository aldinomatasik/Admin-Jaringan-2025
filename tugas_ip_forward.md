**Laporan Pengerjaan Tugas**

Workshop Administrasi Jaringan


Nama : Aldino Matasik

Kelas : 2 D4 IT A

Nrp : 3123600030

**DEPARTEMEN TEKNIK INFORMATIKA DAN KOMPUTER POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**

**Tujuan**

Mengkonfigurasi jaringan yang melibatkan dua mesin virtual dengan pengaturan jaringan internal dan akses ke internet melalui bridge adapter. VM 1 berperan sebagai **gateway**, dengan alamat IP **192.168.200.1/24**, terhubung ke internet menggunakan bridge adapter sehingga dapat mengakses internet. VM 2 adalah **client** dengan alamat IP **192.168.200.22/24**, yang hanya terhubung ke VM 1 melalui jaringan internal. Dalam skenario ini, VM 2 tidak memiliki akses langsung ke internet, tetapi dapat mengaksesnya melalui VM 1 yang bertindak sebagai gateway dengan IP Forwarding.

**Konfigurasi VM1**

Install bind9 untuk DNS server

sudo apt install bind9 bind9utils

Install iptables untuk Routing Tables

sudo apt install iptables

1. Konfigurasi IP address

sudo nano /etc/network/interfaces
![image](https://github.com/user-attachments/assets/3f7fe3ae-b7b7-4592-b9a8-cfc7afb165dc)


* **enp0s3** merupakan network interface primary yang kita hubungkan ke bridge adapter, konfigurasi ini akan kita biarkan default untuk menerima IP address sebagai DHCP client
* **enp0s8** merupakan network interface secondary yang terhubung ke internal network yang digunakan untuk menghubungkan ke VM2, konfigurasi ini kita beri ip statis di 192.168.200.1/24

1. Konfigurasi DNS

Masuk ke direktori /etc/bind**,** pada folder ini kita akan mengkonfigurasi dan membuat beberapa file baru

1. **named.conf**

sudo nano /etc/bind/named.conf

![image](https://github.com/user-attachments/assets/59f7a92a-b5c4-4956-92e9-73f4c71e4302)


tambahkan line include "/etc/bind/named.conf.external-zones" untuk menambahkan zones yang nantinya akan kita buat

1. **named.conf.options**

sudo nano /etc/bind/named.conf.options
![image](https://github.com/user-attachments/assets/e22ba80c-a765-4a58-a23f-7b171185b337)



Tambahkan :

allow-query { any; };

allow-transfer { any; };

recursion yes;

1. named.conf.external-zones

sudo nano named.conf.external-zones
![image](https://github.com/user-attachments/assets/661779bf-8459-4955-a6bf-83d884e59e63)





Disini kita membuat zones untuk DNS kita, terdapat 2 zone dimana satu untuk nameserver yaitu **kelompok9.com** dan satu lagi untuk IP [\*\*1.200.168.192.in-addr.arpa](http://1.200.168.192.in-addr.arpa/).\*\*

1. kelompok5(zone file)

sudo nano kelompok5.com

![image](https://github.com/user-attachments/assets/5ac96f75-e1dc-475b-93ee-b37a68fd9a0c)


1.200.168.192(zone file)

sudo nano 1.200.168.192.db

![image](https://github.com/user-attachments/assets/ebabbfd5-4c82-4e72-a7df-af5e27914cc8)


Setelah itu semua kita konfigurasi , kita cek terlebih dahulu apakah konfigurasi yang sudah kita buat sudah benar atau belum

named-checkzone [nama\_zone] [file\_konfigurasi\_zone]

![image](https://github.com/user-attachments/assets/619a1c8c-f4dc-4b80-b6b2-5c551d710dd4)


Setelah semuanya OK restart

sudo systemctl restart named

1. Set IP Forwading

echo 1 > /proc/sys/net/ipv4/ip\_forward

![image](https://github.com/user-attachments/assets/5f2879d1-b7c5-4ca0-bbd8-c6876541b220)


sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

![image](https://github.com/user-attachments/assets/0af29f6a-9b88-4ffa-b24d-3e407df0d7fd)


Perintah ini berfungsi untuk menambahkan tabel NAT pada **iptables** dan melakukan **MASQUERADE** ke ip yang diteruskan ke interface **enp0s3 yang** terhubung ke internet. Ini akan mentranslasikan ip private ke ip public. Selanjutnya eksekusi perintah berikut

sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT

sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -j ACCEPT

![](data:image/png;base64...)
![image](https://github.com/user-attachments/assets/9bedaf0b-fc03-4cc3-94c7-cabb4633bcbc)


Perintah ini akan menambahkan rules untuk meneruskan ip dari interface **enp0s8** ke **enp0s3** dan sebaliknya agar VM2 bisa terhubung ke internet.

sudo iptables-save

![image](https://github.com/user-attachments/assets/a39b3e7a-636f-4e1e-ae49-81c3b02b20ef)


Konfigurasi VM2

Konfigurasikan IP STatic dari VM2 di halmaan setting address dengan address yang berada pada satu network dengan VM1 pada interface **enp0s8** yaitu **192.168.200.x**

![image](https://github.com/user-attachments/assets/f9d6dc79-7c6b-4292-bb09-3e028ef12950)


Ping VM1 ke VM2 dan sebaliknya.


![image](https://github.com/user-attachments/assets/4dd1458e-261c-4a97-a9e3-fd9ccc82d7e3)



![image](https://github.com/user-attachments/assets/739dc2b2-3354-47ef-978f-aaccb8b40ba0)


Cek internet VM2


![image](https://github.com/user-attachments/assets/5bf6a398-2d6c-4363-ab28-e1f0899bd2cb)


dig www.kelompok5.com


![image](https://github.com/user-attachments/assets/b99e308d-3c87-42c1-8fbc-89452b9ea6a7)


dig -x 192.168.200.1


![image](https://github.com/user-attachments/assets/6dd51114-fe4a-409d-86b8-1f657778174a)


Nslookup ww.kelompok5.com


![image](https://github.com/user-attachments/assets/74c60d94-9f76-45dc-ab84-b6af9498ffe4)
