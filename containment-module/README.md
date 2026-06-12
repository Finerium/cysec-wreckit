# containment-module

**Status: belum diimplementasikan — direktori ini akan diisi pada waktu hacking babak final WRECK-IT 7.0.**

## Apa yang akan dibangun di sini

Eksekutor **kill-switch** otomatis TALOS — target < 10 detik sejak deteksi, tanpa menunggu persetujuan manual:

- **Isolasi jaringan host** terinfeksi (pemutusan antarmuka / aturan firewall) untuk menghentikan penyebaran lateral.
- **Terminasi proses** berbahaya yang teridentifikasi mesin deteksi.
- **Penonaktifan berbagi berkas (file share)** untuk menghentikan enkripsi atas data bersama.
- **Umpan balik status** ke `dashboard/` dan perintah isolasi ke `endpoint-agent/`.

Seluruh aksi dicatat (audit log) agar dapat diverifikasi operator melalui Command Dashboard.
