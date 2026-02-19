<div align="center">
  <img src="https://readme-typing-svg.herokuapp.com?size=30&color=ff2400&center=true&vCenter=true&width=650&lines=Secure+Shell+(SSH)+Server+di+Debian">
</div>

## Apa Itu SSH?

SSH adalah singkatan dari Secure Shell, yaitu cara untuk mengakses komputer atau server dari jarak jauh lewat jaringan dengan aman. Dengan SSH, kita bisa menjalankan perintah dan mengelola sistem seolah-olah sedang berada langsung di depan komputer tersebut. Keamanannya dijaga dengan sistem enkripsi, sehingga data seperti username dan password tidak mudah disadap oleh orang lain. Karena aman dan praktis, SSH banyak digunakan untuk mengelola server, belajar Linux, dan kebutuhan jaringan maupun keamanan siber.

## Penginstalan dan Penggunaan SSH

Sebelum masuk ke proses instalasi, pastikan server yang akan digunakan sudah terhubung ke jaringan dan bisa mengakses internet. Pada bagian ini, kita akan mulai memasang layanan SSH agar perangkat dapat diakses dari jarak jauh dengan lebih mudah dan aman. Ikuti langkah-langkah berikut secara berurutan supaya proses instalasi berjalan lancar. Untuk penginstallanya sangatlah mudah kalian dapat melakukan Update terlebih dahulu pada server kalian lalu menginstall ssh server.

```bash
apt update
apt install ssh -y
```

Setelah melakukan Penginstalan kalian dapat memeriksa status ssh menggunakan perintah berikut ini:

```bash
systemctl status ssh
systemctl enable ssh #Jika statusnya disable
systemctl start ssh #Jika statusnya masih di stop
```

SSH sacara default berjalan di port 22. Jika status SSH sudah aktif dan berjalan maka kita bisa langsung mencoba meremote shell. Kita dapat melakukan Test remote SSH menggunakan perintah ini:

```bash
#ssh user@ip
ssh kaguai@192.168.10.10
```

------------------------------------------------------------------
Setelah penginstalan SSH secara default kita hanya bisa meremote user demi keamanan agar pihak luar tidak dapat meremote root. Namun jika kite memerlukan akses root secara langsung disaat melakukan ssh kita dapat mengubah konfigurasi file sshd_config pada bagian Root login permission . ***Note**: lebih aman kita melakukan ssh ke User lalu dari terminal kita masuk ke root dengan perintah `sudo su` atau `su`.
```bash
nano /etc/ssh/sshd_config
```
Ubah Value PermitRootLogin menjadi yes Untuk mengizinkan ssh dengan root

```txt
 32 #LoginGraceTime 2m
 33 PermitRootLogin yes
 34 #StrictModes yes
 35 #MaxAuthTries 6
 36 #MaxSessions 10
```

------------------------------------------------------------------------
Atau kalian juga bisa membatasi User untuk bisa Di SSH. Cara Konfigurasinya pun sama pada file sshd_config yang disimpan di direktori `/etc/ssh` . Untuk membati user yang bisa dapat di SSH kalian dapat menambahakan `AllowUsers` pada baris paling bawah pada file config `sshd_config`. Kalian bisa Tuliskan nama user yang diizinkan untuk Di SSH. Berikut contoh nya:

```bash
nano /etc/ssh/sshd_config
```
```txt
...
AllowUsers kaguai admin
```

Dengan menambahkan `AllowUsers` dan memasuksan nama user maka hanya user tersebut yang bisa kita ssh. Pada contoh tersebut user yang dizinkan untuk di SSH ada user dengan nama kaguai dan admin. Jadi misal pada server kalian memiliki nama user lain maka tidak dapat di SSH.

-----------------------------------------------------------------------
Selain itu kita dapat juga merubah default port pada SSH dari 22 yaitu ke port yang kita inginkan, Untuk mengubahnya pun kita dapat melakukannya dengan mengkonfigurasi file sshd_config

Ubah Value Port dari 22 menjadi nomor port yang kalian inginkan

```txt
 12 Include /etc/ssh/sshd_config.d/*.conf
 13
 14 Port 1010
```

Jika Nomor port ssh diubah maka dalam melakukan koneksi atau remote shell ada tambahan parameter yaitu `-p` sehingga seperti ini
```bash
ssh root@192.168.10.10 -p 1010
```

-------------------------------------------------------------------
Lalu kalian juga dapat membuat agar setiap ssh tidak perlu memasukan password. Tetapi SSH kita akan divalidasi menggunakan key yang dibuat. Ini dapat kita lakukan jika kalian mengirim file Key Public ke server terlebih dahulu sebelum menonaktifkan `PasswordAuthenticatin`. Ini dapat kita lakukan konfigurasi pada file sshd_config.
```bash
nano /etc/ssh/sshd_config
```
```txt
 38 PubkeyAuthentication yes
 39
 40 # Expect .ssh/authorized_keys2 to be disregarded by default in future.
 41 #AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2
 42
 43 #AuthorizedPrincipalsFile none
 44
 45 #AuthorizedKeysCommand none
 46 #AuthorizedKeysCommandUser nobody
 47
 48 # For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
 49 #HostbasedAuthentication no
 50 # Change to yes if you don't trust ~/.ssh/known_hosts for
 51 # HostbasedAuthentication
 52 #IgnoreUserKnownHosts no
 53 # Don't read the user's ~/.rhosts and ~/.shosts files
 54 #IgnoreRhosts yes
 55
 56 # To disable tunneled clear text passwords, change to no here!
 57 PasswordAuthentication no
```

Setelah itu kalian dapat mencoba SSH dan login tanpa password tapi nanti server akan memverivikasi priv key yang anda punya. Untuk membuat File Key kalian dapat membaca tentang SSH-Keygen. Kalian juga bisa belajar lainnya

| Materi | URL |
|---------|-------|
| SSH Keygen | https://github.com/Kaguai10/ssh-server/blob/main/SSH-Keygen.md | 
| SSH Port Forwarding | https://github.com/Kaguai10/ssh-server/blob/main/SSH-Port-Forwarding.md |
| SCP dan SFTP | https://github.com/Kaguai10/ssh-SecureShell/blob/main/SCP%26SFTP.md |

**Note: Setiap setelah melakukan Konfigurasi jangan lupa untuk merestart SSH `systemctl restart ssh`. Merestart SSH diwajibkan agar konfigurasi kita ditetapkan, service SSH diperbarui sesuai config. dan agar Konfigurasi berjalan**

Kalian juga bisa mempelajari manual-nya SSH, dengan membaca dari manual SSH menggunakan perintah dibawah ini
```bash
man ssh
```

