# LAB-24-Firewall-chain-forward-2
tanggal 17 Agustus 2025

![m]()

# Langkah Konfigurasi Firewall Chain Forward pada Mikrotik

Dengan ketentuan :  

**PC dan Laptop **bisa akses internet**.   
**Kecuali YouTube dan Facebook** (diblokir pada jam kerja tertentu).**  
**IP klien yang mengakses tetap terdokumentasi.**  

---

1. Buat Address List untuk Klien (PC & Laptop).  
   kita masukkan IP klien ke address list.    

```bash
/ip firewall address-list
add address=192.168.10.2 list=KLIEN
add address=192.168.10.3 list=KLIEN
```

2. Blokir Akses YouTube & Facebook.    
   Gunakan layer7 protocol atau langsung domain list:    

```bash
/ip firewall layer7-protocol
add name=block_youtube regexp="^.+(youtube.com|ytimg.com).*\$"
add name=block_facebook regexp="^.+(facebook.com|fbcdn.net).*\$"
```

3. Buat Rule Firewall Forward.    
   Agar hanya saat **jam kerja (misal 08:00–16:00)** akses diblokir.    

```bash
/ip firewall filter
add chain=forward src-address-list=KLIEN layer7-protocol=block_youtube \
 action=drop time=8h-16h,sun,mon,tue,wed,thu,fri,sat
add chain=forward src-address-list=KLIEN layer7-protocol=block_facebook \
 action=drop time=8h-16h,sun,mon,tue,wed,thu,fri,sat
```

4. Dokumentasi Log IP Klien yang Akses.   

```bash
/ip firewall filter
add chain=forward src-address-list=KLIEN action=log log-prefix="AKSES-KLIEN "
```
# pengujian   
1. Klien masih bisa akses internet secara umum (Google, email, dll).  

![m]()

3. Akses YouTube dan Facebook terblokir saat jam kerja.  

![m]()

4. Log mencatat IP setiap klien yang melakukan request.  

![m]()

# Kesimpulan

Dengan konfigurasi firewall chain forward, administrator dapat **mengontrol akses situs tertentu**,   
berdasarkan kebutuhan. Rule firewall juga dapat diberi **jadwal waktu**.   
Dokumentasi log sangat penting untuk **memantau aktivitas klien** dan memastikan kebijakan jaringan.    


