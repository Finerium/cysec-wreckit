# detection-engine

**Status: belum diimplementasikan, direktori ini akan diisi pada waktu hacking babak final WRECK-IT 7.0.**

## Apa yang akan dibangun di sini

Mesin keputusan yang menyimpulkan apakah sedang terjadi enkripsi massal, dari kombinasi sinyal (bukan indikator tunggal, untuk menekan false positive):

- **Entropi Shannon** `H(X) = -Σ p_i log2 p_i` pada sampel tulisan berkas, lonjakan entropi menandakan keluaran terenkripsi.
- **Frekuensi operasi berkas**, laju write/rename abnormal per proses per detik.
- **Pemicuan canary** dari `endpoint-agent/`, sinyal berkeyakinan tinggi.
- **Opsi klasifikator ML ringan** (gradient boosting) untuk menggabungkan fitur dan menekan kesalahan deteksi, termasuk menghadapi enkripsi terputus (intermittent encryption).

Target latensi deteksi: **1–2 detik** sejak enkripsi massal dimulai. Keputusan terkonfirmasi diteruskan ke `containment-module/` dan konteks insiden ke `llm-orchestration/`.
