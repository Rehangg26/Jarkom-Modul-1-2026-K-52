# Jarkom-Modul-1-2026-K-52


**1**. Untuk mempersiapkan pembangunan The Wired, Lain yang berperan sebagai Router membuat tiga Switch/Gateway: Switch 1 menuju dua Entitas yaitu Alice dan Mika, Switch 2 menuju Chisa, sedangkan Switch 3 menuju Knights dan Eiri. Kelima Entitas tersebut dikonfigurasi sebagai Client di GNS3. [GUNAKAN PREFIX IP MASING-MASING KELOMPOK]
<img width="972" height="731" alt="image" src="https://github.com/user-attachments/assets/7ce50026-695c-486d-94ae-a81adcaaaaba" />

**2**. Karena menurut Lain pada saat itu The Wired masih terisolasi dari dunia luar, konfigurasikan router Lain agar dapat tersambung langsung ke jaringan internet publik melalui NAT/DHCP pada interface eth0.
   
**Router Lain (ROUTER):**
```
# Static config for eth0
iface eth0 inet dhcp
  up sysctl -w net.ipv4.ip_forward=1
  up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

```

**3**. Setelah router Lain terhubung ke internet, pastikan seluruh Entitas (Client) di bawah Switch 1, Switch 2, dan Switch 3 dapat saling terhubung dan berkomunikasi satu sama lain melalui konfigurasi routing.

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
