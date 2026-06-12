# llm-orchestration

**Status: belum diimplementasikan, direktori ini akan diisi pada waktu hacking babak final WRECK-IT 7.0.**

## Apa yang akan dibangun di sini

Lapisan kecerdasan pemulihan TALOS, pembeda utama sistem:

- **Model**: DeepSeek V4 (open-weight, mixture-of-experts, konteks 1M token) dijalankan **self-hosted on-premise**, telemetri sensitif tidak pernah meninggalkan infrastruktur organisasi (kedaulatan data). Arsitektur model-agnostic.
- **RAG** atas playbook respons insiden internal untuk membatasi halusinasi.
- **Tiga keluaran otomatis** dari konteks insiden:
  1. **Laporan insiden terstruktur** (kronologi, teknik MITRE ATT&CK, aksi yang diambil);
  2. **Peta radius dampak** (host/segmen terdampak dan berisiko);
  3. **Rencana pemulihan berprioritas** (urutan restore berdasarkan kekritisan dan dependensi layanan).
- Keluaran bersifat **advisory** dengan human-in-the-loop untuk aksi destruktif; lingkar umpan balik memperbarui aturan deteksi.
