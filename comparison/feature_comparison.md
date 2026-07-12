# Perbandingan: Fitur di Project vs Fitur di Laporan

**Tanggal:** 10 Juli 2026  
**Scope:** Seluruh file di folder `FinalProject/` vs `Bab4_Pengujian_dan_Analisis.md` + `TemplateTaTeknikKomputer(8).pdf`

---

## 🟢 FITUR YANG ADA DI PROJECT & SUDAH DIJELASKAN DI LAPORAN

| Fitur | File Kode | Dibahas di Laporan |
|:---|:---|:---:|
| WoE Transformation + IV Feature Selection | `preprocessing/feature_selection.py` | ✅ §4.3.1 |
| SMOTE Class Imbalance Handling | `preprocessing/imbalance_handler.py` | ✅ §4.3.2 |
| XGBoost Training + RandomizedSearchCV | `ModelTraining/trainingCredit.py` | ✅ §4.3.3 |
| SHAP Explainability (Global) | `serving/app.py` (L95, L393) | ✅ §4.3.3 |
| SHAP Cross-Model Comparison | `run_ablation.py` | ✅ §4.3.4 |
| Ablation Study (4 model × 4 config) | `run_ablation.py` | ✅ §4.3.4 |
| TSTR vs TRTR Quality Gate | `TSTR/evaluate_tstr.py` | ✅ §4.3.5 |
| DCR Privacy Verification | `TSTR/utils_metrics.py` | ✅ §4.3.5 |
| Inverted KS Statistic | `TSTR/utils_metrics.py` | ✅ §4.3.5 |
| FastAPI `/predict` endpoint | `serving/app.py` (L273) | ✅ §4.3.6 |
| Grade A/B/C/D sistem | `serving/app.py` (L421–428) | ✅ §4.3.6 |
| Age Maturity Rule (80 tahun) | `serving/app.py` (L349–365) | ✅ §4.3.6 |
| Mitigasi Tenor Dinamis | `serving/app.py` (L356–360) | ✅ §4.3.6 |
| CSI Drift Detection | `preprocessing/drift_monitor.py` | ✅ §4.3.7 |
| n8n Webhook Retrain Trigger | `n8n_data/My workflow.json` | ✅ §4.3.7 |
| MLflow Experiment Tracking | `ModelTraining/trainingCredit.py` | ✅ §4.3.3 |
| Docker Containerization | `Dockerfile`, `docker-compose.yml` | ✅ Disebutkan |

---

## 🔴 FITUR YANG ADA DI PROJECT TAPI **TIDAK DIJELASKAN** DI LAPORAN

Ini yang paling penting — fitur-fitur ini sudah **benar-benar diimplementasikan** di kode tapi tidak ada pembahasannya di laporan.

---

### ❶ Sistem Database Nasabah (SQLite CRUD) — `serving/database.py`

**Apa yang ada di kode:**
File [`database.py`](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/serving/database.py) adalah modul database lengkap dengan:
- **SQLite persistent storage** untuk seluruh data 10.000 nasabah dari CSV
- **CRUD penuh**: `GET`, `POST`, `PUT`, `DELETE` via endpoint `/api/nasabah`
- **Pagination + Search + Filter** (by grade, by approval_status)
- **Auto-import dari CSV** saat startup (jika tabel belum ada)
- **Schema 40+ kolom** termasuk `approval_status`, `risk_grade`, `EAD`, `LGD`, `expected_loss`
- **Tabel `sent_emails`** untuk log email konfirmasi

**Di laporan:** ❌ Tidak disebutkan sama sekali. Laporan hanya membahas `/predict` endpoint. Seluruh subsistem database nasabah yang menjadi backbone UI tidak dibahas.

**Draft paragraph untuk laporan:**
> Sistem juga dilengkapi dengan modul database berbasis SQLite (`nasabah.db`) yang diinisialisasi secara otomatis dari dataset sintetis saat startup. Modul ini menyediakan endpoint CRUD lengkap — termasuk operasi pencarian, filter berdasarkan grade risiko, paginasi, serta manajemen status persetujuan — yang menjadi tulang punggung antarmuka pengguna web.

---

### ❷ Sistem Persetujuan Kredit via Email (Human-in-the-Loop) — `app.py` L443–562

**Apa yang ada di kode:**
- Endpoint `POST /api/nasabah/{id}/send-confirmation` yang mengirim **email HTML** ke reviewer dengan tombol Setuju/Tolak
- Email berisi ringkasan nasabah: bureau score, pendapatan, grade risiko, PD%
- Endpoint `GET /api/nasabah/{id}/approve` dan `/reject` yang **mengupdate `approval_status`** ke database
- Integrasi dengan **n8n webhook** untuk pengiriman email (`/webhook/send-confirmation`)
- Sistem ini adalah implementasi **Human-in-the-Loop (HITL)** nyata — analis bisa setuju/tolak via klik di email

**Di laporan:** ❌ Tidak disebutkan sama sekali. Ironisnya, laporan di §4.11.1 justru mengkritik "ketiadaan mekanisme Human-in-the-Loop" — padahal sistem ini **sudah diimplementasikan** di kode! Ini adalah inkonsistensi besar antara kode dan laporan.

**Draft paragraph untuk laporan:**
> Sebagai mekanisme Human-in-the-Loop, sistem dilengkapi dengan fitur pengiriman konfirmasi email kepada reviewer. Setelah model menghasilkan prediksi, analis kredit dapat memicu pengiriman email HTML yang berisi ringkasan risiko nasabah beserta dua tombol interaktif — Setuju dan Tolak. Klik pada tombol tersebut secara otomatis memperbarui status persetujuan (`approval_status`) nasabah di database melalui endpoint `/api/nasabah/{id}/approve` atau `/reject`. Mekanisme ini memungkinkan keputusan kredit final tetap berada di tangan manusia, bukan diserahkan sepenuhnya kepada model machine learning.

---

### ❸ Workflow n8n: Ingestion Formulir Kredit via Gmail PDF — `My workflow.json`

**Apa yang ada di kode:**
Workflow n8n sebenarnya memiliki **dua pipeline terpisah**, bukan satu:

**Pipeline 1 (Retraining) — yang ada di laporan:**
- `Schedule Trigger (7 hari)` → `Execute Training` → `AUC > 0.75?` → `Promote to MLflow Production` → `Reload Model`

**Pipeline 2 (Ingestion Formulir) — yang TIDAK ada di laporan:**
- `Gmail Trigger` (polling setiap jam, filter `subject:"Credit Application" has:attachment filename:pdf`)
- `Get Message` (download attachment)
- `Write PDF File` (simpan ke `/tmp/application.pdf`)
- `Run PDF Parser` (`parse_pdf.py`)
- `Parse Parser Output` (validasi JSON)
- `Is Parser Successful?`
- `Save to Database` (POST ke `/api/nasabah`)

Artinya ada **otomasi end-to-end**: nasabah kirim email dengan PDF formulir kredit → n8n otomatis parse → simpan ke database → siap untuk diprediksi. File `parse_pdf.py` dan `credit_application_form.pdf` juga ada di folder.

**Di laporan:** ❌ Hanya pipeline retraining yang disebutkan. Pipeline ingestion formulir PDF via Gmail sama sekali tidak ada.

**Draft paragraph untuk laporan:**
> Selain pipeline retraining, workflow n8n juga mengintegrasikan jalur ingesti data nasabah berbasis email. Setiap jam, n8n memonitor inbox Gmail untuk email dengan subjek "Credit Application" yang menyertakan lampiran PDF. Formulir yang diterima secara otomatis diunduh, diteruskan ke skrip `parse_pdf.py` untuk ekstraksi data, kemudian disimpan ke database nasabah melalui endpoint API. Dengan demikian, proses onboarding nasabah baru dapat dilakukan sepenuhnya tanpa intervensi manual.

---

### ❹ Model Registry & Promotions System — `app.py` L644–713

**Apa yang ada di kode:**
- `GET /api/model/versions` — menampilkan semua versi model dari MLflow Registry (versi, stage, AUC, F1, params, waktu pembuatan)
- `POST /api/model/promote` — mem-promosikan versi model tertentu ke stage `Production` dan otomatis memuat ulang model ke memori
- `POST /api/model/reload` — memuat ulang artefak model terbaru dari MLflow tanpa restart server
- `POST /api/model/retrain` — trigger manual retraining via n8n webhook

**Di laporan:** ⚠️ Hanya disebutkan bahwa "MLflow digunakan untuk mencatat eksperimen." Fitur Model Registry, version management, dan hot-reload tanpa downtime tidak dibahas.

**Draft paragraph untuk laporan:**
> Sistem juga menyediakan manajemen versi model secara operasional melalui endpoint Model Registry. Analis dapat melihat semua versi model yang terdaftar beserta metriknya, mem-promosikan versi tertentu ke tahap Production, dan memuat ulang model ke memori serving secara langsung tanpa perlu me-restart layanan API. Kemampuan hot-reload ini memastikan layanan inferensi tetap berjalan saat model baru dideploy.

---

### ❺ PSI Monitoring (selain CSI) — `preprocessing/drift_monitor.py`

**Apa yang ada di kode:**
File [`drift_monitor.py`](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/preprocessing/drift_monitor.py) mengimplementasikan **dua jenis drift metric**:
- **PSI (Population Stability Index)** — untuk monitoring fitur numerik/distribusi populasi
- **CSI (Characteristic Stability Index)** — untuk monitoring per-fitur

**Di laporan:** ⚠️ Hanya CSI yang diuji dan dibahas di §4.3.7. PSI disebutkan di `Documentation.md` tapi tidak ada di Bab 4. Laporan juga tidak menjelaskan **perbedaan PSI vs CSI** dan kapan masing-masing digunakan.

---

### ❻ Inference Logging ke JSONL — `app.py` L376–389

**Apa yang ada di kode:**
Setiap request prediksi ke `/predict` secara otomatis di-log ke file `n8n_data/inference_logs.jsonl`. Log berisi seluruh fitur input + probabilitas prediksi + timestamp. File ini berfungsi sebagai **data produksi** yang dibaca oleh `drift_monitor.py` untuk mendeteksi drift secara real.

**Di laporan:** ❌ Sama sekali tidak disebutkan. Padahal ini adalah komponen krusial yang membuat "closed-loop automation" benar-benar berjalan — tanpa log ini, drift detection tidak punya data produksi untuk dibandingkan.

---

### ❼ Tiga Zona Keputusan Usia (Joint-Borrower) — `app.py` L356–365

**Apa yang ada di kode:**
```python
if max_term_allowed > 0:
    m_list.append(f"Rekomendasi pengurangan tenor maksimal menjadi {max_term_allowed} bulan...")
else:
    m_list.append("Nasabah telah berusia 80 tahun ke atas. Rekomendasi: joint-income (co-borrower)...")
```

Ada **tiga zona keputusan** nyata di kode (sesuai template PDF):
1. Usia pelunasan ≤ 80 → normal
2. Usia < 80 tapi pelunasan > 80 → mitigasi tenor dinamis  
3. Usia ≥ 80 → rekomendasi co-borrower

**Di laporan:** ⚠️ Zona co-borrower (zona 3) tidak disebutkan. Hanya 2 dari 3 zona yang dibahas.

---

### ❽ MLflow Tracing pada Inferensi — `app.py` L274, L287

**Apa yang ada di kode:**
```python
@app.post("/predict")
@mlflow.trace(name="credit_risk_predict")
def predict(request: PredictRequest):
    mlflow.update_current_trace(tags={
        "model_version": ...,
        "model_run_id": ...,
    })
```

Setiap panggilan prediksi di-**trace oleh MLflow** dengan tag versi model dan run ID. Ini adalah fitur MLflow Tracing (bukan sekadar experiment logging) yang memungkinkan audit trail per-inferensi.

**Di laporan:** ❌ Tidak disebutkan. Hanya experiment logging yang dibahas, bukan inference tracing.

---

### ❾ Dataset Drifted untuk Pengujian — `ModelTraining/synthetic_credit_risk_data_drifted.csv`

**Apa yang ada di kode:**
Ada tiga versi dataset di folder:
- `synthetic_credit_risk_data.csv` — data utama (10.000 baris)
- `synthetic_credit_risk_data_nondrift.csv` — data produksi stabil
- `synthetic_credit_risk_data_drifted.csv` — data produksi dengan drift simulasi (pendapatan -20%, pinjaman +35%)

**Di laporan:** ⚠️ Laporan menyebutkan skenario drift tapi tidak menjelaskan bahwa ada file CSV terpisah yang digunakan untuk mensimulasikan kondisi drifted. Ini penting untuk reprodusibilitas pengujian.

---

## 📊 Ringkasan Perbandingan

```
TOTAL FITUR DI PROJECT:  ~25 fitur/sistem
DIJELASKAN DI LAPORAN:   ~15 fitur
TIDAK DIJELASKAN:        ~9 fitur (36%)
```

### Urutan Prioritas untuk Ditambahkan ke Laporan

| Prioritas | Fitur | Alasan |
|:---:|:---|:---|
| 🔴 **P1** | Email Approval + Human-in-the-Loop | Laporan §4.11.1 justru mengkritik "tidak ada HITL" padahal ada! Inkonsistensi fatal |
| 🔴 **P1** | Database Nasabah SQLite (CRUD) | Backbone seluruh UI tapi tidak disebutkan sama sekali |
| 🔴 **P1** | n8n Pipeline Ingestion Gmail PDF | Pipeline ke-2 yang sama sekali tidak ada di laporan |
| 🟡 **P2** | Model Registry & Hot-Reload | Fitur operasional penting untuk MLOps maturity |
| 🟡 **P2** | Inference Logging JSONL | Komponen kritis untuk closed-loop drift detection |
| 🟡 **P2** | Zona Joint-Borrower (Zona ke-3) | Ada di kode, ada di template PDF, tidak ada di draft laporan |
| 🟢 **P3** | PSI vs CSI distinction | Perlu klarifikasi dua metric berbeda |
| 🟢 **P3** | MLflow Inference Tracing | Audit trail per-prediksi |
| 🟢 **P3** | Dataset Drifted (file terpisah) | Transparansi metodologi pengujian |

---

*Laporan ini dibuat berdasarkan scan kode sumber di `serving/app.py` (735 baris), `serving/database.py` (328 baris), `n8n_data/My workflow.json`, `preprocessing/drift_monitor.py`, dan seluruh struktur direktori proyek.*
