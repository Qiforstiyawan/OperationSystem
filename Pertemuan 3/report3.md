# Laporan Pertemuan ke-3 Sistem Operasi

**Tanggal:** 01 maret 2026  
**Disusun Oleh:** Rofiq Aristiyawan
**NIM:** 254107020060  
**Kelas/No:** TI-1G/27  

---

## 1.11 Latihan Praktikum

### Latihan 3.1: Log Management
**Tugas:** Menampilkan 10 file terbesar di `/var/log/`, menyimpan ke `large-logs.txt`, menampilkan di terminal, dan menangani error ke `error.log`.

* **Perintah:**
    ```
    sudo du -ah /var/log/ 2> error.log | sort -rh | head -n 10 | tee large-logs.txt
    ```
* **Hasil Output:**
    ```
165M    /var/log/
160M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8
160M    /var/log/journal
8.1M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/system@00064b4fb141feaa-c313e8b2ed35dc7b.journal~
8.0M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/user-1000.journal
8.0M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/user-1000@00064b7e18051682-9087e75696451600.journal~
8.0M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/user-1000@00064b4fb2a94fb2-92684a764da9b47e.journal~
8.0M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/system.journal
8.0M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/system@00064c20a352dd54-02b32a229bdb76fa.journal~
8.0M    /var/log/journal/5c9f49ed1cdc463e9b30d0b0125e3bb8/system@00064b9def6a38d2-6bacd68dc18ab757.journal~
    ```

---

### Latihan 3.2: Pipeline User Extraction
**Tugas:** Ekstraksi username dari `/etc/passwd`, urutkan secara alfabetis, dan simpan ke `sorted-users.txt`.

* **Perintah:**
    ```
    cut -d: -f1 /etc/passwd | sort | tee sorted-users.txt
    ```
* **Hasil Output:**
    ```
_apt
backup
bin
daemon
dhcpcd
fwupd-refresh
games
irc
landscape
list
lp
mail
man
messagebus
news
nobody
polkitd
pollinate
proxy
qifor
root
sshd
sync
sys
syslog
systemd-network
systemd-resolve
systemd-timesync
tcpdump
tss
usbmux
uucp
uuidd
www-data
    ```

---

### Latihan 3.3: Monitoring Script (1 Menit)
**Tugas:** Membuat script monitoring CPU dan Memory setiap 5 detik dengan output terminal dan file log.

* **Langkah-langkah:**
    1. Membuat file: `nano monitor.sh`
    2. Memberikan izin: `chmod +x monitor.sh`
    3. Eksekusi: `./monitor.sh`

* **Isi Script:**
    ```
    LOG_FILE="monitoring_log.txt"

echo "Memulai monitoring selama 1 menit..."
echo "------------------------------------"

# Loop sebanyak 12 kali
for i in {1..12}
do
    # Mengambil timestamp saat ini
    TIMESTAMP=$(date "+%Y-%m-%d %H:%M:%S")
    
    # Mengambil data CPU dan RAM (menggunakan top dan free)
    # Output dikirim ke terminal DAN di-append (ditambah) ke file menggunakan tee -a
    echo "[$TIMESTAMP] Iterasi ke-$i" | tee -a $LOG_FILE
    
    # Menampilkan penggunaan CPU (baris ke-3 dari top)
    top -bn1 | grep "Cpu(s)" | awk '{print "CPU Usage: " $2 + $4 "%"}' | tee -a $LOG_FILE
    
    # Menampilkan penggunaan Memory dalam MB
    free -m | grep Mem | awk '{print "Mem Usage: " $3 "MB / " $2 "MB"}' | tee -a $LOG_FILE
    
    echo "------------------------------------" | tee -a $LOG_FILE
    
    # Jeda 5 detik sebelum iterasi berikutnya (kecuali iterasi terakhir)
    if [ $i -lt 12 ]; then
        sleep 5
    fi
done

echo "Monitoring selesai! Hasil tersimpan di $LOG_FILE"
    ```

---

* **Hasil:**
```
Memulai monitoring selama 1 menit...
------------------------------------
[2026-02-28 17:13:48] Iterasi ke-1
CPU Usage: 4.1%
Mem Usage: 420MB / 3996MB
------------------------------------
[2026-02-28 17:13:53] Iterasi ke-2
CPU Usage: 2%
Mem Usage: 420MB / 3996MB
------------------------------------
[2026-02-28 17:13:59] Iterasi ke-3
CPU Usage: 2.6%
Mem Usage: 420MB / 3996MB
------------------------------------
[2026-02-28 17:14:05] Iterasi ke-4
CPU Usage: 0%
Mem Usage: 424MB / 3996MB
------------------------------------
[2026-02-28 17:14:11] Iterasi ke-5
CPU Usage: 1.6%
Mem Usage: 424MB / 3996MB
------------------------------------
[2026-02-28 17:14:17] Iterasi ke-6
CPU Usage: 2%
Mem Usage: 424MB / 3996MB
------------------------------------
[2026-02-28 17:14:23] Iterasi ke-7
CPU Usage: 0%
Mem Usage: 424MB / 3996MB
------------------------------------
[2026-02-28 17:14:29] Iterasi ke-8
CPU Usage: 0%
Mem Usage: 425MB / 3996MB
------------------------------------
[2026-02-28 17:14:34] Iterasi ke-9
CPU Usage: 9.5%
Mem Usage: 425MB / 3996MB
------------------------------------
[2026-02-28 17:14:40] Iterasi ke-10
CPU Usage: 0%
Mem Usage: 426MB / 3996MB
------------------------------------
[2026-02-28 17:14:45] Iterasi ke-11
CPU Usage: 0%
Mem Usage: 426MB / 3996MB
------------------------------------
[2026-02-28 17:14:51] Iterasi ke-12
CPU Usage: 0%
Mem Usage: 426MB / 3996MB
------------------------------------
```

---

### Latihan 3.4: File Searching & Redirection
**Tugas:** Mencari semua file `.conf`, membuang error ke `/dev/null`, dan menghitung jumlahnya.

* **Perintah:**
    ```
    sudo find / -name "*.conf" 2> /dev/null | tee config-files.txt | wc -l
    ```
* **Hasil:** 
`401`

---

### Latihan 3.5: Advanced Backup Script
**Tugas:** Backup direktori dengan pemisahan log sukses dan log error disertai timestamp.

* **Langkah-langkah:**
    1. Membuat file: `nano backup_script.sh`
    2. Memberikan izin: `chmod +x backup_script.sh`
    3. Eksekusi: `./backup_script`

* **Isi Script:**
    ```
FOLDER_ASAL="/etc/network" 
NAMA_BACKUP="backup_$(date +%Y%m%d_%H%M%S).tar.gz"
SUCCESS_LOG="backup-success.log"
ERROR_LOG="backup-error.log"

echo "--- Memulai Backup: $(date) ---"

# Proses Tar
# -v: verbose (menampilkan file yang sedang diproses)
# 1> >(...) : Mengambil stdout, tambah timestamp, simpan ke log sukses
# 2> >(...) : Mengambil stderr, tambah timestamp, simpan ke log error
tar -cvzf "$NAMA_BACKUP" "$FOLDER_ASAL" \
    1> >(while read line; do echo "$(date '+%Y-%m-%d %H:%M:%S') - $line"; done | tee -a "$SUCCESS_LOG") \
    2> >(while read line; do echo "$(date '+%Y-%m-%d %H:%M:%S') - ERROR: $line"; done | tee -a "$ERROR_LOG")

echo "--- Backup Selesai: $(date) ---"
    ```

---

* **Perintah:**
    ```
    ./backup_script.sh
    ```
* **Hasil Output:**
    ```
--- Memulai Backup: Sat Feb 28 05:35:05 PM UTC 2026 ---
2026-02-28 17:35:05 - /etc/network/
--- Backup Selesai: Sat Feb 28 05:35:05 PM UTC 2026 ---
2026-02-28 17:35:05 - /etc/network/if-up.d/
Rappizr7@ubuntu-server:~$ 2026-02-28 17:35:05 - /etc/network/if-up.d/ethtool
2026-02-28 17:35:05 - /etc/network/if-pre-up.d/
2026-02-28 17:35:05 - /etc/network/if-pre-up.d/ethtool
2026-02-28 17:35:05 - 2026-02-28 17:35:05 - ERROR: tar: Removing leading `/' from member names
    ```

---

## Kesimpulan

Berdasarkan praktikum yang telah dilaksanakan, dapat disimpulkan beberapa poin utama:
1.  **I/O Redirection:** Penggunaan `2> /dev/null` sangat efektif untuk membersihkan output dari pesan error yang tidak diperlukan,
sementara `tee -a` memastikan data tersimpan secara permanen (append) tanpa menghilangkan tampilan di terminal.
2.  **Otomasi:** Scripting sederhana dapat menggantikan tugas monitoring manual, memberikan efisiensi tinggi bagi administrator sistem dalam memantau resource server secara berkala.
3.  **Manajemen Data:** Teknik pipeline (`|`) memungkinkan pengolahan teks dalam jumlah besar secara cepat dan akurat, yang merupakan keunggulan utama sistem operasi berbasis Unix/Linux.

---