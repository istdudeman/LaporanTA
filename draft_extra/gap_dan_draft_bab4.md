# Gap Analisis & Draft Tambahan — Bab 4 (Pengujian dan Analisis)

**Draft Bab 4 saat ini:** `Bab4_Pengujian_dan_Analisis.md` (364 baris)  
**Status:** Konten inti sudah bagus, tapi ada **7 tambahan** yang perlu disisipkan.

---

## Peta Penyisipan (Urutan dari Atas ke Bawah File)

| # | Sisipan | Setelah Baris | Prioritas |
|:---:|:---|:---:|:---:|
| 1 | Pendahuluan Bab (§4.0) | Baris 3 | 🔴 Tinggi |
| 2 | Tabel Skenario Formal | Baris 37 | 🟡 Sedang |
| 3 | Validasi Awal Dataset Sintetis (§4.2.1) | Baris 39 | 🔴 Tinggi |
| 4 | Zona Joint-Borrower + perluasan mitigasi | Baris 333 | 🔴 Tinggi |
| 5 | Kasus Uji 3: Email HITL | Baris 341 | 🔴 Tinggi |
| 6 | Inference Logging + n8n Gmail Pipeline | Baris 363 | 🟡 Sedang |
| 7 | Evaluasi Kritis §4.11 + Ringkasan §4.12 | Baris 364 (akhir) | 🔴 Tinggi |

---

## SISIPAN 1 — Pendahuluan Bab
**Sisipkan setelah baris 3 (setelah `---`), sebelum `## 4.1 Lingkungan Pengujian`**

```markdown
## 4.0 Pendahuluan Pengujian

Bab ini membahas pengujian dan analisis terhadap sistem *Credit Risk Scoring* berbasis
Machine Learning Operations (MLOps) yang telah dirancang pada Bab III. Pengujian tidak
hanya diarahkan untuk melihat performa model prediktif, tetapi juga untuk memverifikasi
kesiapan sistem sebagai *pipeline* operasional yang dapat melakukan pengolahan data,
pelatihan model, interpretasi hasil, validasi kualitas data, penyajian model melalui API,
pemantauan *drift*, serta notifikasi keputusan berbasis email.

Ruang lingkup pengujian disusun agar mampu menjawab dua kebutuhan utama. Pertama,
kebutuhan akademik — membuktikan bahwa metode yang digunakan memiliki dasar kuantitatif
dan dapat dievaluasi secara ilmiah. Kedua, kebutuhan implementatif — membuktikan bahwa
sistem dapat berjalan sebagai rangkaian proses MLOps yang mendukung reprodusibilitas,
pelacakan eksperimen, dan pengambilan keputusan berbasis model. Dengan demikian, Bab IV
berfungsi sebagai penghubung antara rancangan sistem dan bukti implementasi nyata.

---
```

---

## SISIPAN 2 — Tabel Skenario Formal
**Sisipkan setelah baris 37 (setelah item ke-7 list), sebelum `---`**

```markdown

Tabel berikut merangkum seluruh skenario pengujian beserta tujuan verifikasinya:

| No. | Skenario | Tujuan Pengujian |
| :---: | :--- | :--- |
| 1 | Preprocessing WoE dan IV | Menguji efektivitas transformasi dan seleksi fitur berdasarkan kekuatan prediktif. |
| 2 | Penanganan *imbalance* dengan SMOTE | Memastikan data latih lebih seimbang sehingga model tidak bias terhadap kelas mayoritas. |
| 3 | Pelatihan dan tuning XGBoost | Menentukan konfigurasi model terbaik berdasarkan performa validasi dan data uji. |
| 4 | *Quality gate* data sintetis (TSTR/TRTR) | Menguji utilitas data sintetis dan risiko kebocoran privasi via DCR dan KS. |
| 5 | API *serving* dan aturan bisnis | Memverifikasi layanan prediksi FastAPI, aturan usia, dan sistem notifikasi email HITL. |
| 6 | Deteksi *drift* dan *retraining* otomatis | Menguji deteksi perubahan distribusi dan pemicuan webhook n8n untuk *retraining*. |
| 7 | Studi Ablasi Perbandingan Model | Mengukur kontribusi WoE dan SMOTE pada 4 arsitektur model. |

Pembagian skenario ini memperlihatkan bahwa pengujian tidak berhenti pada akurasi model —
sistem juga diverifikasi dari sisi kualitas data, keamanan privasi, ketahanan aturan
bisnis, kesiapan monitoring, dan alur keputusan berbasis notifikasi.
```

---

## SISIPAN 3 — Validasi Awal Dataset Sintetis
**Sisipkan setelah baris 39 (setelah `---`), sebelum `## 4.3 Hasil Pengujian`**

```markdown
## 4.2.1 Validasi Awal Dataset Sintetis

Sebelum pengujian komponen model dilakukan, dataset sintetis yang dibentuk pada Bab III
terlebih dahulu divalidasi secara deskriptif. Validasi ini bertujuan memastikan bahwa
struktur atribut, format nilai, dan relasi antarkolom tetap konsisten dengan karakteristik
umum data kredit konsumtif pada praktik perbankan.

Sebanyak 50 baris sampel data sintetis ditinjau. Tabel 4.X menampilkan 10 baris dari
sampel tersebut untuk membuktikan bahwa dataset tidak hanya dibangkitkan secara numerik,
tetapi juga memiliki variasi atribut identitas, pekerjaan, finansial, histori kredit,
parameter risiko, dan variabel makroekonomi yang saling berhubungan secara logis.

[**Catatan: Sisipkan Tabel 4.X Profil Data 10 Nasabah Normal di sini.**
File sudah tersedia di `tabel_10_nasabah_normal.md` — tinggal di-embed atau dikonversi ke LaTeX.]

Berdasarkan sampel tersebut, dataset sintetis dinilai memadai karena telah memuat variasi
profil nasabah (pensiunan, ASN, BUMN), produk kredit (KTA, Kredit Pensiun, Vehicle Loan),
indikator risiko (PD, LGD, EAD, EL), dan parameter asuransi yang relevan untuk *credit
risk scoring*. Dengan demikian, pengujian pada bagian berikutnya dapat difokuskan pada
kualitas *preprocessing*, performa model, dan kesiapan operasional *pipeline* MLOps.

---
```

---

## SISIPAN 4 — Zona Joint-Borrower (Mitigasi ke-3)
**Sisipkan setelah baris 333, di dalam `#### B. Kasus Uji 2`, setelah item mitigasi ke-2**

```markdown
    3. **Skema *Joint-Borrower* / *Co-Borrower*:** Apabila usia nasabah saat pengajuan
       telah mencapai 80 tahun atau lebih, pengurangan tenor tidak lagi memadai karena
       sisa batas usia bernilai nol atau negatif. Pada kondisi ini, sistem merekomendasikan
       alternatif pengajuan berbasis *joint-income* bersama anggota keluarga inti yang
       berusia di bawah 60 tahun. Dengan skema *co-borrower*, kapasitas bayar dinilai
       ulang secara agregat sehingga nasabah lanjut usia tidak langsung ditolak secara kaku.
```

**Kemudian tambahkan sub-bab baru setelah Kasus Uji 2 (setelah baris 341):**

```markdown

### 4.3.6.1 Analisis Tiga Zona Keputusan Hibrida

Sebelum aturan batas usia pelunasan diterapkan, model XGBoost murni dapat meluluskan
nasabah berusia lanjut — misalnya 75–80+ tahun — dengan *risk grade* baik karena profil
keuangannya terlihat kuat (pendapatan pensiun stabil, *bureau score* tinggi, DTI rendah).
Untuk mengatasi hal ini, sistem menerapkan logika hibrida pada lapisan API yang membagi
kondisi nasabah ke dalam **tiga zona keputusan**:

| Zona | Kondisi | Respons Sistem |
| :---: | :--- | :--- |
| **Zona Lolos** | Usia pelunasan ≤ 80 tahun | Diteruskan ke model XGBoost, grade A–D normal |
| **Zona Mitigasi Dinamis** | Usia < 80, tapi pelunasan > 80 tahun | `Grade Undefined` + rekomendasi tenor maks: $(80 - \text{usia}) \times 12$ bulan |
| **Zona Joint-Borrower** | Usia saat pengajuan ≥ 80 tahun | `Grade Undefined` + rekomendasi skema *co-borrower* keluarga inti |

Sistem ini membuktikan bahwa logika aturan bisnis berfungsi sebagai lapisan pengaman
yang membuat hasil inferensi model lebih realistis dan dapat diterapkan pada proses
kredit operasional — tanpa langsung menolak nasabah secara kaku.
```

---

## SISIPAN 5 — Kasus Uji 3: Email HITL
**Sisipkan setelah baris 341 (setelah `---` penutup §4.3.6), sebelum §4.3.7**

```markdown

#### C. Kasus Uji 3: Sistem Konfirmasi Email dan Keputusan Manual (*Human-in-the-Loop*)

Selain pengujian prediksi berbasis model, sistem juga dilengkapi dengan mekanisme
*Human-in-the-Loop* (HITL) yang memungkinkan keputusan kredit final tetap berada pada
pihak manusia. Mekanisme ini diimplementasikan melalui fitur pengiriman email konfirmasi
kepada analis kredit yang diintegrasikan dengan n8n via *workflow* pengiriman SMTP.

**Alur kerja sistem konfirmasi email:**

1. Setelah model menghasilkan prediksi, analis kredit memicu endpoint
   `POST /api/nasabah/{customer_id}/send-confirmation` dengan menyertakan alamat email reviewer.
2. Sistem FastAPI menyusun **email HTML responsif** berisi ringkasan risiko nasabah:
   nama, usia, *bureau score*, pendapatan, jumlah pinjaman, *risk grade*, dan probabilitas *default*.
3. Email dikirimkan ke reviewer melalui n8n webhook (`/webhook/send-confirmation`) via SMTP Gmail.
4. Setiap pengiriman dicatat ke tabel `sent_emails` di database sebagai *audit trail*.
5. Email berisi **dua tombol interaktif** — *Setuju* dan *Tolak*. Klik pada tombol memanggil
   `GET /api/nasabah/{id}/approve` atau `/reject` yang memperbarui `approval_status` di database.
6. Apabila email dikirim ulang, sistem otomatis mereset `approval_status` ke `"Pending"`.

**Hasil pengujian:**

| Aspek yang Diuji | Hasil |
| :--- | :--- |
| Email HTML terkirim ke reviewer via n8n SMTP | ✅ Berhasil |
| Tombol *Approve* memperbarui `approval_status` | ✅ `→ "Accepted"` |
| Tombol *Reject* memperbarui `approval_status` | ✅ `→ "Rejected"` |
| Pengiriman ulang mereset status ke Pending | ✅ Berhasil |
| Log konfirmasi tercatat di tabel `sent_emails` | ✅ Berhasil |

Mekanisme ini membuktikan bahwa sistem tidak sepenuhnya mengandalkan model *machine
learning* untuk keputusan final. Pada kasus dengan zona abu-abu — misalnya nasabah dengan
*grade* B/C yang memerlukan pertimbangan lebih lanjut — analis dapat menerima ringkasan
risiko via email dan memberikan keputusan manusiawi secara langsung.
```

---

## SISIPAN 6 — Inference Logging + Pipeline Gmail n8n
**Sisipkan setelah baris 363 (di akhir §4.3.7, setelah paragraf terakhir)**

```markdown

#### Mekanisme Pencatatan Inferensi (*Inference Logging*) untuk Deteksi Drift

Agar deteksi *drift* dapat beroperasi secara *closed-loop* pada lingkungan produksi nyata,
setiap permintaan prediksi ke endpoint `/predict` secara otomatis dicatat ke file
`n8n_data/inference_logs.jsonl`. File log ini berisi seluruh fitur input nasabah,
probabilitas *default* yang dihasilkan model, serta *timestamp* prediksi. `DriftMonitor`
kemudian membandingkan distribusi data di file log ini dengan distribusi referensi data
pelatihan untuk menghitung nilai PSI dan CSI. Tanpa mekanisme pencatatan inferensi ini,
deteksi *drift* tidak akan memiliki data produksi yang representatif untuk dianalisis.

#### Alur Orkestrasi n8n: Ingesti Formulir Kredit via Gmail PDF

Selain *pipeline* retraining, sistem juga mengimplementasikan *pipeline* kedua dalam
workflow n8n yang mengintegrasikan jalur ingesti data nasabah berbasis email secara
otomatis. Workflow ini berjalan dengan penjadwalan polling Gmail setiap satu jam:

| Langkah | Node n8n | Aksi |
| :---: | :--- | :--- |
| 1 | **Gmail Trigger** | Memonitor inbox untuk email bersubjek *"Credit Application"* + lampiran PDF |
| 2 | **Download Attachment** | Lampiran PDF diunduh dan disimpan ke direktori sementara |
| 3 | **PDF Parser** | `parse_pdf.py` mengekstraksi data nasabah ke format JSON |
| 4 | **Validasi Parser** | n8n memvalidasi apakah ekstraksi berhasil |
| 5 | **Simpan ke Database** | Data dikirimkan via `POST /api/nasabah` ke database SQLite |

Dengan alur ini, proses *onboarding* nasabah baru dapat dilakukan tanpa intervensi
manual — formulir kredit yang dikirim lewat email langsung masuk ke database dan siap
untuk diprediksi oleh model. Integrasi dua *pipeline* n8n (retraining + ingesti PDF)
memperlihatkan bahwa sistem MLOps yang dibangun memiliki tingkat otomasi yang komprehensif.
```

---

## SISIPAN 7 — Evaluasi Kritis + Ringkasan Bab
**Sisipkan setelah baris 364 (di akhir file)**

```markdown

---

## 4.11 Evaluasi Kritis dan Batasan Operasional Sistem

Meskipun hasil pengujian menunjukkan bahwa *pipeline* MLOps telah mampu menjalankan
otomasi pelatihan, *deployment*, monitoring *drift*, dan integrasi aturan bisnis, sistem
ini masih memiliki beberapa batasan jika dilihat dari sudut pandang manajemen risiko riil.
Evaluasi kritis ini penting karena sistem penilaian kredit tidak hanya harus akurat secara
teknis, tetapi juga harus aman secara operasional dan dapat dipertanggungjawabkan.

### 4.11.1 Kerentanan Subjektivitas pada Proses WoE *Binning*

Transformasi WoE terbukti membantu stabilitas fitur dan mendukung interpretabilitas model.
Namun, proses pembentukan interval (*binning*) pada variabel numerik tetap memiliki unsur
sensitivitas metodologis. Perbedaan metode *binning* — *equal width*, *equal frequency*,
atau berbasis aturan bisnis — dapat menghasilkan batas interval yang berbeda dan mengubah
nilai WoE secara signifikan. Sebagai contoh, perubahan kecil pada batas kelompok
`bureau_score` dapat mengubah interpretasi risiko dan memengaruhi *grade* akhir nasabah.
Oleh karena itu, proses *binning* perlu divalidasi tidak hanya secara statistik, tetapi
juga berdasarkan kewajaran bisnis dan kebijakan risiko lembaga keuangan.

### 4.11.2 Beban Finansial Spesifik yang Tidak Sepenuhnya Tercermin pada *Risk Grade*

Berdasarkan penelaahan deskriptif terhadap sampel data nasabah (Tabel 4.X), terdapat
celah antara profil finansial yang terlihat di fitur model dan kondisi keuangan nyata
nasabah. Sebagai contoh, nasabah dengan pendapatan sekitar Rp3.285.278 dan rasio
pengeluaran medis 42,09% memiliki beban medis rutin sekitar **Rp1,38 juta per bulan**.
Beban ini berpotensi menjadi *fixed commitment* yang mengurangi kapasitas bayar
(*repayment capacity*), namun mungkin tidak cukup dominan untuk mengubah *grade* jika
fitur seperti `bureau_score` tinggi. Kondisi ini menunjukkan bahwa keluaran model
sebaiknya diposisikan sebagai **alat bantu keputusan**, bukan pengganti penuh proses
validasi kredit.

### 4.11.3 Dependensi Infrastruktur pada Lingkungan Lokal

Seluruh sistem diuji pada lingkungan lokal menggunakan Docker Compose. Pada skenario
produksi skala penuh, ketergantungan pada konektivitas internal antarcontainer (FastAPI
↔ MLflow ↔ n8n) dan konfigurasi jaringan Docker perlu digantikan dengan arsitektur
berbasis *service mesh* atau cloud-native yang lebih robust. Selain itu, file
`inference_logs.jsonl` yang saat ini disimpan secara lokal perlu dipindahkan ke solusi
penyimpanan terdistribusi agar tidak menjadi *single point of failure* pada proses
monitoring *drift*.

---

## 4.12 Ringkasan Bab

Bab ini telah menguraikan hasil pengujian sistem secara menyeluruh, mulai dari
*preprocessing* WoE dan IV, penanganan *imbalance*, pelatihan dan tuning model, evaluasi
performa, interpretabilitas SHAP, validasi *quality gate* data sintetis, *API serving*
beserta aturan bisnis dan konfirmasi email HITL, hingga deteksi *drift* dan *retraining*
otomatis.

Hasil pengujian menunjukkan bahwa model XGBoost memperoleh AUC **0,8262** dengan
keseimbangan presisi dan *recall* yang baik. Transformasi WoE dan seleksi IV memperkuat
dasar pemilihan fitur, SHAP memberikan transparansi prediksi per-nasabah, dan data
sintetis berhasil melewati *quality gate* utilitas (TSTR drop hanya **1,41%**) dan privasi
(DCR mean 1,9421, tidak ada rekaman yang tersalin persis). Dari sisi operasional, layanan
FastAPI mampu mengembalikan prediksi beserta penjelasan, aturan bisnis tiga zona mencegah
keputusan yang melanggar kebijakan, mekanisme konfirmasi email memungkinkan *human
oversight* pada kasus ambang, dan sistem monitoring *drift* memicu *retraining* otomatis
melalui n8n ketika distribusi data produksi bergeser secara signifikan (CSI > 0,25).

Dengan demikian, sistem yang dikembangkan tidak hanya memenuhi kebutuhan pemodelan
prediktif, tetapi juga menunjukkan kesiapan sebagai *pipeline* MLOps yang operasional,
dapat diaudit, dan mendukung keputusan kredit yang bertanggung jawab.
```

---

## Ringkasan Operasional

| Sisipan | Di mana | Setelah Baris | Ubah Teks Lama? |
|:---:|:---|:---:|:---:|
| 1 — Pendahuluan Bab §4.0 | Sebelum §4.1 | 3 | ❌ Tidak |
| 2 — Tabel Skenario Formal | Setelah list 7 item | 37 | ❌ Tidak |
| 3 — Validasi Dataset §4.2.1 | Sebelum §4.3 | 39 | ❌ Tidak |
| 4 — Zona Joint-Borrower + Tabel 3 Zona | Di dalam §4.3.6.B | 333 | ❌ Tidak (tambah saja) |
| 5 — Kasus Uji 3 Email HITL | Setelah §4.3.6 | 341 | ❌ Tidak |
| 6 — Inference Logging + Gmail Pipeline | Di akhir §4.3.7 | 363 | ❌ Tidak |
| 7 — §4.11 Evaluasi Kritis + §4.12 Ringkasan | Akhir file | 364 | ❌ Tidak |

> **Semua sisipan bersifat TAMBAH SAJA — tidak ada satu baris pun dari draft yang perlu dihapus atau diubah.**
