# SERVER-
Membuat RAID baru (tanpa downtime) di server Dell R720xd yang menjalankan VMware ESXi7.0
❌ Masalah yang Kamu Hadapi:
| Masalah                                                            | Keterangan                                                                                                                                   |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Tidak bisa install OMSA VIB di ESXi 7.0 (R720xd)                   | Karena Dell **hanya mendukung sampai ESXi 6.5** untuk R720 – tidak ada VIB yang kompatibel                                                   |
| iDRAC hanya bisa monitoring RAID, **tidak bisa membuat RAID baru** | Ini memang keterbatasan iDRAC, kecuali iDRAC Enterprise dengan fitur **Lifecycle Controller + UEFI RAID Configuration**, tapi tetap terbatas |
| Ingin seperti server R730xd + ESXi 8.0                             | R730xd masih didukung OMSA untuk ESXi 8 → bisa create RAID via OMSA dari Windows/Linux VM                                                    |
✅ Solusi Realistis: Gunakan OMSA dalam VM + iSM (Dell iDRAC Service Module)
💡 Penjelasan:

Agar kamu bisa membuat Virtual Disk baru / RAID baru dari dalam VM, ada 2 pendekatan yang bisa digunakan tanpa menginstal apapun di ESXi 7, karena tidak ada VIB.

🔧 Opsi A: Install OMSA + iSM di Linux/Windows VM + Akses PERC Controller via iDRAC
1. Install OMSA di VM (Windows/Linux)

Sama seperti yang sudah kamu lakukan.

2. Install iSM (iDRAC Service Module) di VM juga

Dell iSM adalah agent kecil yang bisa menyambungkan OMSA di VM dengan iDRAC secara lokal

Ini akan “menjembatani” akses ke RAID controller (PERC) secara indirect – memungkinkan kontrol penuh seperti create/delete RAID

🔗 iSM download (pilih OS):

https://www.dell.com/support/home/en-us/drivers/driversdetails?driverid=h1x64
3. Setelah iSM & OMSA jalan:

Buka OMSA: https://localhost:1311

Login pakai user lokal VM

Kamu akan bisa melihat Storage > PERC H710/H710P dan create Virtual Disk baru

✅ Tidak butuh VIB di ESXi
✅ Bisa buat RAID baru tanpa downtime
✅ Tidak perlu reboot ESXi / shutdown VM

🧪 Catatan Penting:

Pastikan iSM bisa melihat iDRAC IP (192.168.8.5) dari VM kamu.

Saat instal iSM, ada opsi untuk menghubungkan ke iDRAC — pastikan diaktifkan.

Jika berhasil, OMSA akan “menyatu” dengan iDRAC, dan kamu akan melihat seluruh konfigurasi RAID controller.

🔧 Opsi B (Advanced): Jalankan OMSA via LiveCD/USB (Tanpa Ganggu ESXi)

Jika Opsi A benar-benar tidak berhasil (karena iSM tidak mendeteksi PERC via iDRAC), kamu bisa:

Download Dell OMSA LiveCD berbasis Linux (CentOS)

Boot LiveCD via iDRAC virtual media (tanpa cabut disk, tanpa ganggu ESXi)

Jalankan OMSA langsung dari LiveCD

Create RAID dari CLI/web GUI

Shutdown LiveCD → kembali ke ESXi tanpa reboot host

⚠️ Ini lebih teknis dan tidak semua admin nyaman. Tapi ini benar-benar non-intrusif ke sistem ESXi.

| Cara                    | Cocok untuk Kamu?                                                            |
| ----------------------- | ---------------------------------------------------------------------------- |
| 🔥 **OMSA + iSM di VM** | ✅ Solusi terbaik & paling mudah — memungkinkan create RAID tanpa install VIB |
| iDRAC langsung          | ❌ Tidak bisa create RAID (kecuali reboot ke Lifecycle Controller)            |
| OpenManage Enterprise   | ❌ Monitoring saja, tidak bisa create RAID                                    |
| LiveCD OMSA via iDRAC   | ✅ Alternatif ekstrem jika OMSA+iSM gagal total                               |




