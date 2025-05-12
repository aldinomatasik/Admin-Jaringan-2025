# 📧 Protokol & Alur Kerja Sistem Email

## 1. Jenis Protokol Email: SMTP, POP3, IMAP, POP3S

### ✉️ SMTP (Simple Mail Transfer Protocol)

- **Tujuan:** Mengirim email dari pengguna ke server, atau antar server.
- **Port Umum:** 25 (biasa), 587 (TLS), 465 (SSL)
- **Arah:** Outgoing (pengiriman)
- **Catatan:** Tidak digunakan untuk membaca email.

### 📥 POP3 (Post Office Protocol v3)

- **Tujuan:** Mengambil email dari server ke perangkat.
- **Port:** 110 (non-enkripsi), 995 (SSL/TLS)
- **Karakteristik:**
  - Email diunduh ke perangkat lalu dihapus dari server.
  - Ideal jika hanya menggunakan satu perangkat.

### 🌐 IMAP (Internet Message Access Protocol)

- **Fungsi:** Akses email langsung dari server secara sinkron.
- **Port:** 143 (non-enkripsi), 993 (TLS/SSL)
- **Ciri-ciri:**
  - Email tetap tersimpan di server.
  - Sinkron di banyak perangkat.

### 🔐 POP3S (POP3 Secure)

- **Fungsi:** Versi aman dari POP3 menggunakan SSL/TLS.
- **Port:** 995
- **Keunggulan:** Seluruh transmisi terenkripsi.

---

## 2. Apa Itu MX Record?

MX Record (Mail Exchange Record) adalah entri DNS yang menunjukkan **server mana** yang bertanggung jawab menerima email untuk sebuah domain.

### 🎯 Fungsi Utama

- Mengarahkan email ke mail server yang tepat.
- Bisa mendukung beberapa server untuk cadangan (failover).
- Mengatur urutan prioritas server penerima.

### ⚠️ Catatan Penting

- MX Record **harus menunjuk ke nama host**, bukan IP langsung.
- Hostname tersebut **wajib memiliki A record/AAAA record**.
- Tanpa MX Record, domain tidak bisa menerima email eksternal.

---

## 3. Ilustrasi Alur Pengiriman Email

![Alur Email](https://github.com/user-attachments/assets/f27c3720-2546-49f0-845d-ba782d937d98)

Diagram menunjukkan komunikasi email dari User A ke User B via internet. Protokol seperti SMTP, POP3, dan IMAP bekerja di balik layar untuk mengirim dan mengambil email.

---

## 🧩 Komponen Penting dalam Sistem Email

### 1. **User Agent (UA)**
- Aplikasi untuk menulis dan membaca email, misalnya Gmail, Thunderbird, Outlook.
- User A mengirim email melalui UA, User B membacanya lewat UA.

### 2. **Spool**
- Buffer atau antrean sementara untuk email yang sedang dikirim.
- Email dikirim ke spool saat tombol “Send” ditekan.

### 3. **Mailboxes**
- Lokasi tempat email disimpan setelah diterima.
- UA mengambil email dari sini.

### 4. **Alias Expander**
- Menangani alamat alias seperti `support@domain.com`.
- Bisa mengarahkan ke banyak alamat berdasarkan data dari Database.

### 5. **Database**
- Berisi data pengguna, alias, dan aturan pengiriman email.
- Dihubungkan ke alias expander.

### 6. **MTA (Mail Transfer Agent)**
- Mesin pengirim antar-server menggunakan protokol SMTP.
- Bertugas menerima email dari spool dan mengirim ke MTA penerima.

### 7. **Internet**
- Jalur antar-server email antar domain berbeda.
- Menggunakan SMTP sebagai protokol transport utama.

---

## 🔄 Urutan Proses Email (User A ➡️ User B)

1. User A menulis email di aplikasi UA.
2. Email dikirim ke spool.
3. Alias expander mengecek apakah alamat penerima adalah alias.
4. Email dikirim ke MTA pengirim.
5. MTA menghubungi MTA penerima lewat internet.
6. MTA penerima menerima email.
7. Alias expander di server penerima mengecek alias tujuan.
8. Email disimpan ke mailbox User B.
9. UA milik User B mengakses dan membaca email dari mailbox.

---

## 📌 Catatan Tambahan

- SMTP bekerja untuk pengiriman antar server.
- POP3/IMAP digunakan oleh client untuk mengambil email dari server.
- Proses sisi kiri diagram = pengiriman, sisi kanan = penerimaan.

