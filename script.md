
# Dokumentasi Skrip CLI Praktikum Jaringan Komputer - The Wired

Berikut adalah kumpulan perintah CLI yang dijalankan pada setiap node untuk menyelesaikan rangkaian soal praktikum.

---

## Soal 1 & 3: Konfigurasi IP Interface & Routing Antar Entitas

### Router Lain (ROUTER)
```bash
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

### Alice (Client Switch 1)

```bash
auto eth0
iface eth0 inet static
address 192.237.1.4
netmask 255.255.255.0
gateway 192.237.1.1

```

### Mika (Client Switch 1)

```bash
auto eth0
iface eth0 inet static
address 192.237.1.5
netmask 255.255.255.0
gateway 192.237.1.1

```

### Chisa (Client Switch 2)

```bash
auto eth0
iface eth0 inet static
address 192.237.2.6
netmask 255.255.255.0
gateway 192.237.2.2

```

### Knights (Client Switch 3)

```bash
auto eth0
iface eth0 inet static
address 192.237.3.7
netmask 255.255.255.0
gateway 192.237.3.3

```

### Eiri (Client Switch 3)

```bash
auto eth0
iface eth0 inet static
address 192.237.3.8
netmask 255.255.255.0
gateway 192.237.3.3

```

---

## Soal 2 & 4: Koneksi Internet Publik, NAT Masquerade, dan DNS Resolver

### Router Lain (ROUTER)

```bash
auto eth0
iface eth0 inet dhcp
up sysctl -w net.ipv4.ip_forward=1
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

```

### Masing-masing Client (Alice, Mika, Chisa, Knights, Eiri)

```bash
echo nameserver 8.8.8.8 > /etc/resolv.conf
# (Khusus Eiri menggunakan 8.8.4.4)

```

---

## Soal 5: Script Verifikasi Anti-Restart (`/root/cek_status.sh`)

### Router Lain (ROUTER)

```bash
cat << 'EOF' > /root/cek_status.sh
#!/bin/bash
echo "=== RINGKASAN INTERFACE (ip -br a) ==="
ip -br a
echo
echo "=== STATUS TABEL NAT (iptables -t nat -L -v -n) ==="
iptables -t nat -L -v -n
EOF

chmod +x /root/cek_status.sh

```

---

## Soal 6: Analisis Anomali Traffic pada Segmen Mika

### Node Mika

```bash
wget -O traffic.zip "[https://drive.google.com/drive/folders/1ZjFvWIjVAQAjE9pPthm7V_bGyaSt93lY?usp=sharing](https://drive.google.com/drive/folders/1ZjFvWIjVAQAjE9pPthm7V_bGyaSt93lY?usp=sharing)"
unzip traffic.zip

```

*Lakukan packet sniffing di Wireshark dengan filter: `dns || icmp*`

---

## Soal 7: Konfigurasi FTP Server pada Node Chisa

### Node Chisa

```bash
mkdir -p /var/wired/data
adduser -h /var/wired/data -D alice
adduser -h /var/wired/data -D mika
adduser -h /var/wired/data -D eiri

```

### Node Alice (Pengujian)

```bash
echo "signal from alice" > signal_alice.txt
lftp 192.237.2.6
> user alice
> put signal_alice.txt

```

---

## Soal 8: Upload Dokumen Intelijen dari Knights ke FTP Chisa

### Node Knights

```bash
wget [https://drive.google.com/drive/folders/1tvZpueSH9E3GWwXM6KNnM64Y5wNoIAYP?usp=sharing](https://drive.google.com/drive/folders/1tvZpueSH9E3GWwXM6KNnM64Y5wNoIAYP?usp=sharing) -O laporan.doc
lftp 192.237.2.6
> user alice
> put laporan.doc

```

---

## Soal 9: Pengujian Akses Read-Only oleh Mika pada FTP Chisa

### Node Mika

```bash
lftp 192.237.2.6
> user mika
> get protokol_tujuh.txt
> put file_baru.txt  # (Akan merespon error: 550 Permission denied)

```

---

## Soal 10: Uji Ketahanan Latensi Jaringan dari Knights ke Chisa

### Node Knights

```bash
ping -c 77 -s 128 -i 0.3 192.237.2.6

```

---

## Soal 11: Analisis Kelemahan Telnet

### Node Chisa

```bash
adduser -D phantom_user
echo "phantom_user:wired_ghost" | chpasswd

```

### Node Eiri

```bash
telnet 192.237.2.6

```

---

## Soal 12: Pemindaian Port Menggunakan Netcat

### Node Knights (Listener)

```bash
nc -l -p 7777

```

### Node Alice (Port Scanning)

```bash
nc -zv 192.237.3.7 7777
nc -zv 192.237.3.7 80
nc -zv 192.237.3.7 22

```

---

## Soal 14 s.d. 20: Validasi Socket Server Analisis PCAP

Gunakan perintah Netcat berikut untuk memasukkan flag/jawaban hasil analisis Wireshark:

* **Soal 14 (Brute Force Analysis):** `nc [IP_Group] 3401`
* **Soal 15 (USB Keystroke Decoding):** `nc [IP_Group] 3402`
* **Soal 16 (FTP Credential Theft):** `nc [IP_Group] 3403`
* **Soal 17 (HTTP Malware Retrieval):** `nc [IP_Group] 3404`
* **Soal 18 (SMB Lateral Transfer):** `nc [IP_Group] 3405`
* **Soal 19 (SMTP Threat Inspection):** `nc [IP_Group] 3406`
* **Soal 20 (TLS Decrypted Stream):** `nc [IP_Group] 3407`

```

```
