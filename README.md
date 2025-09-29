# Konfigurasi-Hot-Standby-Router-Protocol

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Contributors](https://img.shields.io/badge/contributors-1-green.svg)
![Forks](https://img.shields.io/badge/forks-0-lightgrey.svg)
![Stars](https://img.shields.io/badge/stars-0-yellow.svg)

Proyek ini mengimplementasikan Hot Standby Router Protocol (HSRP) untuk mencapai redundansi dan ketersediaan tinggi pada jaringan. HSRP memungkinkan multiple router untuk bekerja sama sebagai satu gateway virtual, memastikan konektivitas jaringan tetap berjalan meskipun terjadi kegagalan perangkat.

## 🌐 Topologi Jaringan

```
      +-------+
      |  PC1  |
      +-------+
         |
         | 192.168.1.0/24
         |
+--------+--------+
|        |        |
|   R1   |   R6   |   R7
|        |        |
+--------+--------+
         |
         | 10.0.0.0/24
         |
    Jaringan Lain
```
<img width="827" height="460" alt="image" src="https://github.com/user-attachments/assets/1870f35c-f55b-4de3-86c4-5f33d90cd08b" />

| Perangkat | Interface | IP Address | Subnet Mask | Peran HSRP |
|-----------|-----------|------------|-------------|------------|
| PC1 | - | 192.168.1.10 | 255.255.255.0 | - |
| R1 | GigabitEthernet0/0 | 192.168.1.2 | 255.255.255.0 | Active (Priority 110) |
| R1 | GigabitEthernet0/1 | 10.0.0.1 | 255.255.255.0 | - |
| R6 | GigabitEthernet0/0 | 192.168.1.3 | 255.255.255.0 | Standby (Priority 100) |
| R6 | GigabitEthernet0/1 | 10.0.0.2 | 255.255.255.0 | - |
| R7 | GigabitEthernet0/0 | 192.168.1.4 | 255.255.255.0 | Standby (Priority 90) |
| R7 | GigabitEthernet0/1 | 10.0.0.3 | 255.255.255.0 | - |
| Virtual Gateway | - | 192.168.1.1 | 255.255.255.0 | - |

## 📝 Penjelasan Konfigurasi

### Konfigurasi HSRP

Pada proyek ini, kita mengimplementasikan HSRP dengan tiga router yang memiliki peran berbeda:

1. **Router 1 (R1)**: Dikonfigurasi sebagai router active dengan priority 110 (tertinggi)
2. **Router 6 (R6)**: Dikonfigurasi sebagai router standby pertama dengan priority 100
3. **Router 7 (R7)**: Dikonfigurasi sebagai router standby kedua dengan priority 90

### Parameter HSRP

- **standby 1 ip 192.168.1.1**: Mengatur virtual IP address untuk group HSRP 1
- **standby 1 priority 110**: Mengatur priority router (semakin tinggi semakin berpeluang menjadi active)
- **standby 1 preempt**: Memungkinkan router dengan priority lebih tinggi untuk mengambil alih peran active
- **standby 1 name HSRP_GROUP1**: Memberikan nama pada group HSRP untuk identifikasi

### Cara Kerja HSRP

1. Router dengan priority tertinggi akan menjadi router active
2. Router active akan menangani semua traffic yang ditujukan ke virtual IP address
3. Router standby akan memonitor status router active melalui pesan hello
4. Jika router active gagal, router standby dengan priority tertinggi berikutnya akan mengambil alih peran active
5. PC1 menggunakan virtual IP address (192.168.1.1) sebagai default gateway, sehingga tidak perlu mengubah konfigurasi saat terjadi failover

---

**luqmanaru**
