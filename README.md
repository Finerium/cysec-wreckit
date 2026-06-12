# TALOS — Autonomous Ransomware Defense

> **Tim Cyber Crown — Politeknik Negeri Bandung**
> Submission Hackathon **WRECK-IT 7.0** (Politeknik Siber dan Sandi Negara / BSSN)
> Subtema: *Autonomous Defense & AI-Driven Threat Hunting*

**TALOS** adalah rancangan sistem pertahanan ransomware otonom berbasis *closed-loop AI agent*: deteksi dini, isolasi otomatis (kill-switch), dan pemulihan cerdas dalam satu lingkar tertutup yang bekerja dalam hitungan detik — tanpa menunggu intervensi manusia pada fase kritis.

> ⚠️ **Status repositori: DOKUMENTASI PERENCANAAN — BELUM ADA KODE APLIKASI.**
> Sesuai mekanisme lomba, implementasi dilakukan pada waktu hacking yang ditetapkan panitia (babak final). Repositori ini sengaja hanya berisi dokumentasi arsitektur dan rencana implementasi, sehingga juri dapat memverifikasi bahwa tidak ada bagian aplikasi yang dibangun lebih awal.

---

## 1. Latar Belakang Masalah: Insiden PDNS 2024

Pada 20 Juni 2024, serangan ransomware **Brain Cipher (varian LockBit 3.0)** melumpuhkan Pusat Data Nasional Sementara (PDNS):

- Layanan keimigrasian bandara dan **lebih dari 280 layanan publik** lintas kementerian/lembaga terhenti.
- Penyerang lebih dulu **menonaktifkan antivirus** (MITRE ATT&CK T1562), **melumpuhkan mekanisme pemulihan** — Volume Shadow Copy, Hyper-V, backup Veeam (T1490) — lalu mengenkripsi data secara massal (T1486).
- Tuntutan tebusan USD 8 juta; hanya ±2% data memiliki cadangan.
- Indeks ketahanan siber nasional (NCSI) Indonesia turun dari peringkat 48 (2023) ke 84 (2025).

Akar persoalannya adalah **kecepatan**: LockBit mampu mengenkripsi ±25.000 berkas/menit (median tuntas ±6 menit — Splunk SURGe), sementara median *dwell time* respons konvensional ±5 hari (Mandiant M-Trends). Pertahanan yang bergantung pada manusia secara struktural kalah cepat. TALOS dirancang untuk memenangkan detik-detik pertama tersebut.

## 2. Arsitektur Closed-Loop

```
[Endpoint Agent] --telemetri--> [Detection Engine] --sinyal <10 dtk--> [Containment Module]
       ^                              ^      |                                |
       |                              |      +--konteks insiden--+            |
       +------perintah isolasi--------+                          v            v
                                      +------------- [LLM Orchestration] [Command Dashboard]
                                        pembaruan aturan   |   laporan, peta dampak,
                                                            +-> rencana pemulihan berprioritas
```

Lingkar tertutup: **Deteksi → Isolasi → Pemulihan**, dan keluaran AI mengalir balik memperbarui aturan deteksi.

## 3. Komponen (rencana implementasi per direktori)

| Direktori | Peran | Rencana teknologi |
|---|---|---|
| [`endpoint-agent/`](endpoint-agent/) | Sensor host: pemantauan sistem berkas real-time + canary files | Python + watchdog/inotify; eksplorasi eBPF untuk penelusuran syscall ber-overhead rendah |
| [`detection-engine/`](detection-engine/) | Keputusan deteksi enkripsi massal multi-sinyal | Entropi Shannon, frekuensi operasi tulis/rename, pemicuan canary; opsi klasifikator gradient boosting |
| [`containment-module/`](containment-module/) | Kill-switch otomatis < 10 detik | Isolasi jaringan host, terminasi proses, penguncian file share |
| [`llm-orchestration/`](llm-orchestration/) | Pemulihan cerdas berbasis AI agent | **DeepSeek V4 open-weight, self-hosted on-premise** + RAG atas playbook internal → laporan insiden, peta radius dampak, rencana restore berprioritas |
| [`dashboard/`](dashboard/) | Antarmuka komando real-time | Aplikasi web ringan; purwarupa UI high-fidelity sudah dirancang (tema gelap & terang) |

## 4. Mengapa On-Premise LLM (Kedaulatan Data)

Model open-weight (DeepSeek V4, jendela konteks 1M token) dijalankan **sepenuhnya di dalam infrastruktur organisasi** — log dan telemetri sensitif tidak pernah keluar ke layanan awan pihak ketiga. Untuk konteks Infrastruktur Informasi Vital nasional, properti ini adalah keharusan, bukan pilihan. Arsitektur dirancang *model-agnostic* sehingga model dapat dipertukarkan tanpa mengubah inti sistem.

## 5. Pendekatan Pengujian yang Aman & Etis

TALOS **tidak akan pernah diuji dengan ransomware sungguhan**. Validasi memakai **simulator enkripsi jinak** yang:

1. hanya beroperasi pada direktori sandbox khusus;
2. menyimpan kunci sehingga seluruh berkas dapat didekripsi kembali (reversible);
3. berjalan di lingkungan terisolasi tanpa konektivitas jaringan;
4. tidak memiliki kemampuan menyebar.

Kerangka standar seperti **Atomic Red Team** (simulasi teknik T1486) digunakan untuk pengujian adversarial yang terkontrol.

## 6. Roadmap (selaras Lampiran A proposal)

| Fase | Lingkup | Status |
|---|---|---|
| 0 | Fondasi repositori & kontrak antarmuka antarkomponen | 📄 didokumentasikan (repo ini) |
| 1 | Endpoint Agent & mesin canary | ⏳ menunggu waktu hacking |
| 2 | Detection Engine (entropi + frekuensi I/O + klasifikasi) | ⏳ menunggu waktu hacking |
| 3 | Containment Module | ⏳ menunggu waktu hacking |
| 4 | LLM Orchestration (DeepSeek V4 + RAG) → **Minimum Viable Loop** | ⏳ menunggu waktu hacking |
| 5 | Command Dashboard | ⏳ menunggu waktu hacking |
| 6 | Integrasi + simulator aman + pengujian | ⏳ menunggu waktu hacking |
| 7 | Hardening, evaluasi, persiapan demo | ⏳ menunggu waktu hacking |

Strategi **minimum viable loop**: satu lingkar fungsional utuh dibangun lebih dahulu, baru diperkaya — sehingga di setiap titik selalu ada purwarupa yang dapat didemonstrasikan.

## 7. Tim

| Nama | Peran |
|---|---|
| Ghaisan Khoirul Badruzaman | Ketua Tim |
| Hafiz Fauzan Syafrudin | Anggota |
| Moh. Fariq Alaudin | Anggota |
| Hasan Nasrullah | Anggota |

## Lisensi

Didistribusikan di bawah lisensi MIT — lihat [LICENSE](LICENSE).
