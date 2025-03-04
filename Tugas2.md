# Laporan Pengerjaan Tugas - Workshop Administrasi Jaringan

## Informasi Umum
**Nama:** Aldino Matasik  
**Kelas:** 2 D4 IT A  
**NRP:** 3123600030  
**Departemen:** Teknik Informatika dan Komputer, Politeknik Elektronika Negeri Surabaya

## Chapter 4: Proses Control
Proses control terdiri dari address space dan sekumpulan struktur data dalam kernel. Address space adalah sekumpulan halaman memori yang ditandai oleh kernel untuk proses penggunaan. Halaman digunakan untuk menyimpan kode, data, dan tumpukan proses.

### Struktur Data Kernel:
- **Peta ruang alamat proses**
- **Status proses** (berjalan, tidur, dll.)
- **Informasi sumber daya** (CPU, memori, dll.)
- **Informasi file dan port jaringan yang digunakan**
- **Sinyal proses yang diblokir**
- **Pemilik proses (ID pengguna yang memulai proses)**

### Thread
Thread adalah konteks eksekusi dalam sebuah proses. Thread digunakan untuk mencapai paralelisme dalam proses. Contohnya, server web yang membuat thread baru untuk menangani setiap permintaan masuk.

### PID dan PPID
- **PID (Process ID):** Identifikasi unik proses yang dibuat kernel.
- **PPID (Parent Process ID):** ID dari proses induk yang membuatnya.

### UID & EUID
- **UID (User ID):** ID pengguna yang memulai proses.
- **EUID (Effective User ID):** ID pengguna yang menentukan sumber daya yang dapat diakses oleh proses.

### Lifecycle of a Process
- **Fork:** Menyalin proses untuk membuat proses baru.
- **Init/Systemd:** Proses pertama yang berjalan saat sistem booting.

### Signal
Sinyal adalah cara untuk mengirim pemberitahuan ke suatu proses. Contoh:
- **KILL:** Tidak dapat diblokir dan menghentikan proses pada tingkat kernel.
- **INT:** Dikirim oleh driver terminal ketika pengguna mengetikkan `Ctrl+C`.
- **TERM:** Permintaan untuk menghentikan eksekusi sepenuhnya.
- **HUP:** Dikirim ke proses saat terminal pengendali ditutup.
- **QUIT:** Mirip TERM tetapi menghasilkan core dump jika tidak tertangkap.

### Kill: Send Signal
Perintah `kill` digunakan untuk menghentikan sebuah proses dengan mengirimkan sinyal tertentu.
```sh
kill [-signal] pid
```
- `kill -9 pid`: Mematikan proses secara paksa menggunakan sinyal KILL.
- `killall firefox`: Membunuh semua proses dengan nama "firefox".
- `pkill -u username`: Membunuh semua proses yang dimiliki oleh pengguna tertentu.

### PS: Monitoring Processes
Perintah `ps` digunakan untuk memantau proses yang sedang berjalan.
```sh
ps aux
```
- `a`: Menampilkan proses dari semua pengguna.
- `u`: Menampilkan informasi rinci tentang proses.
- `x`: Menampilkan proses yang tidak berhubungan dengan terminal.

### Monitoring Real-Time
- `top`: Menampilkan proses yang berjalan secara real-time.
- `htop`: Versi interaktif dari `top` dengan tampilan yang lebih baik.

## Chapter 5: The Filesystem
Sistem berkas digunakan untuk mengatur sumber daya penyimpanan sistem.

### Komponen Utama Filesystem:
1. **Ruang Nama:** Struktur hierarki untuk menamai file.
2. **API:** Panggilan sistem untuk manipulasi file.
3. **Model Keamanan:** Skema perlindungan file.
4. **Implementasi:** Cara sistem berkas dihubungkan ke perangkat keras.

### Pathnames
Nama path bisa bersifat:
- **Absolut:** `/home/user/file.txt`
- **Relatif:** `./file.txt`

### Mounting & Unmounting
- **mount [device] [mountpoint]**: Melekatkan sistem berkas ke sistem.
- **umount [mountpoint]**: Melepaskan sistem berkas.

### Tipe File
1. **Regular file:** Berisi data teks atau biner.
2. **Direktori:** Menyimpan referensi ke file lain.
3. **Character & Block Device Files:** Berinteraksi dengan perangkat keras.
4. **Local Domain Socket:** Komunikasi antar proses dalam sistem yang sama.
5. **Named Pipes:** Komunikasi antar proses menggunakan file khusus.
6. **Symbolic Links:** Referensi ke file lain.

### Hak Akses File
- `chmod u+rwx,g+rx,o-r file.txt` (Mengatur izin untuk user, group, dan others)
- `chown user:group file.txt` (Mengubah kepemilikan file)
- `umask 022` (Mengatur izin default untuk file baru)

### Access Control Lists (ACL)
ACL memungkinkan pengaturan izin lebih fleksibel dibandingkan model Unix tradisional.
- `getfacl file.txt` (Melihat ACL file)
- `setfacl -m u:user:rw file.txt` (Menetapkan izin tambahan untuk user tertentu)

## Periodic Processes
### Cron: Schedule Processes
Cron digunakan untuk menjalankan perintah pada jadwal yang telah ditentukan.
- `crontab -e`: Mengedit file crontab.
- `crontab -l`: Menampilkan daftar tugas cron.
- `crontab -r`: Menghapus file crontab.

### Systemd Timer
Timer systemd digunakan sebagai alternatif cron dengan lebih banyak fleksibilitas.
- `systemctl list-timers`: Menampilkan daftar timer aktif.

### Penggunaan Tugas Terjadwal
1. **Mengirim Email**: Mengirim laporan harian secara otomatis.
2. **Membersihkan Sistem Berkas**: Menghapus file sementara secara berkala.
3. **Rotasi Log**: Memutar file log berdasarkan ukuran atau tanggal.
4. **Backup & Mirroring**: Mencadangkan data ke sistem lain secara berkala.

## The /proc Filesystem
Direktori `/proc` berisi informasi status sistem dan proses yang berjalan.
- `cat /proc/cpuinfo`: Menampilkan informasi CPU.
- `cat /proc/meminfo`: Menampilkan informasi memori.

## Debugging dengan Strace & Truss
- `strace -p [PID]`: Melihat panggilan sistem dari suatu proses.
- `truss -p [PID]`: Alternatif strace pada FreeBSD.

## Runaway Processes
Jika suatu proses menggunakan 100% CPU tanpa kontrol:
- `kill -9 [PID]`: Mematikan proses secara paksa.
- `top` / `htop`: Memonitor proses runaway secara real-time.

## Kesimpulan
Dokumen ini mencakup dasar-dasar administrasi jaringan, termasuk proses kontrol dan sistem berkas dalam Linux/Unix. Untuk informasi lebih lanjut, silakan merujuk ke dokumentasi resmi atau materi pembelajaran lainnya.