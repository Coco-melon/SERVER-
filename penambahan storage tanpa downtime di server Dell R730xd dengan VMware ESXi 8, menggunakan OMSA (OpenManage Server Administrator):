penambahan storage tanpa downtime di server Dell R730xd dengan VMware ESXi 8, menggunakan OMSA (OpenManage Server Administrator)

🔧 Tujuan

Menambah kapasitas storage (RAID & VMFS datastore) tanpa mematikan VM.

🧰 Langkah Ringkas

1.  Install OMSA di ESXi 8

    Download OMSA VIB dari Dell.

    Upload ke datastore ESXi.

    Jalankan via SSH:
      esxcli software vib install -d /path/to/OMSA.zip

2.  Install OMSA di Windows (Remote GUI)

    Install OMSA Managed Node di PC.

    Akses ESXi host dari OMSA GUI (pakai IP dan user root ESXi).

3.  Tambahkan Hard Disk Fisik ke Server

    Pasang HDD/SSD baru ke slot kosong.

4.  Expand Virtual Disk (RAID) via OMSA

    Di OMSA → masuk ke Storage → pilih Virtual Disk.

    Gunakan fitur Reconfigure / Expand untuk menambahkan disk ke array RAID.

    Tunggu proses selesai (bisa lama, tergantung kapasitas).

5.  Perbesar Datastore di vSphere

    Masuk vSphere → Storage → Rescan Storage Adapter.

    Klik kanan datastore → Increase Capacity.

    Pilih disk yang sudah diperbesar → ikuti wizard.
