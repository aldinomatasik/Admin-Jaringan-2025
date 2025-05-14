# LAPORAN PENGERJAAN TUGAS  
## Workshop Administrasi Jaringan

**Nama:** Aldino Matasik  
**Kelas:** 2 D4 IT A  
**NRP:** 3123600030  

---

## DEPARTEMEN TEKNIK INFORMATIKA DAN KOMPUTER  
## POLITEKNIK ELEKTRONIKA NEGERI SURABAYA  

---

## Tujuan

Mengkonfigurasi jaringan yang melibatkan dua mesin virtual dengan pengaturan jaringan **internal** dan **akses ke internet melalui bridge adapter**.

- **VM1** berperan sebagai **gateway** dengan IP **192.168.200.1/24**, terhubung ke internet melalui **bridge adapter**.
- **VM2** sebagai **client** dengan IP **192.168.200.22/24**, hanya terhubung ke VM1 melalui jaringan internal.

VM2 tidak memiliki akses langsung ke internet, tetapi dapat terhubung ke internet melalui VM1 yang bertindak sebagai gateway dengan mengaktifkan **IP Forwarding**.

---

## Konfigurasi VM1 (Gateway)

### 1. Instalasi DNS Server (Bind9) dan iptables

```bash
sudo apt update
sudo apt install bind9 bind9utils
sudo apt install iptables
```

---

### 2. Konfigurasi IP Address

Edit file interfaces:

```bash
sudo nano /etc/network/interfaces
```

Isi konfigurasi:

```
auto enp0s3
iface enp0s3 inet dhcp

auto enp0s8
iface enp0s8 inet static
    address 192.168.200.1
    netmask 255.255.255.0
```

- `enp0s3`: Terhubung ke internet via **bridge adapter** (DHCP).
- `enp0s8`: Terhubung ke jaringan internal dengan VM2 (**static IP**).

**Ilustrasi:**
![Konfigurasi IP VM1](https://github.com/user-attachments/assets/3f7fe3ae-b7b7-4592-b9a8-cfc7afb165dc)

---

### 3. Konfigurasi DNS (Bind9)

Pindah ke direktori bind:

```bash
cd /etc/bind
```

#### a. Tambahkan file zone di `named.conf`

```bash
sudo nano /etc/bind/named.conf
```

Tambahkan:

```conf
include "/etc/bind/named.conf.external-zones";
```

![named.conf](https://github.com/user-attachments/assets/59f7a92a-b5c4-4956-92e9-73f4c71e4302)

---

#### b. Konfigurasi `named.conf.options`

```bash
sudo nano /etc/bind/named.conf.options
```

Tambahkan:

```conf
allow-query { any; };
allow-transfer { any; };
recursion yes;
```

![named.conf.options](https://github.com/user-attachments/assets/e22ba80c-a765-4a58-a23f-7b171185b337)

---

#### c. Buat file `named.conf.external-zones`

```bash
sudo nano /etc/bind/named.conf.external-zones
```

![external zones](https://github.com/user-attachments/assets/661779bf-8459-4955-a6bf-83d884e59e63)

---

#### d. Buat Zone File `db.kelompok5`

```bash
sudo nano /etc/bind/kelompok5.com
```

![kelompok5.com zone](https://github.com/user-attachments/assets/5ac96f75-e1dc-475b-93ee-b37a68fd9a0c)

---

#### e. Buat Zone File `db.192`

```bash
sudo nano /etc/bind/1.200.168.192.db
```

![192.db zone](https://github.com/user-attachments/assets/ebabbfd5-4c82-4e72-a7df-af5e27914cc8)

---

### 4. Cek Validasi Konfigurasi DNS

```bash
sudo named-checkzone kelompok5.com /etc/bind/kelompok5.com
sudo named-checkzone 1.200.168.192.in-addr.arpa /etc/bind/1.200.168.192.db
```

![named-checkzone](https://github.com/user-attachments/assets/619a1c8c-f4dc-4b80-b6b2-5c551d710dd4)

Restart bind9:

```bash
sudo systemctl restart bind9
```

---

### 5. Aktifkan IP Forwarding dan NAT

Aktifkan IP Forwarding:

```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

![ip_forward](https://github.com/user-attachments/assets/5f2879d1-b7c5-4ca0-bbd8-c6876541b220)

Tambahkan NAT agar VM2 bisa akses internet via VM1:

```bash
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE
```

![iptables masquerade](https://github.com/user-attachments/assets/0af29f6a-9b88-4ffa-b24d-3e407df0d7fd)

Tambahkan forwarding rule:

```bash
sudo iptables -A FORWARD -i enp0s8 -o enp0s3 -j ACCEPT
sudo iptables -A FORWARD -i enp0s3 -o enp0s8 -j ACCEPT
```

![iptables forward](https://github.com/user-attachments/assets/9bedaf0b-fc03-4cc3-94c7-cabb4633bcbc)

Simpan konfigurasi iptables:

```bash
sudo iptables-save > /etc/iptables/rules.v4
```

![iptables-save](https://github.com/user-attachments/assets/a39b3e7a-636f-4e1e-ae49-81c3b02b20ef)

---

## Konfigurasi VM2 (Client)

### 1. Set IP Statik

Atur IP static yang satu jaringan dengan VM1:

- Address: `192.168.200.22`
- Netmask: `255.255.255.0`
- Gateway: `192.168.200.1`
- DNS: `192.168.200.1`

![VM2 IP](https://github.com/user-attachments/assets/f9d6dc79-7c6b-4292-bb09-3e028ef12950)

---

### 2. Pengujian Koneksi

#### a. Ping antar VM

- **VM2 ke VM1**:
  ```bash
  ping 192.168.200.1
  ```

- **VM1 ke VM2**:
  ```bash
  ping 192.168.200.22
  ```

![ping vm1](https://github.com/user-attachments/assets/4dd1458e-261c-4a97-a9e3-fd9ccc82d7e3)
![ping vm2](https://github.com/user-attachments/assets/739dc2b2-3354-47ef-978f-aaccb8b40ba0)

---

#### b. Cek Koneksi Internet dari VM2

```bash
ping google.com
```

![internet test](https://github.com/user-attachments/assets/5bf6a398-2d6c-4363-ab28-e1f0899bd2cb)

---

#### c. Tes DNS

```bash
dig www.kelompok5.com
dig -x 192.168.200.1
nslookup www.kelompok5.com
```

![dig](https://github.com/user-attachments/assets/b99e308d-3c87-42c1-8fbc-89452b9ea6a7)  
![dig reverse](https://github.com/user-attachments/assets/6dd51114-fe4a-409d-86b8-1f657778174a)  
![nslookup](https://github.com/user-attachments/assets/74c60d94-9f76-45dc-ab84-b6af9498ffe4)

---

## Kesimpulan

Dengan konfigurasi ini, VM1 berhasil difungsikan sebagai **gateway dan DNS server**, serta mampu meneruskan koneksi internet ke VM2 melalui pengaturan NAT dan IP forwarding. VM2 dapat terhubung ke internet meskipun hanya menggunakan jaringan internal, karena semua trafik dialihkan melalui VM1.

---
