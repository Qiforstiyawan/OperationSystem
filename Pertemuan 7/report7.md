# Laporan Pertemuan ke-7 Sistem Operasi

**Tanggal:** 14 April 2026  
**Disusun Oleh:** Rofiq Aristiyawan 
**NIM:** 254107020060
**Kelas/No:** TI-1G/27  

---

## Praktikum 6.1 
### Lihat shell login dan shell aktif saat ini

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ echo " Shell login : $SHELL " 
 Shell login : /bin/bash 
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ echo " Shell aktif : $0"
 Shell aktif : /usr/bin/bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ bash -- version | head -n 1
bash: version: No such file or directory
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ bash --version | head -n 1
GNU bash, version 5.2.37(1)-release (aarch64-unknown-linux-gnu)
```


### Lihat proses shell yang sedang berjalan

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ echo $$
3213
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ ps -p $$ -o pid,ppid,args=
    PID    PPID 
   3213    3173 /usr/bin/bash
```


### Buat workspace praktikum

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ mkdir -p {bin,backup,logs,sampel,ruang-nama}
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ pwd
/home/qifor/praktikum-os/week07
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ ls
backup  bin  logs  ruang-nama  sampel
```


### Buat beberapa file contoh yang akan dipakai pada praktikum berikutnya

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch sampel/app.conf
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch logs/app-01.log logs/app-02.log logs/app-03.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch sampel/catatan-a.txt sampel/catatan-b.txt
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch sampel/backup-01.tar sampel/backup-02.tar
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch sampel/laporan-harian.log sampel/laporan-mingguan.log sampel/laporan-bulanan.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch "ruang-nama/laporan server april.txt"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ touch "ruang-nama/backup [mingguan] server.conf"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ ls -R
.:
backup  bin  logs  ruang-nama  sampel

./backup:

./bin:

./logs:
app-01.log  app-02.log  app-03.log

./ruang-nama:
'backup [mingguan] server.conf'  'laporan server april.txt'

./sampel:
app.conf  backup-01.tar  backup-02.tar  catatan-a.txt  catatan-b.txt  laporan-bulanan.log  laporan-harian.log  laporan-mingguan.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ 
```


---
## Praktikum 6.2
### Simpan informasi sesi terminal ke file laporan

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ {
  echo "=== RINGKASAN SESI BASH ==="
  date
  echo "User         : $(whoami)"
  echo "Hostname     : $(hostname)"
  echo "Shell login  : $SHELL"
  echo "Shell aktif  : $0"
  echo "PID shell    : $$"
  echo "Direktori    : $(pwd)"
} | tee session-info.txt
=== RINGKASAN SESI BASH ===
Sat Apr 11 11:59:14 WIB 2026
User         : qifor
Hostname     : qifor-QEMU-Virtual-Machine
Shell login  : /bin/bash
Shell aktif  : /usr/bin/bash
PID shell    : 3213
Direktori    : /home/qifor/praktikum-os/week07
```


### Verifikasi isi file laporan

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cat session-info.txt
=== RINGKASAN SESI BASH ===
Sat Apr 11 11:59:14 WIB 2026
User         : qifor
Hostname     : qifor-QEMU-Virtual-Machine
Shell login  : /bin/bash
Shell aktif  : /usr/bin/bash
PID shell    : 3213
Direktori    : /home/qifor/praktikum-os/week07
```

---
## Praktikum 6.3
### Lihat file konfigurasi Bash pada home directory

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ ls -la ~ | grep -E 'bashrc|bash_profile|profile'
-rw-r--r--  1 qifor qifor 3771 Sep  8  2025 .bashrc
-rw-r--r--  1 qifor qifor  807 Sep  8  2025 .profile
```

### Buat backup .bashrc

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cp ~/.bashrc ~/.bashrc.bak-praktikum
```

### Tambahkan blok konfigurasi praktikum

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cat << 'EOF' >> ~/.bashrc
export PRAKTIKUM_BASH_DIR="$HOME/praktikum-os/week07"
export EDITOR=nano
EOF
```

### Terapkan konfigurasi tanpa logout

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ source ~/.bashrc
echo "$PRAKTIKUM_BASH_DIR"
echo "$EDITOR"
/home/qifor/praktikum-os/week07
nano
```


---
## Praktikum 6.4 
### Backup .bash_profile jika sudah ada

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ [ -f ~/.bash_profile ] && cp ~/.bash_profile ~/.bash_profile.bak-praktikum
```


### Tambahkan konfigurasi login shell

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cat << 'EOF' >> ~/.bash_profile
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

echo "Login Bash pada $(date '+%F %T')" >> "$HOME/praktikum-os/week07/login-audit.log"
EOF
```


### Uji dengan membuka login shell baru

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ bash -l
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ tail -n 3 ~/praktikum-os/week07/login-audit.log
Login Bash pada 2026-04-11 12:11:32
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ exit
logout
```


---
## Praktikum 6.5
### Buat variabel lokal

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ KELAS_OS="Sistem Operasi A"
echo "$KELAS_OS"
Sistem Operasi A
```


### Buka subshell dan cek apakah variabel masih ada

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ bash
echo " $KELAS_OS "
exit
```


### Sekarang ubah menjadi environment variable

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ export KELAS_OS="Sistem Operasi A"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ echo "$KELAS_OS"
Sistem Operasi A
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ exit
exit
```


### Lihat isi PATH dan lokasi beberapa perintah

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ echo "$PATH"
which bash
type ls
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin
/usr/bin/bash
ls is aliased to `ls --color=auto'
```


---
## Praktikum 6.6
### Pastikan direktori bin praktikum tersedia

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ mkdir -p ~/praktikum-os/week07/bin
```


### Tambahkan direktori tersebut ke PATH melalui .bashrc

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cat << 'EOF' >> ~/.bashrc
export PATH="$HOME/praktikum-os/week07/bin:$PATH"
EOF

source ~/.bashrc
echo "$PATH"

/home/qifor/praktikum-os/week07/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin
```


### Buat script ringkasan sistem

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cat << 'EOF' > ~/praktikum-os/week07/bin/ringkas-sistem
> echo "Hostname : $(hostname)"
echo "User     : $(whoami)"
echo "Uptime   : $(uptime -p)"
echo "Disk /   :"
df -h /
EOF
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ chmod +x ~/praktikum-os/week07/bin/ringkas-sistem
```


### Jalankan script dari direktori yang berbeda

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07$ cd ~
ringkas-sistem
Hostname : qifor-QEMU-Virtual-Machine
User     : qifor
Uptime   : up 11 hours, 19 minutes
Disk /   :
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda2        24G   13G   11G  55% /
```


---
## Praktikum 6.7
### Tambahkan alias ke .bashrc

```bash
qifor@qifor-QEMU-Virtual-Machine:~$ cat << 'EOF' >> ~/.bashrc
alias ll='ls -lah --color=auto'
alias hist10='history | tail -n 10'
alias cdbashlab='cd $HOME/praktikum-os/week07'
EOF
source ~/.bashrc
```


### Uji alias

```bash
qifor@qifor-QEMU-Virtual-Machine:~$ ll
total 136K
-rw-r--r--  1 root   root      7 Feb 24 11:03 -
drwxr-x--- 21 qifor qifor 4.0K Apr 11 12:10 .
drwxr-xr-x  3 root   root   4.0K Feb 11 21:51 ..
-rw-------  1 qifor qifor 2.4K Apr 11 12:17 .bash_history
-rw-r--r--  1 qifor qifor  220 Sep  8  2025 .bash_logout
-rw-rw-r--  1 qifor qifor  133 Apr 11 12:10 .bash_profile
-rw-r--r--  1 qifor qifor 4.0K Apr 11 12:26 .bashrc
-rw-r--r--  1 qifor qifor 3.7K Apr 11 12:03 .bashrc.bak-praktikum
drwx------ 12 qifor qifor 4.0K Feb 22 20:13 .cache
drwx------ 18 qifor qifor 4.0K Mar  3 18:54 .config
drwx------  2 qifor qifor 4.0K Apr 11 11:45 .gnupg
drwx------  4 qifor qifor 4.0K Feb 11 21:51 .local
-rw-r--r--  1 qifor qifor  807 Sep  8  2025 .profile
drwx------  2 qifor qifor 4.0K Feb 11 21:51 .ssh
drwxrwxr-x  4 qifor qifor 4.0K Mar 11 10:28 A
drwxrwxr-x  3 qifor qifor 4.0K Mar 11 10:28 B
drwxrwxr-x  2 qifor qifor 4.0K Mar 11 10:28 C
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Desktop
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Documents
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Downloads
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Music
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Pictures
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Public
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Templates
drwxr-xr-x  2 qifor qifor 4.0K Feb 11 21:51 Videos
-rw-r--r--  1 root   root      7 Feb 24 11:03 conf
-rw-r--r--  1 root   root      7 Feb 24 11:03 d
-rw-r--r--  1 root   root      7 Feb 24 11:03 etc
-rw-r--r--  1 root   root      7 Feb 24 11:03 load
-rw-r--r--  1 root   root      7 Feb 24 11:03 loop
-rw-r--r--  1 root   root      7 Feb 24 11:03 modules
drwxrwxr-x  4 qifor qifor 4.0K Feb 24 08:23 neofetch
drwxrwxr-x  7 qifor qifor 4.0K Apr 11 11:48 praktikum-os
drwx------  4 qifor qifor 4.0K Feb 11 21:51 snap
qifor@qifor-QEMU-Virtual-Machine:~$ hist10

alias hist10='history | tail -n 10'

alias cdbashlab='cd $HOME/praktikum-os/week07'

EOF

  136  source ~/.bashrc
  137  ll
  138  hist10
```


---
## Praktikum 6.8
### Siapkan file konfigurasi contoh
```bash
qifor@qifor-QEMU-Virtual-Machine:~$ echo "PORT=8080" > ~/praktikum-os/week07/sample-app.conf
qifor@qifor-QEMU-Virtual-Machine:~$ cat ~/praktikum-os/week07/sample-app.conf
PORT=8080
```

### Tambahkan fungsi ke .bashrc
```bash
qifor@qifor-QEMU-Virtual-Machine:~$ cat << 'EOF' >> ~/.bashrc
backup_conf() {
    if [ $# -ne 1 ]; then
        echo "Usage: backup_conf <file>"
        return 1
    fi
    local src="$1"
    local dst="$HOME/praktikum-os/week07/backup"
    if [ ! -f "$src" ]; then
        echo "File tidak ditemukan: $src"
        return 2
    fi
    mkdir -p "$dst"
    cp -- "$src" "$dst/$(basename "$src").$(date +%F-%H%M%S).bak"
    echo "Backup selesai di: $dst"
}
EOF
source ~/.bashrc
```

### Uji fungsi
```bash
qifor@qifor-QEMU-Virtual-Machine:~$ backup_conf ~/praktikum-os/week07/sample-app.conf
Backup selesai di: /home/qifor/praktikum-os/week07/backup
qifor@qifor-QEMU-Virtual-Machine:~$ ls -lah ~/praktikum-os/week07/backup
total 12K
drwxrwxr-x 2 qifor qifor 4.0K Apr 11 12:32 .
drwxrwxr-x 7 qifor qifor 4.0K Apr 11 12:28 ..
-rw-rw-r-- 1 qifor qifor   10 Apr 11 12:32 sample-app.conf.2026-04-11-123234.bak
qifor@qifor-QEMU-Virtual-Machine:~$ type backup_conf
backup_conf is a function
backup_conf () 
{ 
    if [ $# -ne 1 ]; then
        echo "Usage: backup_conf <file>";
        return 1;
    fi;
    local src="$1";
    local dst="$HOME/praktikum-os/week07/backup";
    if [ ! -f "$src" ]; then
        echo "File tidak ditemukan: $src";
        return 2;
    fi;
    mkdir -p "$dst";
    cp -- "$src" "$dst/$(basename "$src").$(date +%F-%H%M%S).bak";
    echo "Backup selesai di: $dst"
}
```

---
## Praktikum 6.9
### Pastikan file contoh tersedia
```bash
qifor@qifor-QEMU-Virtual-Machine:~$ cd ~/praktikum-os/week07/sampel
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ touch laporan-harian.log laporan-mingguan.log laporan-bulanan.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ls
app.conf  backup-01.tar  backup-02.tar  catatan-a.txt  catatan-b.txt  laporan-bulanan.log  laporan-harian.log  laporan-mingguan.log
```

### Pastikan file contoh tersedia
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ cat laporan-
laporan-bulanan.log   laporan-harian.log    laporan-mingguan.log  
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ cat laporan-harian.log 
```

### Jalankan beberapa perintah sederhana
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ pwd
/home/qifor/praktikum-os/week07/sampel
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ls -lah
total 8.0K
drwxrwxr-x 2 qifor qifor 4.0K Apr 11 11:54 .
drwxrwxr-x 7 qifor qifor 4.0K Apr 11 12:28 ..
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:53 app.conf
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:53 backup-01.tar
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:53 backup-02.tar
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:53 catatan-a.txt
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:53 catatan-b.txt
-rw-rw-r-- 1 qifor qifor    0 Apr 11 12:35 laporan-bulanan.log
-rw-rw-r-- 1 qifor qifor    0 Apr 11 12:35 laporan-harian.log
-rw-rw-r-- 1 qifor qifor    0 Apr 11 12:35 laporan-mingguan.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ date
Sat Apr 11 12:38:56 WIB 2026
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ whoami
qifor
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ history | tail -n 10
  147  clear
  148  cd ~/praktikum-os/week07/sampel
  149  touch laporan-harian.log laporan-mingguan.log laporan-bulanan.log
  150  ls
  151  cat lap
  152  pwd
  153  ls -lah
  154  date
  155  whoami
  156  history | tail -n 10
```

---
## Praktikum 6.10
### Jalankan beberapa perintah diagnostik
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           677M  2.2M  675M   1% /run
/dev/vda2        24G   13G   11G  55% /
tmpfs           1.7G     0  1.7G   0% /dev/shm
efivarfs        256K   17K  240K   7% /sys/firmware/efi/efivars
tmpfs           5.0M  8.0K  5.0M   1% /run/lock
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/vda1       1.1G  6.6M  1.1G   1% /boot/efi
tmpfs           1.7G  8.0K  1.7G   1% /tmp
tmpfs           1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyAMA0.service
tmpfs           339M   92K  339M   1% /run/user/1000
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ free -h
               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       1.4Gi        73Mi        95Mi       2.1Gi       1.9Gi
Swap:          3.8Gi       128Ki       3.8Gi
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ uptime
 12:40:37 up 11:37,  1 user,  load average: 0.08, 0.11, 0.04
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ps aux | head
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.4  26264 15728 ?        Ss   01:03   0:05 /usr/lib/systemd/systemd --switched-root --system --deserialize=48 splash
root           2  0.0  0.0      0     0 ?        S    01:03   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        S    01:03   0:00 [pool_workqueue_release]
root           4  0.0  0.0      0     0 ?        I<   01:03   0:00 [kworker/R-rcu_gp]
root           5  0.0  0.0      0     0 ?        I<   01:03   0:00 [kworker/R-sync_wq]
root           6  0.0  0.0      0     0 ?        I<   01:03   0:00 [kworker/R-kvfree_rcu_reclaim]
root           7  0.0  0.0      0     0 ?        I<   01:03   0:00 [kworker/R-slub_flushwq]
root           8  0.0  0.0      0     0 ?        I<   01:03   0:00 [kworker/R-netns]
root          11  0.0  0.0      0     0 ?        I<   01:03   0:00 [kworker/0:0H-events_highpri]
```

### Cari ulang perintah diagnostik dari history
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ history | grep -E 'df -h|free -h|uptime|ps aux'
   27  df -h
   28  free -h
echo "Uptime   : $(uptime -p)"
df -h /
  158  df -h
  159  free -h
  160  uptime
  161  ps aux | head
  162  history | grep -E 'df -h|free -h|uptime|ps aux'
```

### Jalankan ulang salah satu perintah berdasarkan nomor history
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ !158
df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           677M  2.2M  675M   1% /run
/dev/vda2        24G   13G   11G  55% /
tmpfs           1.7G     0  1.7G   0% /dev/shm
efivarfs        256K   17K  240K   7% /sys/firmware/efi/efivars
tmpfs           5.0M  8.0K  5.0M   1% /run/lock
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
/dev/vda1       1.1G  6.6M  1.1G   1% /boot/efi
tmpfs           1.7G  8.0K  1.7G   1% /tmp
tmpfs           1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyAMA0.service
tmpfs           339M   92K  339M   1% /run/user/1000
```

### Simpan potongan history ke file dokumentasi
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ history | tail -n 20 > ~/praktikum-os/week07/diag-history.txt
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ cat ~/praktikum-os/week07/diag-history.txt
  145  ls -lah ~/praktikum-os/week07/backup
  146  type backup_conf
  147  clear
  148  cd ~/praktikum-os/week07/sampel
  149  touch laporan-harian.log laporan-mingguan.log laporan-bulanan.log
  150  ls
  151  cat lap
  152  pwd
  153  ls -lah
  154  date
  155  whoami
  156  history | tail -n 10
  157  clear
  158  df -h
  159  free -h
  160  uptime
  161  ps aux | head
  162  history | grep -E 'df -h|free -h|uptime|ps aux'
  163  df -h
  164  history | tail -n 20 > ~/praktikum-os/week07/diag-history.txt
```

---
## Praktikum 6.11
### Masuk ke direktori sampel
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ls
app.conf  backup-01.tar  backup-02.tar  catatan-a.txt  catatan-b.txt  laporan-bulanan.log  laporan-harian.log  laporan-mingguan.log
```

### Coba beberapa pola wildcard
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ls *.log
laporan-bulanan.log  laporan-harian.log  laporan-mingguan.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ls catatan-?.txt
catatan-a.txt  catatan-b.txt
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ ls backup-0[12].tar
backup-01.tar  backup-02.tar
```

### Coba beberapa ekspansi lain
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ echo log-{pagi,siang,malam}.txt
log-pagi.txt log-siang.txt log-malam.txt
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ echo ~
/home/qifor
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ echo ~/praktikum-os/week07
/home/qifor/praktikum-os/week07
```

---
## Praktikum 6.12
### Siapkan file log tambahan
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/sampel$ cd ~/praktikum-os/week07/logs
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ touch access-01.log access-02.log access-03.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ ls
access-01.log  access-02.log  access-03.log  app-01.log  app-02.log  app-03.log
```

### Preview file yang akan diproses
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ echo *.log
access-01.log access-02.log access-03.log app-01.log app-02.log app-03.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ echo access-0?.log
access-01.log access-02.log access-03.log
```

### Pindahkan semua file log ke folder arsip
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ mkdir -p arsip-log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ mv *.log arsip-log/
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ ls arsip-log
access-01.log  access-02.log  access-03.log  app-01.log  app-02.log  app-03.log
```

### Kompres folder arsip
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ tar -czf "arsip-log-$(date +%F).tar.gz" arsip-log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ ls -lah
total 16K
drwxrwxr-x 3 qifor qifor 4.0K Apr 13 13:16 .
drwxrwxr-x 7 qifor qifor 4.0K Apr 11 12:43 ..
drwxrwxr-x 2 qifor qifor 4.0K Apr 13 13:15 arsip-log
-rw-rw-r-- 1 qifor qifor  223 Apr 13 13:16 arsip-log-2026-04-13.tar.gz
```

---
## Praktikum 6.13
### Uji single quote dan double quote
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ echo '$USER bekerja di $HOME'
$USER bekerja di $HOME
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ echo "$USER bekerja di $HOME"
qifor bekerja di /home/qifor
```

### Uji escape karakter spasi
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ cd ~/praktikum-os/week07/ruang-nama
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ ls "laporan server april.txt"
'laporan server april.txt'
```

### Uji akses file yang sama dengan double quote
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/logs$ cd ~/praktikum-os/week07/ruang-nama
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ ls "laporan server april.txt"
'laporan server april.txt'
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ cat "laporan server april.txt"
```

---
## Praktikum 6.14
### Pastikan file target tersedia
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ cd ~/praktikum-os/week07/ruang-nama
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ ls -lah
total 8.0K
drwxrwxr-x 2 qifor qifor 4.0K Apr 11 11:54  .
drwxrwxr-x 7 qifor qifor 4.0K Apr 11 12:43  ..
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:54 'backup [mingguan] server.conf'
-rw-rw-r-- 1 qifor qifor    0 Apr 11 11:54 'laporan server april.txt'
```

### Salin file dengan nama kompleks ke folder backup
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ cp -- "backup [mingguan] server.conf" "$HOME/praktikum-os/week07/backup/backup-mingguan-server.conf"
```

### Gunakan variabel untuk memproses path dengan aman:
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ cp -- "backup [mingguan] server.conf" "$HOME/praktikum-os/week07/backup/backup-mingguan-server.conf"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ file_asli="$HOME/praktikum-os/week07/ruang-nama/backup [mingguan] server.conf"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ file_salinan="$HOME/praktikum-os/week07/backup/backup-mingguan-server-v2.conf"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ cp -- "$file_asli" "$file_salinan"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ ls -lah "$HOME/praktikum-os/week07/backup"
total 12K
drwxrwxr-x 2 qifor qifor 4.0K Apr 13 13:24 .
drwxrwxr-x 7 qifor qifor 4.0K Apr 11 12:43 ..
-rw-rw-r-- 1 qifor qifor    0 Apr 13 13:24 backup-mingguan-server-v2.conf
-rw-rw-r-- 1 qifor qifor    0 Apr 13 13:23 backup-mingguan-server.conf
-rw-rw-r-- 1 qifor qifor   10 Apr 11 12:32 sample-app.conf.2026-04-11-123234.bak
```

### Tampilkan daftar file hasil backup:
```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/ruang-nama$ for file in "$HOME/praktikum-os/week07/backup"/*; do
    printf 'Hasil backup: %s\n' "$file"
done

Hasil backup: /home/qifor/praktikum-os/week07/backup/backup-mingguan-server.conf
Hasil backup: /home/qifor/praktikum-os/week07/backup/backup-mingguan-server-v2.conf
Hasil backup: /home/qifor/praktikum-os/week07/backup/sample-app.conf.2026-04-11-123234.bak
```
---
## Tugas Praktikum 1

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ mkdir -p ~/bin
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ export PATH="$HOME/bin:$PATH"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ alias ll='ls -lah'
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ alias cek-disk='df -h | grep "^/dev/"'
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ quick-zip() {
    tar -czf "$1.tar.gz" "$1" && echo "File $1 berhasil di-zip."
}
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ source ~/.bashrc
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ cat << 'EOF' > ~/bin/spek
> echo "--- RINGKASAN SISTEM ---"
echo "User    : $(whoami)"
echo "Uptime  : $(uptime -p)"
echo "Memory  : $(free -h | grep Mem | awk '{print $3 "/" $2}')"
EOF
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ chmod +x ~/bin/spek
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum1$ cd /tmp
qifor@qifor-QEMU-Virtual-Machine:/tmp$ echo "--- LAPORAN TOOLKIT BASH ---" > ~/toolkit-bash-report.txt
echo -e "\n1. Output PATH:" >> ~/toolkit-bash-report.txt
echo $PATH >> ~/toolkit-bash-report.txt
qifor@qifor-QEMU-Virtual-Machine:/tmp$ echo -e "\n2. Verifikasi Alias (ll):" >> ~/toolkit-bash-report.txt
type ll >> ~/toolkit-bash-report.txt
qifor@qifor-QEMU-Virtual-Machine:/tmp$ echo -e "\n3. Verifikasi Fungsi (quick-zip):" >> ~/toolkit-bash-report.txt
type quick-zip >> ~/toolkit-bash-report.txt
qifor@qifor-QEMU-Virtual-Machine:/tmp$ echo -e "\n4. Verifikasi Script (spek):" >> ~/toolkit-bash-report.txt
type spek >> ~/toolkit-bash-report.txt
qifor@qifor-QEMU-Virtual-Machine:/tmp$ echo -e "\n5. Eksekusi Script dari direktori $(pwd):" >> ~/toolkit-bash-report.txt
spek >> ~/toolkit-bash-report.txt
qifor@qifor-QEMU-Virtual-Machine:/tmp$ cat ~/toolkit-bash-report.txt
--- LAPORAN TOOLKIT BASH ---

1. Output PATH:
/home/qifor/praktikum-os/week07/bin:/home/qifor/bin:/home/qifor/praktikum-os/week07/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin

2. Verifikasi Alias (ll):
ll is aliased to `ls -lah --color=auto'

3. Verifikasi Fungsi (quick-zip):
quick-zip is a function
quick-zip () 
{ 
    tar -czf "$1.tar.gz" "$1" && echo "File $1 berhasil di-zip."
}

4. Verifikasi Script (spek):
spek is /home/qifor/bin/spek

5. Eksekusi Script dari direktori /tmp:
--- RINGKASAN SISTEM ---
User    : qifor
Uptime  : up 55 minutes
Memory  : 1.3Gi/3.3Gi
```



---
## Tugas Praktikum 2


```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ LAPORAN="audit-konfigurasi-$(date +%F).txt"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ ERROR_LOG="audit-error.log"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ find /etc -name "*.conf" > "$LAPORAN" 2> "$ERROR_LOG"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ JUMLAH=$(wc -l < "$LAPORAN")
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ echo -e "\n--- RINGKASAN AUDIT ---" | tee -a "$LAPORAN"

--- RINGKASAN AUDIT ---
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ echo "Total file ditemukan: $JUMLAH" | tee -a "$LAPORAN"
Total file ditemukan: 316
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ echo "Error dicatat di    : $ERROR_LOG" | tee -a "$LAPORAN"
Error dicatat di    : audit-error.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ cat <<EOF >> "$LAPORAN"
> ANALISIS SINGKAT:
Pemisahan stdout (>) dan stderr (2>) penting agar daftar konfigurasi tidak kotor oleh pesan error. 
Hal ini memudahkan penghitungan file dan memastikan log error tersimpan khusus untuk pengecekan hak akses.
EOF
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum2$ cat "$LAPORAN"
/etc/PackageKit/PackageKit.conf
/etc/PackageKit/Vendor.conf
/etc/udisks2/udisks2.conf
/etc/libao.conf
/etc/cups/snmp.conf
/etc/cups/cupsd.conf
/etc/cups/cups-files.conf
/etc/cups/subscriptions.conf
/etc/cups/cups-browsed.conf
/etc/gtk-3.0/im-multipress.conf
/etc/gtk-2.0/im-multipress.conf
/etc/ubuntu-advantage/uaclient.conf
/etc/usb_modeswitch.conf
/etc/environment.d/90qt-a11y.conf
/etc/environment.d/90atk-adaptor.conf
/etc/ssh/ssh_config.d/20-systemd-ssh-proxy.conf
/etc/xattr.conf
/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
/etc/NetworkManager/NetworkManager.conf
/etc/dbus-1/system.d/com.ubuntu.WhoopsiePreferences.conf
/etc/dbus-1/system.d/com.redhat.NewPrinterNotification.conf
/etc/dbus-1/system.d/com.hp.hplip.conf
/etc/dbus-1/system.d/org.opensuse.CupsPkHelper.Mechanism.conf
/etc/dbus-1/system.d/com.ubuntu.LanguageSelector.conf
/etc/dbus-1/system.d/com.redhat.PrinterDriversInstaller.conf
/etc/dbus-1/system.d/org.debian.apt.conf
/etc/dbus-1/system.d/com.ubuntu.SoftwareProperties.conf
/etc/ca-certificates.conf
/etc/mke2fs.conf
/etc/UPower/UPower.conf
/etc/chrony/conf.d/ubuntu-nts.conf
/etc/chrony/chrony.conf
/etc/cracklib/cracklib.conf
/etc/updatedb.conf
/etc/libaudit.conf
/etc/debconf.conf
/etc/nftables.conf
/etc/vconsole.conf
/etc/rsyslog.d/50-default.conf
/etc/rsyslog.d/21-cloudinit.conf
/etc/rsyslog.d/20-ufw.conf
/etc/tmpfiles.d/screen-cleanup.conf
/etc/dhcpcd.conf
/etc/init/whoopsie.conf
/etc/sudo_logsrvd.conf
/etc/avahi/avahi-daemon.conf
/etc/ufw/ufw.conf
/etc/ufw/sysctl.conf
/etc/resolv.conf
/etc/ipp-usb/ipp-usb.conf
/etc/pnm2ppa.conf
/etc/adduser.conf
/etc/rygel.conf
/etc/snmp/snmp.conf
/etc/selinux/semanage.conf
/etc/sensors3.conf
/etc/ldap/ldap.conf
/etc/udev/udev.conf
/etc/udev/iocost.conf
/etc/speech-dispatcher/modules/espeak-ng.conf
/etc/speech-dispatcher/modules/cicero.conf
/etc/speech-dispatcher/modules/llia_phon-generic.conf
/etc/speech-dispatcher/modules/dtk-generic.conf
/etc/speech-dispatcher/modules/espeak.conf
/etc/speech-dispatcher/modules/festival.conf
/etc/speech-dispatcher/modules/flite.conf
/etc/speech-dispatcher/modules/espeak-ng-mbrola-generic.conf
/etc/speech-dispatcher/modules/mary-generic.conf
/etc/speech-dispatcher/modules/openjtalk.conf
/etc/speech-dispatcher/modules/espeak-ng-mbrola.conf
/etc/speech-dispatcher/modules/epos-generic.conf
/etc/speech-dispatcher/modules/mimic3-generic.conf
/etc/speech-dispatcher/modules/espeak-mbrola-generic.conf
/etc/speech-dispatcher/modules/swift-generic.conf
/etc/speech-dispatcher/speechd.conf
/etc/speech-dispatcher/clients/emacs.conf
/etc/ld.so.conf
/etc/plymouth/plymouthd.conf
/etc/initramfs-tools/initramfs.conf
/etc/ghostscript/fontmap.d/10fonts-urw-base35.conf
/etc/ghostscript/cidfmap.d/90gs-cjk-resource-japan2.conf
/etc/ghostscript/cidfmap.d/90gs-cjk-resource-korea1.conf
/etc/ghostscript/cidfmap.d/90gs-cjk-resource-japan1.conf
/etc/ghostscript/cidfmap.d/90gs-cjk-resource-cns1.conf
/etc/ghostscript/cidfmap.d/90gs-cjk-resource-gb1.conf
/etc/ld.so.conf.d/libc.conf
/etc/ld.so.conf.d/aarch64-linux-gnu.conf
/etc/xdg/user-dirs.conf
/etc/host.conf
/etc/locale.conf
/etc/pulse/client.conf
/etc/rsyslog.conf
/etc/apt/apt.conf.d/20apt-esm-hook.conf
/etc/apt/apt.conf.d/20snapd.conf
/etc/logrotate.conf
/etc/modprobe.d/blacklist-oss.conf
/etc/modprobe.d/blacklist-framebuffer.conf
/etc/modprobe.d/blacklist.conf
/etc/modprobe.d/blacklist-modem.conf
/etc/modprobe.d/iwlwifi.conf
/etc/modprobe.d/alsa-base.conf
/etc/modprobe.d/blacklist-firewire.conf
/etc/modprobe.d/blacklist-ath_pci.conf
/etc/modprobe.d/blacklist-rare-network.conf
/etc/fprintd.conf
/etc/bluetooth/input.conf
/etc/bluetooth/network.conf
/etc/bluetooth/main.conf
/etc/systemd/logind.conf
/etc/systemd/system.conf
/etc/systemd/resolved.conf
/etc/systemd/journald.conf
/etc/systemd/oomd.conf
/etc/systemd/sleep.conf
/etc/systemd/networkd.conf
/etc/systemd/user.conf
/etc/systemd/pstore.conf
/etc/deluser.conf
/etc/ucf.conf
/etc/e2scrub.conf
/etc/apparmor/parser.conf
/etc/gdm3/custom.conf
/etc/kdump/sysctl.conf
/etc/apport/crashdb.conf
/etc/modules-load.d/modules.conf
/etc/pam.conf
/etc/fonts/fonts.conf
/etc/fonts/conf.avail/20-unhint-small-dejavu-lgc-serif.conf
/etc/fonts/conf.avail/30-droid-noto.conf
/etc/fonts/conf.avail/20-unhint-small-dejavu-sans.conf
/etc/fonts/conf.avail/58-dejavu-lgc-sans-mono.conf
/etc/fonts/conf.avail/20-unhint-small-dejavu-lgc-sans-mono.conf
/etc/fonts/conf.avail/20-unhint-small-dejavu-sans-mono.conf
/etc/fonts/conf.avail/20-unhint-small-dejavu-serif.conf
/etc/fonts/conf.avail/65-droid-sans-fallback.conf
/etc/fonts/conf.avail/57-dejavu-serif.conf
/etc/fonts/conf.avail/58-dejavu-lgc-serif.conf
/etc/fonts/conf.avail/57-dejavu-sans-mono.conf
/etc/fonts/conf.avail/57-dejavu-sans.conf
/etc/fonts/conf.avail/20-unhint-small-dejavu-lgc-sans.conf
/etc/fonts/conf.avail/58-dejavu-lgc-sans.conf
/etc/fonts/conf.avail/30-droid-noto-mono.conf
/etc/fonts/conf.d/10-scale-bitmap-fonts.conf
/etc/fonts/conf.d/61-urw-standard-symbols-ps.conf
/etc/fonts/conf.d/69-language-selector-zh-tw.conf
/etc/fonts/conf.d/20-unhint-small-dejavu-lgc-serif.conf
/etc/fonts/conf.d/49-sansserif.conf
/etc/fonts/conf.d/60-generic.conf
/etc/fonts/conf.d/20-unhint-small-dejavu-sans.conf
/etc/fonts/conf.d/58-dejavu-lgc-sans-mono.conf
/etc/fonts/conf.d/61-urw-nimbus-roman.conf
/etc/fonts/conf.d/10-hinting-slight.conf
/etc/fonts/conf.d/20-unhint-small-dejavu-lgc-sans-mono.conf
/etc/fonts/conf.d/61-urw-gothic.conf
/etc/fonts/conf.d/48-spacing.conf
/etc/fonts/conf.d/61-urw-fallback-backwards.conf
/etc/fonts/conf.d/40-nonlatin.conf
/etc/fonts/conf.d/10-sub-pixel-rgb.conf
/etc/fonts/conf.d/61-urw-d050000l.conf
/etc/fonts/conf.d/69-language-selector-zh-cn.conf
/etc/fonts/conf.d/20-unhint-small-dejavu-sans-mono.conf
/etc/fonts/conf.d/60-latin.conf
/etc/fonts/conf.d/69-unifont.conf
/etc/fonts/conf.d/45-generic.conf
/etc/fonts/conf.d/65-fonts-persian.conf
/etc/fonts/conf.d/90-synthetic.conf
/etc/fonts/conf.d/64-language-selector-cjk-prefer.conf
/etc/fonts/conf.d/61-urw-p052.conf
/etc/fonts/conf.d/99-language-selector-zh.conf
/etc/fonts/conf.d/61-urw-bookman.conf
/etc/fonts/conf.d/80-delicious.conf
/etc/fonts/conf.d/70-no-bitmaps-except-emoji.conf
/etc/fonts/conf.d/61-urw-nimbus-sans.conf
/etc/fonts/conf.d/70-fonts-noto-cjk.conf
/etc/fonts/conf.d/51-local.conf
/etc/fonts/conf.d/20-unhint-small-dejavu-serif.conf
/etc/fonts/conf.d/45-latin.conf
/etc/fonts/conf.d/65-droid-sans-fallback.conf
/etc/fonts/conf.d/61-urw-nimbus-mono-ps.conf
/etc/fonts/conf.d/57-dejavu-serif.conf
/etc/fonts/conf.d/58-dejavu-lgc-serif.conf
/etc/fonts/conf.d/71-ubuntulegacy.conf
/etc/fonts/conf.d/65-nonlatin.conf
/etc/fonts/conf.d/69-language-selector-zh-sg.conf
/etc/fonts/conf.d/30-metric-aliases.conf
/etc/fonts/conf.d/57-dejavu-sans-mono.conf
/etc/fonts/conf.d/69-language-selector-zh-mo.conf
/etc/fonts/conf.d/30-cjk-aliases.conf
/etc/fonts/conf.d/10-yes-antialias.conf
/etc/fonts/conf.d/57-dejavu-sans.conf
/etc/fonts/conf.d/20-unhint-small-dejavu-lgc-sans.conf
/etc/fonts/conf.d/20-unhint-small-vera.conf
/etc/fonts/conf.d/11-lcdfilter-default.conf
/etc/fonts/conf.d/69-language-selector-zh-hk.conf
/etc/fonts/conf.d/61-urw-z003.conf
/etc/fonts/conf.d/61-urw-fallback-generics.conf
/etc/fonts/conf.d/69-language-selector-ja.conf
/etc/fonts/conf.d/56-language-selector-prefer.conf
/etc/fonts/conf.d/58-dejavu-lgc-sans.conf
/etc/fonts/conf.d/61-urw-c059.conf
/etc/fonts/conf.d/50-user.conf
/etc/fonts/snap-override/10-prefer-noto.conf
/etc/brltty.conf
/etc/geoclue/geoclue.conf
/etc/nsswitch.conf
/etc/gai.conf
/etc/hp/hplip.conf
/etc/depmod.d/ubuntu.conf
/etc/dracut.conf
/etc/fwupd/fwupd.conf
/etc/fwupd/remotes.d/lvfs-testing.conf
/etc/fwupd/remotes.d/vendor-directory.conf
/etc/fwupd/remotes.d/lvfs.conf
/etc/fuse.conf
/etc/security/access.conf
/etc/security/capability.conf
/etc/security/namespace.conf
/etc/security/pwquality.conf
/etc/security/time.conf
/etc/security/sepermit.conf
/etc/security/limits.conf
/etc/security/faillock.conf
/etc/security/pam_env.conf
/etc/security/pwhistory.conf
/etc/security/group.conf
/etc/security/limits.d/10-gamemode.conf
/etc/security/limits.d/25-pw-rlimits.conf
/etc/security/limits.d/10-coredump-debian.conf
/etc/hdparm.conf
/etc/sane.d/leo.conf
/etc/sane.d/coolscan.conf
/etc/sane.d/epjitsu.conf
/etc/sane.d/u12.conf
/etc/sane.d/s9036.conf
/etc/sane.d/qcam.conf
/etc/sane.d/dll.conf
/etc/sane.d/umax.conf
/etc/sane.d/test.conf
/etc/sane.d/pie.conf
/etc/sane.d/umax_pp.conf
/etc/sane.d/net.conf
/etc/sane.d/cardscan.conf
/etc/sane.d/gphoto2.conf
/etc/sane.d/hp5400.conf
/etc/sane.d/matsushita.conf
/etc/sane.d/mustek_usb.conf
/etc/sane.d/artec_eplus48u.conf
/etc/sane.d/dc240.conf
/etc/sane.d/stv680.conf
/etc/sane.d/ma1509.conf
/etc/sane.d/artec.conf
/etc/sane.d/canon.conf
/etc/sane.d/sm3840.conf
/etc/sane.d/saned.conf
/etc/sane.d/snapscan.conf
/etc/sane.d/sharp.conf
/etc/sane.d/abaton.conf
/etc/sane.d/kodakaio.conf
/etc/sane.d/teco3.conf
/etc/sane.d/p5.conf
/etc/sane.d/st400.conf
/etc/sane.d/canon_dr.conf
/etc/sane.d/gt68xx.conf
/etc/sane.d/v4l.conf
/etc/sane.d/teco1.conf
/etc/sane.d/dell1600n_net.conf
/etc/sane.d/dmc.conf
/etc/sane.d/mustek_pp.conf
/etc/sane.d/nec.conf
/etc/sane.d/coolscan3.conf
/etc/sane.d/avision.conf
/etc/sane.d/canon_lide70.conf
/etc/sane.d/microtek2.conf
/etc/sane.d/kvs1025.conf
/etc/sane.d/magicolor.conf
/etc/sane.d/airscan.conf
/etc/sane.d/canon630u.conf
/etc/sane.d/canon_pp.conf
/etc/sane.d/microtek.conf
/etc/sane.d/genesys.conf
/etc/sane.d/hp3900.conf
/etc/sane.d/xerox_mfp.conf
/etc/sane.d/tamarack.conf
/etc/sane.d/fujitsu.conf
/etc/sane.d/lexmark_x2600.conf
/etc/sane.d/agfafocus.conf
/etc/sane.d/rts8891.conf
/etc/sane.d/pixma.conf
/etc/sane.d/sceptre.conf
/etc/sane.d/sp15c.conf
/etc/sane.d/hp.conf
/etc/sane.d/apple.conf
/etc/sane.d/epson.conf
/etc/sane.d/plustek.conf
/etc/sane.d/lexmark.conf
/etc/sane.d/teco2.conf
/etc/sane.d/pieusb.conf
/etc/sane.d/hp4200.conf
/etc/sane.d/escl.conf
/etc/sane.d/mustek.conf
/etc/sane.d/epson2.conf
/etc/sane.d/bh.conf
/etc/sane.d/coolscan2.conf
/etc/sane.d/kodak.conf
/etc/sane.d/hpsj5s.conf
/etc/sane.d/plustek_pp.conf
/etc/sane.d/ibm.conf
/etc/sane.d/umax1220u.conf
/etc/sane.d/dc210.conf
/etc/sane.d/hs2p.conf
/etc/sane.d/dc25.conf
/etc/sane.d/ricoh.conf
/etc/sane.d/epsonds.conf
/etc/alsa/conf.d/50-pipewire.conf
/etc/alsa/conf.d/99-pipewire-default.conf
/etc/sudo.conf

--- RINGKASAN AUDIT ---
Total file ditemukan: 316
Error dicatat di    : audit-error.log
ANALISIS SINGKAT:
Pemisahan stdout (>) dan stderr (2>) penting agar daftar konfigurasi tidak kotor oleh pesan error. 
Hal ini memudahkan penghitungan file dan memastikan log error tersimpan khusus untuk pengecekan hak akses.
```


---
## Tugas Praktikum 3

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ cat << 'EOF' > ~/bin/daily-healthcheck
> LOG_FILE="$HOME/healthcheck-$(date +%F).log"
> header() {
    echo -e "\n=============================="
    echo " $1"
    echo "=============================="
}
> {
    header "WAKTU & IDENTITAS"
    echo "Waktu    : $(date)"
    echo "Hostname : $HOSTNAME"
    echo "User     : $USER"
    echo "Shell    : $SHELL"

    header "STATUS SISTEM"
    echo "Uptime   :"
    uptime -p
    echo -e "\nMemori   :"
    free -h
    echo -e "\nDisk Root (/):"
    df -h / | grep "^/"

    header "RIWAYAT PERINTAH TERAKHIR"
    history 10
} | tee "$LOG_FILE"
> if [ ${PIPESTATUS[0]} -eq 0 ]; then
    echo -e "\n[SUKSES] Laporan disimpan di: $LOG_FILE"
else
    echo -e "\n[GAGAL] Terjadi kesalahan saat pengambilan data." >&2
fi
EOF
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ chmod +x ~/bin/daily-healthcheck
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ daily-healthcheck
daily-healthcheck: command not found
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ ls
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ source ~/.bashrc
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ daily-healthcheck
daily-healthcheck: command not found
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ export PATH="$HOME/bin:$PATH"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum3$ daily-healthcheck

==============================
 WAKTU & IDENTITAS
==============================
Waktu    : Mon Apr 13 13:56:17 WIB 2026
Hostname : qifor-QEMU-Virtual-Machine
User     : qifor
Shell    : /bin/bash

==============================
 STATUS SISTEM
==============================
Uptime   :
up 59 minutes

Memori   :
               total        used        free      shared  buff/cache   available
Mem:           3.3Gi       1.3Gi       580Mi        80Mi       1.7Gi       2.0Gi
Swap:          3.8Gi          0B       3.8Gi

Disk Root (/):
/dev/vda2        24G   13G   10G  56% /

==============================
 RIWAYAT PERINTAH TERAKHIR
==============================
  216  cd praktikum3
  217  clear
  218  cat << 'EOF' > ~/bin/daily-healthcheck
LOG_FILE="$HOME/healthcheck-$(date +%F).log"
header() {
    echo -e "\n=============================="
    echo " $1"
    echo "=============================="
}
{
    header "WAKTU & IDENTITAS"
    echo "Waktu    : $(date)"
    echo "Hostname : $HOSTNAME"
    echo "User     : $USER"
    echo "Shell    : $SHELL"

    header "STATUS SISTEM"
    echo "Uptime   :"
    uptime -p
    echo -e "\nMemori   :"
    free -h
    echo -e "\nDisk Root (/):"
    df -h / | grep "^/"

    header "RIWAYAT PERINTAH TERAKHIR"
    history 10
} | tee "$LOG_FILE"
if [ ${PIPESTATUS[0]} -eq 0 ]; then
    echo -e "\n[SUKSES] Laporan disimpan di: $LOG_FILE"
else
    echo -e "\n[GAGAL] Terjadi kesalahan saat pengambilan data." >&2
fi
EOF

  219  chmod +x ~/bin/daily-healthcheck
  220  daily-healthcheck
  221  ls
  222  source ~/.bashrc
  223  daily-healthcheck
  224  export PATH="$HOME/bin:$PATH"
  225  daily-healthcheck

[SUKSES] Laporan disimpan di: /home/qifor/healthcheck-2026-04-13.log
```

---
## Tugas Praktikum 4

```bash
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ touch "laporan bulanan.txt"
touch "backup [utama] config.conf"
touch "data-01.log"
touch "data-02.log"
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ ls -F
'backup [utama] config.conf'   data-01.log   data-02.log  'laporan bulanan.txt'
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ ls laporan bulanan.txt
ls: cannot access 'laporan': No such file or directory
ls: cannot access 'bulanan.txt': No such file or directory
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ ls "laporan bulanan.txt"
'laporan bulanan.txt'
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ echo *.log
data-01.log data-02.log
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ echo "backup [utama] config.conf"
backup [utama] config.conf
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ mkdir -p ../backup-aman
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ for file in *; do
    cp -- "$file" "../backup-aman/"
done
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum/praktikum4$ cd ..
tar -czf "arsip-praktikum-$(date +%F).tar.gz" backup-aman
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum$ history | tail -n 20 > riwayat-arsip.txt
qifor@qifor-QEMU-Virtual-Machine:~/praktikum-os/week07/tugas_praktikum$ cat << 'EOF' > refleksi-quoting.txt
PENTINGNYA QUOTING DI BASH:
1. Menghindari "Word Splitting": Tanpa kutip, spasi dalam nama file dianggap sebagai pemisah argumen.
2. Mencegah Interpretasi Wildcard: Karakter seperti [ ] atau * bisa disalahartikan oleh shell jika tidak dilindungi kutip.
3. Keamanan Variabel: Menggunakan "$file" memastikan isi variabel diproses secara utuh meskipun mengandung karakter aneh.
EOF
```