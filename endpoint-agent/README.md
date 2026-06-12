# endpoint-agent

**Status: belum diimplementasikan — direktori ini akan diisi pada waktu hacking babak final WRECK-IT 7.0.**

## Apa yang akan dibangun di sini

Agen ringan yang berjalan pada setiap host yang dilindungi — indera terdepan TALOS:

- **Pemantauan sistem berkas real-time** atas peristiwa create/write/rename/delete, memakai pustaka pemantauan lintas platform (watchdog) di atas mekanisme kernel (inotify pada Linux); eksplorasi eBPF untuk penelusuran syscall dengan overhead rendah pada tahap lanjut.
- **Manajemen canary files**: menanam dan mengawasi berkas umpan di lokasi strategis; setiap modifikasi terhadap canary adalah sinyal serangan berkeyakinan tinggi.
- **Pengiriman telemetri** (peristiwa berkas, entropi sampel tulisan, metadata proses) ke `detection-engine/`.
- **Penerima perintah** dari `containment-module/` untuk eksekusi isolasi pada host.

Kontrak antarmuka (format telemetri dan kanal perintah) ditetapkan pada Fase 0 sebelum implementasi paralel dimulai.
