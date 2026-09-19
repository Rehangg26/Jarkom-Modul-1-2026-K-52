# Jarkom-Modul-1-2026-K-52

##  Anggota Kelompok

| No. | Nama | NRP |
| :---: | :--- | :---: |
| 1 | Reyhan Adi Satrio | 5027251080 |
| 2 | Muhammad Rifki Pribadi | 5027251087 |

**1**.**Untuk mempersiapkan pembangunan The Wired, Lain yang berperan sebagai Router membuat tiga Switch/Gateway: Switch 1 menuju dua Entitas yaitu Alice dan Mika, Switch 2 menuju Chisa, sedangkan Switch 3 menuju Knights dan Eiri. Kelima Entitas tersebut dikonfigurasi sebagai Client di GNS3. [GUNAKAN PREFIX IP MASING-MASING KELOMPOK]**
<img width="972" height="731" alt="image" src="https://github.com/user-attachments/assets/7ce50026-695c-486d-94ae-a81adcaaaaba" />

**2**. **Karena menurut Lain pada saat itu The Wired masih terisolasi dari dunia luar, konfigurasikan router Lain agar dapat tersambung langsung ke jaringan internet publik melalui NAT/DHCP pada interface eth0.**
   
**Router Lain (ROUTER):**
```
# Static config for eth0
iface eth0 inet dhcp
  up sysctl -w net.ipv4.ip_forward=1
  up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

```

**3**. **Setelah router Lain terhubung ke internet, pastikan seluruh Entitas (Client) di bawah Switch 1, Switch 2, dan Switch 3 dapat saling terhubung dan berkomunikasi satu sama lain melalui konfigurasi routing.**
Router Lain (ROUTER):
```
auto eth1
iface eth1 inet static
    address 192.237.1.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.237.2.2
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 192.237.3.3
    netmask 255.255.255.0
```

Alice (Switch 1):
```
auto eth0
iface eth0 inet static
    address 192.237.1.4
    netmask 255.255.255.0
    gateway 192.237.1.1
```

Mika (Switch 1):
```
auto eth0
iface eth0 inet static
    address 192.237.1.5
    netmask 255.255.255.0
    gateway 192.237.1.1
```

Chisa (Switch 2):
```
auto eth0
iface eth0 inet static
    address 192.237.2.6
    netmask 255.255.255.0
    gateway 192.237.2.2
```

Knights (Switch 3):
```
auto eth0
iface eth0 inet static
    address 192.237.3.7
    netmask 255.255.255.0
    gateway 192.237.3.3
```

Eiri (Switch 3):
```
auto eth0
iface eth0 inet static
    address 192.237.3.8
    netmask 255.255.255.0
    gateway 192.237.3.3
```
**4**. **Lain ingin agar setiap Entitas (Client) memiliki kemandirian di The Wired. Konfigurasikan firewall/iptables (NAT Masquerade) dan DNS resolver agar setiap Client dapat terhubung ke internet secara mandiri (dapat melakukan ping ke 8.8.8.8 dan membuka domain web google.com).**

Setelah kami mengkonfigurasi iptables dari Router Lain, selanjutnya kami mengkonfigurasi DNS resolver dari semua client agar terhubung mandiri ke internet.

Alice (Switch 1):
```
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Mika (Switch 1):
```
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Chisa (Switch 2):
```
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Knights (Switch 3):
```
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Eiri (Switch 3):
```
   echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

**5**. **Eiri tetap berupaya menanamkan kekacauan ke dalam jaringan. Untuk mengantisipasi restart tiba-tiba, pastikan seluruh konfigurasi jaringan tidak hilang saat semua node di-restart. Buat script verifikasi di /root/cek_status.sh pada router Lain yang menampilkan ringkasan interface (ip -br a) dan status tabel NAT (iptables -t nat -L -v -n) setelah reboot.**
```
root@Lain:~# cat cek_status.sh
#!/bin/bash
echo "=== RINGKASAN INTERFACE (ip -br a) ==="
ip -br a
echo ""
echo "=== STATUS TABEL NAT (iptables -t nat -L -v -n) ==="
iptables -t nat -L -v -n
```

**6**. **Mika mencurigai adanya anomali traffic pada segmen jaringannya. Jalankan generator traffic berikut (link file) pada node Mika, lalu lakukan packet sniffing menggunakan Wireshark pada interface node Mika. Terapkan display filter khusus untuk menyaring paket yang berprotokol DNS atau ICMP. Tunjukkan screenshot hasil filter beserta ringkasan paket yang lolos.**

```
wget -O traffic.zip "https://drive.google.com/drive/folders/1ZjFvWIjVAQAjE9pPthm7V_bGyaSt93lY?usp=sharing"
unzip traffic.zip
bash traffic.sh
```
Setelahnya, kami menjalankan Wireshark dengan filter: dns || ICMP untuk menyaring paket yang berprotokol DNS atau ICMP.
Dokum:
<img width="878" height="532" alt="image-1" src="https://github.com/user-attachments/assets/a7b4ee1a-42d1-49c2-a1f2-2a7ed22a600d" />


**8**. **Kelompok rahasia Knights perlu mengirimkan dokumen laporan intelijen ke FTP Server Chisa. Lakukan koneksi FTP client dari node Knights ke FTP Server Chisa menggunakan akun alice. Upload file berikut (link file). Analisis sesi Wireshark dan sebutkan: perintah FTP untuk upload (STOR), kode status sukses server (226), dan port data TCP yang dinegosiasikan pada mode PASV.**

```
Knights :~ # 1ftp 192.237.2.6
lftp 192.237.2.6 :- > user alice
Password:
lftp alice@192.237.2.6 :~ > put laporan.doc
303205 bytes transferred
lftp alice@192.237.2.6:/>
```

<img width="602" height="78" alt="Picture4" src="https://github.com/user-attachments/assets/d40285d6-18e7-4e78-8e46-9a67e7871dbd" />

**9**. **Mika mengakses dokumen Protokol Tujuh di (link file) dari FTP Server Chisa. Dari node Mika, unduh file tersebut menggunakan akun mika. Setelah itu, buktikan pembatasan read-only dengan mencoba mengunggah file baru dari akun mika, dan tunjukkan pesan error respon server (error 550 Permission denied) saat mika mencoba melakukan upload.**

Yang perlu kami lakukan adalah mengunduh file Protokol Tujuh dari node Mika serta membuktikan pembatasan read-only

```
Mika :~ # 1ftp 192.237.2.6
lftp 192.237.2.6 :~ > user eiri
Password:
lftp eiri@192.237.2.6 :~ > 1s
ls: Login failed: 530 Permission denied.
lftp eiri@192.237.2.6 :~ >
```

<img width="601" height="80" alt="Picture3" src="https://github.com/user-attachments/assets/06e583b2-67a0-46c6-bfef-64c7f002ec75" />

**10**. **Knights melancarkan uji ketahanan koneksi ke server Chisa untuk menguji latensi jaringan The Wired. Kirimkan paket ping dari node Knights ke node Chisa dengan payload khusus 128 bytes dan interval 0.3 detik sebanyak 77 paket 
(ping -c 77 -s 128 -i 0.3 <IP_Chisa>). Buka Wireshark, catat nilai ICMP Type dan Code untuk Echo Request vs Echo Reply, serta analisis packet loss dan RTT (min/avg/max).**

Yang perlu kami lakukan adalah mengirim paket ping dari node Knight ke node Chisa sebagai uji ketahanan koneksi dan mencatat package yang akan muncul di Wireshark

```
Knight :~# ping -c 77 -s 128 -i 0.3 192.237.2.6
```

**packet loss dan RTT (min/avg/max).**

<img width="691" height="62" alt="Screenshot 2026-09-17 143712" src="https://github.com/user-attachments/assets/b54a7595-fa48-42fe-a89d-af49ed22c6b2" />

**nilai ICMP Type dan Code untuk Echo Request vs Echo Reply**

<img width="1236" height="915" alt="Screenshot 2026-09-17 143728" src="https://github.com/user-attachments/assets/453b4a99-5d78-43c4-8f42-587a7b0e93ea" />


**11**. **Buktikan kelemahan protokol Telnet dengan membuat akun phantom_user dan password wired_ghost pada layanan telnetd di node Chisa. Lakukan login Telnet dari node Eiri ke node Chisa dan tangkap sesi menggunakan Wireshark. Tunjukkan kredensial plain text melalui fitur Follow TCP Stream, serta jelaskan mengapa setiap karakter terkirim dalam paket TCP terpisah.**

Di node Chisa, kita perlu menjalankan:
```
adduser -D phantom_user
echo "phantom_user:wired_ghost" | chpasswd
telnetd -l /bin/login
```

Selanjutya di node Eiri kita menjalankan:
```
telnet 192.237.2.6
```

<img width="695" height="830" alt="Screenshot 2026-09-17 150340" src="https://github.com/user-attachments/assets/8b6d995f-61c6-4ec1-8774-14a577575e92" />

<img width="1917" height="1027" alt="Screenshot 2026-09-17 150507" src="https://github.com/user-attachments/assets/42ce5dcc-c24a-4211-b18b-ef4cb4bc2509" />


**12**. **Alice mencurigai Knights menjalankan beberapa layanan rahasia di node-nya. Lakukan pemindaian port dari node Alice ke node Knights menggunakan Netcat (nc) untuk memeriksa port 22 (SSH) dan 80 (HTTP) dalam keadaan terbuka, serta port rahasia 7777 dalam keadaan tertutup. Analisis di Wireshark perbedaan TCP Flag yang dikembalikan antara port terbuka (SYN-ACK) dengan port tertutup (RST-ACK).**

Untuk di node Knight kami menjalankan:
```
Knights:~# nc -l -p 7777
```
(nc -l -p 7777 kami jalankan agar ada port yang terbuka)

Saat kita cek di node Alice:
```
Alice:~# nc -zv 192.237.3.7 7777
Connection to 192.237.3.7 7777 port [tcp/*] succeeded!
```

<img width="1901" height="117" alt="image-2" src="https://github.com/user-attachments/assets/6397e588-c419-45d0-ad22-38abb312ab98" />

Untuk port 22 dan 80:
```
Alice:~# nc -zv 192.237.3.7 80
nc: connect to 192.237.3.7 port 80 (tcp) failed: Connection refused
Alice:~# nc -zv 192.237.3.7 22
nc: connect to 192.237.3.7 port 22 (tcp) failed: Connection refused
```

<img width="1902" height="138" alt="image-3" src="https://github.com/user-attachments/assets/6f00f636-d04d-4c16-8db3-5143e4c818a5" />

disini dapat dilihat pada port 22 dan 80 tidak ada Flag yang dikembalikan, sehingga tidak ada respon.

**13**. **Lain memerintahkan agar administrasi jarak jauh menggunakan SSH secara aman tanpa password. Install OpenSSH server pada node Knights, buat pasangan kunci SSH (ssh-keygen) pada node Mika untuk user mika_admin, dan konfigurasikan public key authentication (PasswordAuthentication no). Lakukan koneksi SSH dari node Mika ke node Knights, tangkap sesi menggunakan Wireshark, identifikasi paket Protocol Version Exchange dan Key Exchange, serta jelaskan mengapa kredensial tidak terlihat dalam bentuk teks terbuka seperti pada Telnet.**

```
(1/2) Installing openssh-server-common (10.2_p1-r0)
(2/2) Installing openssh-server (10.2_p1-r0)
Executing busybox-1.37.0-r30.trigger
OK: 83.4 MiB in 167 packages
Knights:~# ssh-keygen -A
ssh-keygen: generating new host keys: RSA ECDSA ED25519
Knights:~# /usr/sbin/sshd
Knights:~# vi /etc/ssh/sshd_config
7 / 8
README.md
2026-09-17
Knights:~# killall sshd
Knights:~# /usr/sbin/sshd
Knights:~# netstat -an | grep 22
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp6       0      0 :::22                   :::*                    LISTEN
```

(Tidak selesai)
