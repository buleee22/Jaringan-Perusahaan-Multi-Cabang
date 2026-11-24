# 🌐 DellNet Secure
### Jaringan Perusahaan Multi-Cabang yang Aman Berbasis Cisco (VLAN • OSPF • DHCP Relay • NAT • ACL • SSH)

Project simulasi jaringan perusahaan dengan 1 kantor pusat (HQ) dan 2 kantor cabang, dibangun dengan **Cisco Packet Tracer** (kompatibel juga dengan GNS3 / CML). Cocok untuk portofolio, tugas kuliah, dan latihan CCNA.

## 🎯 Tujuan
- Segmentasi jaringan HQ dengan VLAN dan inter-VLAN routing (router-on-a-stick)
- Menghubungkan HQ dan cabang dengan routing dinamis OSPF
- Distribusi IP terpusat dengan DHCP + DHCP relay di cabang
- Akses internet dengan NAT/PAT
- Pengamanan: ACL untuk VLAN Tamu, SSH, port security, nonaktifkan port tak terpakai

## 🗺️ Topologi

```
                       [ R-ISP ] 8.8.8.8 (simulasi internet)
                           | 203.0.113.0/30
                           |
  192.168.110.0/24      [ R-HQ ]      192.168.120.0/24
 [PC]-[SW-A]-[R-A]--------+---------[R-B]-[SW-B]-[PC]
              10.0.0.0/30 |  10.0.0.4/30
                          | trunk (VLAN 10,20,30,99)
                       [ SW-HQ ]
                      /    |    \
                 Server  PC-Keu  PC-Tamu
                (VLAN10) (VLAN20) (VLAN30)
```

## 📋 Tabel IP Addressing

| Segmen | Network | Gateway / Perangkat |
|---|---|---|
| VLAN 10 – IT | 192.168.10.0/24 | R-HQ Gi0/0.10 = 192.168.10.1 (Server DNS/Web: .10) |
| VLAN 20 – Keuangan | 192.168.20.0/24 | R-HQ Gi0/0.20 = 192.168.20.1 |
| VLAN 30 – Tamu | 192.168.30.0/24 | R-HQ Gi0/0.30 = 192.168.30.1 |
| VLAN 99 – Manajemen | 192.168.99.0/24 | R-HQ Gi0/0.99 = 192.168.99.1, SW-HQ = .2 |
| LAN Cabang A | 192.168.110.0/24 | R-A Gi0/0 = 192.168.110.1 |
| LAN Cabang B | 192.168.120.0/24 | R-B Gi0/0 = 192.168.120.1 |
| WAN HQ–A | 10.0.0.0/30 | R-HQ Gi0/1 = .1, R-A Gi0/1 = .2 |
| WAN HQ–B | 10.0.0.4/30 | R-HQ Gi0/2 = .5, R-B Gi0/1 = .6 |
| HQ–ISP | 203.0.113.0/30 | R-ISP Gi0/0 = .1, R-HQ Gi0/3 = .2 |

> Nama interface bisa berbeda tergantung model perangkat. Sesuaikan jika perlu (mis. gunakan router dengan 4 port gigabit atau tambahkan modul).

## 🧰 Perangkat
- 4 Router (R-HQ, R-A, R-B, R-ISP), contoh: Cisco ISR 4331 / 2911 + modul
- 3 Switch (SW-HQ, SW-A, SW-B), contoh: Catalyst 2960
- 1 Server, beberapa PC

## 🚀 Cara Menjalankan
1. Buat topologi di Packet Tracer sesuai diagram dan tabel IP.
2. Buka CLI tiap perangkat, masuk `enable` lalu `configure terminal`.
3. Salin isi file di folder [`configs/`](configs) sesuai nama perangkat.
4. Set server statis: IP `192.168.10.10/24`, gateway `192.168.10.1`. PC lainnya pakai DHCP.
5. Simpan konfigurasi dengan `write memory`.
6. Jalankan skenario di [`docs/test-plan.md`](docs/test-plan.md) dan simpan bukti ke folder `screenshots/`.
7. Simpan file topologi (`.pkt`) ke folder project ini.

## 🔐 Fitur Keamanan
- SSH v2 (telnet dinonaktifkan), password terenkripsi
- ACL VLAN Tamu: hanya boleh internet, tidak bisa akses jaringan internal
- Port security di port access, port tak terpakai di-shutdown
- VLAN manajemen terpisah, native VLAN dipindah ke VLAN 99

## ⚠️ Catatan
Password pada konfigurasi (`Cisco@123`) hanya untuk lab. Jangan dipakai di lingkungan produksi.

## 📚 Teknologi & Konsep
`VLAN` `802.1Q` `Router-on-a-Stick` `OSPFv2` `DHCP Relay` `NAT/PAT` `Extended ACL` `SSH` `Port Security`

## 📄 Lisensi
MIT License
