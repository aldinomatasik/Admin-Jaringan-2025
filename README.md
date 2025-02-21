# Laporan Pengerjaan Tugas

## Workshop Administrasi Jaringan

**Nama**    : Aldino Matasik  
**Kelas**    : 2 D4 IT A  
**NRP**      : 3123600030  

**DEPARTEMEN TEKNIK INFORMATIKA DAN KOMPUTER**  
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**  

---

## Tugas Workshop Administrasi Jaringan [REVIEW]
![image](https://github.com/user-attachments/assets/7765cca1-64cc-457b-b2b9-3f27ba73cc26)


### Langkah-langkah Analisis HTTP di Wireshark  
1. Buka File Capture (.cap)
2. Jalankan Wireshark
3. Klik **File → Open**
4. Pilih file `.cap` yang ingin dianalisis
5. Filter HTTP Traffic
6. Ketik `http` di kolom filter (tekan Enter)

---

### Analisis

#### 1. IP Server dan Client  
- **Source IP (Client)** : `145.245.160.237`  
- **Destination IP (Server)** : `65.208.228.223`

#### 2. Versi HTTP  
- Pada paket **HTTP GET /download.html** terlihat :  
  - **HTTP Version:** `HTTP/1.1`

#### 3. Waktu Client Mengirim Request  
- **HTTP GET request** terjadi pada waktu `0.911310` detik.

#### 4. Waktu Server Menerima HTTP Request dari Client  
- Server merespons dengan **ACK pertama** pada waktu: `1.472116` detik.

#### 5. Waktu yang Dibutuhkan untuk Transfer dan Response  
- **Response Time** : `1.472116` detik.
- **Waktu transfer** : `0.560806` detik (`560ms`).

![image](https://github.com/user-attachments/assets/05e2d872-add3-4860-b3b9-be771a3df56d)


## Proses yang Terjadi dalam Gambar

### 1. Process to Process (Lapisan Transport – Transport Layer)  
- Data dikirim dari satu proses di komputer pengirim ke proses di komputer penerima.
- Lapisan transport (misalnya, **TCP/UDP**) bertanggung jawab untuk memastikan komunikasi antar aplikasi berjalan dengan baik.
- **Contoh**: Saat Anda membuka situs web, browser di komputer Anda berkomunikasi dengan server web melalui **HTTP** yang menggunakan protokol transport seperti **TCP**.

### 2. Host to Host (Lapisan Jaringan – Network Layer)  
- Data dikirim dari satu host (komputer) ke host lain melalui jaringan.
- Lapisan Jaringan (**Network Layer**) mengatur pengalamatan dan pengiriman paket data menggunakan protokol seperti **IP (Internet Protocol)**.
- **Contoh**: Paket data dikirim dari alamat IP komputer pengirim ke alamat IP server tujuan melalui internet.

### 3. Node to Node (Lapisan Data Link – Data Link Layer)  
- Data dikirim antara dua perangkat jaringan (**node**) yang terhubung langsung.
- Lapisan data link (misalnya **Ethernet** atau **Wi-Fi**) memastikan bahwa paket dikirim dengan benar antar perangkat jaringan seperti switch atau router.
- **Contoh**: Sebuah paket dikirim dari komputer pengguna ke **router lokal** sebelum melanjutkan ke jaringan yang lebih luas.

---

## Resume

### 1. Establishment (Pembentukan Koneksi – 3-Way Handshake)  
Tahapan:
- **SYN (Synchronize)** → Client mengirim permintaan koneksi.
- **SYN-ACK (Synchronize-Acknowledge)** → Server merespons dengan SYN-ACK untuk menerima koneksi.
- **ACK (Acknowledge)** → Client mengirim **ACK** untuk mengonfirmasi koneksi telah terbentuk.

### 2. Data Transfer (Pengiriman Data)  
Setelah koneksi terbentuk, data dapat dikirim antara **client** dan **server** dengan mekanisme **Sliding Window & Acknowledgment**:
- Data dikirim dalam segmen-segmen **TCP**.
- Setiap segmen memiliki **nomor urut (Sequence Number)** dan **nomor pengakuan (ACK Number)**.
- Jika paket diterima dengan benar, penerima mengirimkan **ACK** sebagai tanda konfirmasi.
- Jika terjadi **packet loss**, pengirim akan mengirim ulang paket yang hilang (**Retransmission**).

### 3. Termination (Pengakhiran Koneksi - 4-Way Handshake)  
Saat komunikasi selesai, koneksi **TCP** harus dihentikan dengan proses **4-way handshake** untuk memastikan semua data telah diterima.

Tahapan:
- **FIN (Finish)** → Host pertama mengirim **FIN** untuk mengakhiri koneksi.
- **ACK (Acknowledge)** → Host kedua mengirim **ACK** sebagai tanda penerimaan permintaan **FIN**.
- **FIN (Finish)** → Host kedua mengirim **FIN** untuk mengakhiri koneksi dari sisi mereka.
- **ACK (Acknowledge)** → Host pertama mengirim **ACK** terakhir untuk mengonfirmasi koneksi telah berakhir.
