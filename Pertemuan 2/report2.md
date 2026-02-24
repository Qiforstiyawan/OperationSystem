# Laporan pertemuan ke -2 sistem operasi
**Tanggal:** 18 Februari 2026  
**Disusun Oleh:** Rofiq Aristiyawan
**NIM:** 254107020060
**Kelas/No:** TI-1G/27


## 1.1 Deteksi Perangkat Keras di Linux

## 1. Praktikum 2.1 — Identifikasi CPU dan Memori
1. tampilan informasi cpu:
![informasi cpu](image/P2.1-1.png-1)

2. tampilan ringkasan memori:
![informasi memory](image/P2.1-2.png)

3. cek informasi hardware dari DMI/BIOS:
![informasi hardware dari bios](image/P2.1-3.png)

4. Catat: (1) jumlah CPU(s), core/thread, (2) total RAM, (3) total swap. Jelaskan perbedaan RAM vs swap dalam 2–3 kalimat.

jawab : jumlah cpu = 2, core/thread = 2/1, swap = 4GB.
Perbedaan utama antara RAM dan Swap terletak pada fungsi dan kecepatannya:
RAM (Random Access Memory): Merupakan memori utama fisik berkecepatan tinggi yang digunakan untuk menyimpan data aplikasi yang sedang aktif agar dapat diakses instan oleh prosesor.
Swap Memory: Merupakan area di media penyimpanan (HDD atau SSD) yang berfungsi sebagai perluasan atau cadangan ketika kapasitas RAM sudah penuh.

## 2.2 — Identifikasi Perangkat PCI/USB dan Driver
1. Lihat daftar perangkat PCI:
![informasi perangkat PCI](image/P2.2-1.png)

2. Lihat perangkat PCI beserta driver kernel yang digunakan:
![informasi perangkat pci beserta driver](image/P2.2-2.png)

3. Fokus pada NIC (Ethernet) untuk mencari modul driver:
![informasi untuk memastikan ethernet terdeteksi dan memiliki driver yang sesuai](image/P2.2-3.png)

4. Lihat perangkat USB:
![informasi untuk melihat perangkan usb](image/P2.2-4.png)

5. Lihat topologi USB (tree):
![informasi untuk melihat topologi usb tree](image/P2.2-5.png)

6. Temukan 1 perangkat PCI (misal NIC) dan tuliskan: Vendor:Device ID (angka heksadesimal), nama driver/modul kernel, dan deskripsi singkat fungsinya.

jawab : Nama perangkat = Intel Corporation 82540EM Gigabit Ethernet Controller, vendor:device id = 8086:100e, kernel driver = e1000, kernel moduls = e1000.
Deskripsi singkat& fungsi = pengontrol jaringan berkecepatan tinggi yang memungkinkan sistem untuk melakukan pertukaran data secara stabil. Fungsinya agar Ubuntu dapat memperoleh alamat IP melalui protokol DHCP dan memungkinkan akses jarak jauh melalui SSH.

## 2.3 — Identifikasi Storage dan Filesystem
1. Lihat daftar disk/partisi:
![informasi untuk melihat daftar blok perangkat beserta sistem filenya](image/P2.3-1.png)

2. Tampilkan UUID dan tipe filesystem:
![informasi untuk mendapatkan identitas unik dari setiap blok penyimpanan](image/P2.3-2.png)

3. Lihat mount point untuk root filesystem:
![informasi untuk memastikan partisi sistem terpasang dengan benar](image/P2.3-3.png)


## 2.4 — Melihat Modul Aktif dan Informasinya
1. Cek versi kernel:
![informasi versi kernel](image/P2.4-1.png)

2. Tampilkan daftar modul aktif:
![informasi daftar modul aktif](image/P2.4-2.png)

3. Pilih salah satu modul (contoh aman: loop) dan lihat detailnya:
![informasi detail modul dengan modinfo](image/P2.4-3.png)

4. Muat modul (jika belum aktif), lalu verifikasi:
![load modul dan verifikasi](image/P2.4-4.png)

5. (Opsional) lihat pesan kernel terbaru:
![informasi log kernel terbaru](image/P2.4-5.png)


## 2.5 — Konfigurasi Auto-load dan Blacklist:
1. konfigurasi autoload dan blacklist modul
![Melakukan konfigurasi pada modul kernel loop auto load dan blacklist modul.](image/P2.5-1.png)


## 2.6 — Mengenali block vs character  device:
1. Lihat detail salah satu disk (sesuaikan dengan perangkat Anda, misal sda):
![Melihat detail device node disk](image/P2.6-1.png)

2. Lihat detail device terminal:
![Melihat detail device node terminal](image/P2.6-2.png) 

3. Lihat disk dan partisi untuk mengaitkan dengan /dev:
![Mapping disk/partisi](image/P2.6-3.png)

4. Dari output ls -l, jelaskan perbedaan penanda file untuk block device dan
character device. (Hint: karakter pertama pada permission string)

jawab : ls -l /dev/sda	karakter pertama b jenis filenya Block Device (Penyimpanan),
        ls -l /dev/tty	karakter pertama c jenis filenya Character Device (Terminal)


## 2.7 — Melihat Informasi udev:
1. Cek atribut udev untuk disk:
![Melihat atribut udev untuk disk](image/P2.7-1.png)

2. (Opsional) monitor event udev (jalankan, lalu colok/lepas USB pada mesin
fisik):
![Monitor event udev](image/P2.7-2.png)


## 2.8 — Membuat Workspace Praktikum:
1. Buat direktori praktikum dan masuk ke dalamnya:
![Membuat workspace praktikum](image/P2.8-1.png)

2. Buat beberapa file contoh:
![Membuat file contoh](image/P2.8-2.png)

3. Isi file log contoh (simulasi):
![Mengisi file log contoh](image/P2.8-3.png)

4. Baca file dengan less:
![Membaca file dengan less](image/P2.8-4.png)


## 2.9 — Pencarian Pola dengan grep
1. cari barisan error,warn,info dengan grep:
![tampilan dengan grep](image/P2.9-5.png)

2. Gunakan grep untuk menampilkan hanya baris yang mengandung INFO atau
WARN dari data.log. (Hint: gunakan grep -E dengan pola alternatif)

jawab: untuk menggunakan alternatif or kita menggunakan opsi -E dan | berfungsi sebagai atau.  berikut contohnya:
![contoh](image/P2.9-6.png)


## 2.10 — Substitusi dengan sed (Aman di File Latihan)
1. konfigurasi langkah 1-4:
![konfigurasi dan edit](image/P2.10-1.png)

2. Gunakan sed -i dengan hati-hati jika mengedit file sistem. Untuk administrasi
nyata, praktik aman adalah membuat backup: sed -i.bak ’s/old/new/g’
file.conf

jawab: hasil backup
![hasil backup](image/P2.10-2.png)


## 2.11  — Ekstraksi Kolom dengan awk
1. langkah 1-3 hasil:
![hasil](image/P2.11-1.png)


## 2.12 — Melihat Proses dengan ps
1. langkah 1-2 hasil:
![hasil](image/P2.12-1.png)


## 2.13 — Monitoring Real-time dengan top
1. Jalankan top:
![menjalankan top](image/P2.13.png)


## 2.14 — Menghentikan Proses dengan kill
1. langkah 1-5 hasil:
![hasil](image/P2.14.png)


## 2.15 — Cek Disk, Load, dan Service
1. Cek penggunaan disk:
![cek kapasitas disk](image/P2.15-1.png)

2. Cari direktori yang besar (contoh pada /var):
![Cek ukuran direktori](image/P2.15-2.png)

3. Cek load dan uptime:
![cek load average](image/P2.15-3.png)

4. Cek service yang gagal:
![service yang gagal](image/P2.15-4.png)

5. Ambil log error terbaru:
![log error terbaru](image/P2.15-5.png)


## 2.16 — Monitoring Port dan Koneksi (Network Basics)
1. langkah 1-3 hasil:
![hasil](image/P2.16.png)

2. Pilih satu port yang listening dari output ss -tulpn(misal port 22), lalu
tuliskan service/proses yang membukanya. Jelaskan kegunaan port tersebut
secara singkat.

jawab: Port 22 dibuka oleh proses sshd, Port ini digunakan untuk layanan ssh yang memungkinkan akses remote ke server secara aman melalui jaringan.


## 1.9 Latihan
1. 2.A Jalankan lspci -nnk. Pilih 1 perangkat PCI dan tuliskan: nama perangkat, ID vendor:device, dan kernel driver in use.
![hasil](image/L1.png)
jawab: Nama perangkat: Intel 82540EM Gigabit Ethernet Controller, ID vendor: device: 8086:100e, Kernel driver in use: e1000.


2. 2.B Tentukan device root filesystem dengan findmnt /. Lalu cocokkan dengan lsblk -f dan tuliskan tipe filesystem serta UUID-nya.
![hasil](image/L2.png)
jawab: Device = /dev/sda2, FSTYPE = ext4, UUID = 98c08db9-76d1-462a-be73-06afeada6c53.


3. 2.C Buat file server.log berisi minimal 10 baris dengan variasi kata: INFO, WARN, ERROR. Gunakan grep untuk menampilkan hanya baris ERROR.
- ![buat](image/L3.1.png)
- ![hasil](image/L3.2.png)


4. 2.D Gunakan sed untuk mengganti semua kata server menjadi node pada file latihan. Tunjukkan sebelum dan sesudah.
- ![sebelum](image/L4.1.png)
- ![sesudah](image/L4.2.png)


5. 2.E Gunakan df -h lalu awk untuk menampilkan filesystem yang penggunaan disk di atas 70%.
![informasi & filter disk](image/L5.png)


6. 2.F Jalankan sleep 600 &. Temukan PID-nya dengan ps. Hentikan dengan SIGTERM. Jelaskan beda SIGTERM vs SIGKILL.
![hasil](image/L6.png)
perbedaan sigterm dan sigkill = SIGTERM : proses bisa cleanup dulu
                                SIGKILL : langsung paksa mati 


7. 2.G Gunakan systemctl –failed. Jika tidak ada yang gagal, pilih satu service aktif (misal ssh) dan tampilkan status serta 30 baris log terakhirnya.
![hasil](image/L.7.png)