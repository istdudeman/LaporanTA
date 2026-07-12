# Laporan Gap Analisis: Bab IV Draft vs Template TA

**Dokumen Referensi:** `TemplateTaTeknikKomputer(8).pdf` (76 halaman)  
**Dokumen Pembanding:** `Bab4_Pengujian_dan_Analisis.md` (laporan Bab 4 draft)  
**Tanggal Analisis:** 10 Juli 2026

---

## Ringkasan Eksekutif

Setelah membandingkan dokumen PDF resmi dengan laporan Bab 4 yang ada, ditemukan **6 bagian/fitur mayor** yang ada di dalam template PDF namun **tidak atau belum dijelaskan** di laporan draft. Perbedaan ini bukan sekadar persoalan format — beberapa bagian menyangkut konten substantif yang dinilai oleh penguji, termasuk justifikasi metodologis, evaluasi kritis sistem, dan dokumentasi arsitektur teknis.

---

## Gap 1 — Bagian `4.1 Pendahuluan Pengujian` (Hilang Sepenuhnya)

### Kondisi di PDF
Template PDF memiliki sub-bagian eksplisit **§4.1 Pendahuluan Pengujian** (halaman 49) yang berfungsi sebagai pengantar keseluruhan bab sebelum masuk ke spesifikasi lingkungan. Paragraf ini menjelaskan:
- Tujuan ganda pengujian: kebutuhan **akademik** (bukti kuantitatif) dan kebutuhan **implementatif** (bukti sistem dapat berjalan sebagai pipeline MLOps).
- Ruang lingkup pengujian: enam skenario utama disebutkan secara ringkas.
- Posisi Bab 4 sebagai "penghubung antara rancangan sistem dan bukti implementasi."

### Kondisi di Laporan Draft
Laporan draft langsung membuka dengan `## 4.1 Lingkungan Pengujian` tanpa pendahuluan bab. Tidak ada paragraf yang menjelaskan *mengapa* pengujian dirancang seperti ini dan apa yang dibuktikan secara akademis.

### Dampak
Penguji akan kesulitan memahami *kerangka evaluasi* yang digunakan sebelum membaca angka-angka. Ini melanggar konvensi penulisan akademis standar ITS.

### Draft Teks yang Direkomendasikan (Sisipkan sebelum `## 4.1 Lingkungan Pengujian`)

```markdown
## 4.1 Pendahuluan Pengujian

Bab ini membahas pengujian dan analisis terhadap sistem *Credit Risk Scoring* berbasis 
Machine Learning Operations (MLOps) yang telah dirancang pada Bab III. Pengujian tidak 
hanya diarahkan untuk melihat performa model prediktif, tetapi juga untuk memverifikasi 
kesiapan sistem sebagai pipeline operasional yang dapat melakukan pengolahan data, pelatihan 
model, interpretasi hasil, validasi kualitas data, penyajian model melalui API, serta 
pemantauan *drift*.

Ruang lingkup pengujian disusun agar mampu menjawab dua kebutuhan utama. Pertama, 
kebutuhan akademik, yaitu membuktikan bahwa metode yang digunakan memiliki dasar 
kuantitatif dan dapat dievaluasi secara ilmiah. Kedua, kebutuhan implementatif, yaitu 
membuktikan bahwa sistem dapat berjalan sebagai rangkaian proses MLOps yang mendukung 
reprodusibilitas, pelacakan eksperimen, dan pengambilan keputusan berbasis model. Dengan 
demikian, Bab IV berfungsi sebagai penghubung antara rancangan sistem dan bukti 
implementasi.

Secara umum, pengujian dilakukan melalui enam skenario utama, yaitu pengujian 
*preprocessing* menggunakan WoE dan IV, penanganan ketidakseimbangan kelas menggunakan 
SMOTE, pelatihan dan *hyperparameter tuning* model XGBoost, pengujian kualitas data 
sintetis menggunakan pendekatan TSTR dan TRTR, pengujian layanan inferensi FastAPI beserta 
aturan bisnis, serta pengujian deteksi *drift* dan pemicuan *retraining* otomatis.
```

---

## Gap 2 — Bagian `4.2 Skenario Pengujian` (Tabel Formal Hilang)

### Kondisi di PDF
Template PDF memiliki **Tabel 4.3: Skenario pengujian sistem** yang menyajikan seluruh skenario dalam format tabel formal dengan kolom `No.`, `Skenario`, dan `Tujuan Pengujian`. Selain itu, terdapat kalimat penutup yang menegaskan bahwa "pengujian tidak berhenti pada akurasi model" — sistem juga diuji dari sisi kualitas data, keamanan data sintetis, ketahanan aturan bisnis, dan kesiapan monitoring post-deployment.

### Kondisi di Laporan Draft
Laporan draft memiliki `## 4.2 Skenario Pengujian` dengan daftar bernomor (7 skenario), namun:
1. **Format berbeda** — menggunakan bullet list, bukan tabel formal seperti tabel di template PDF.
2. **Draft menampilkan 7 skenario**, sedangkan template PDF hanya memiliki **6 skenario** (Quality Gate TSTR/TRTR digabung sebagai satu skenario, bukan dua). Ini bisa menimbulkan inkonsistensi antara teks dengan daftar gambar/tabel di laporan akhir.
3. **Kalimat penutup analitis** (yang menegaskan pengujian bersifat holistik) tidak ada di draft.

### Draft Penambahan yang Direkomendasikan

Ganti daftar bernomor di `## 4.2 Skenario Pengujian` dengan tabel berikut, lalu tambahkan paragraf penutup:

```markdown
Skenario pengujian dirancang untuk mencakup seluruh siklus hidup sistem, mulai dari data 
mentah hingga pemantauan model setelah model digunakan. Rangkuman skenario pengujian 
ditunjukkan pada Tabel 4.X di bawah ini.

| No. | Skenario | Tujuan Pengujian |
| :---: | :--- | :--- |
| 1 | Preprocessing WoE dan IV | Menguji efektivitas transformasi fitur serta seleksi fitur berdasarkan kekuatan prediktif. |
| 2 | Penanganan imbalance dengan SMOTE | Memastikan data latih menjadi lebih seimbang sehingga model tidak bias terhadap kelas mayoritas. |
| 3 | Pelatihan dan tuning XGBoost | Menentukan konfigurasi model terbaik berdasarkan performa validasi dan evaluasi data uji. |
| 4 | Quality gate data sintetis | Menguji utilitas data sintetis dan risiko kebocoran privasi melalui metrik TSTR, TRTR, DCR, dan KS. |
| 5 | API serving dan aturan bisnis | Memverifikasi layanan prediksi FastAPI serta penerapan aturan bisnis yang bersifat wajib. |
| 6 | Drift dan retraining otomatis | Menguji deteksi perubahan distribusi data dan pemicuan webhook n8n untuk *retraining*. |

*Tabel 4.X Skenario pengujian sistem*

Pembagian skenario ini memperlihatkan bahwa pengujian tidak berhenti pada akurasi model. 
Sistem juga diuji dari sisi kualitas data, keamanan penggunaan data sintetis, ketahanan 
aturan bisnis, serta kesiapan pemantauan model setelah *deployment*.
```

---

## Gap 3 — Bagian `4.4 Validasi Awal Dataset Sintetis` (Hilang Sepenuhnya)

### Kondisi di PDF
Ini adalah **gap terbesar dan paling substansial.** Template PDF memiliki sub-bab lengkap **§4.4 Validasi Awal Dataset Sintetis** (3 halaman, halaman 50–53) yang berisi:

1. **Narasi justifikasi** — menjelaskan *mengapa* 50 baris sampel ditinjau, konteks validasi lapangan (magang di Bank Mandiri Taspen), dan tujuan membuktikan dataset bukan sekadar angka acak.
2. **Tabel 4.4: Profil Data 10 Nasabah Normal** — tabel longitudinal (portrait-landscape) yang menampilkan **semua 43+ atribut** dari 10 sampel nasabah riil dari dataset sintetis, mencakup:
   - Identitas nasabah (Customer ID, Nama, Usia, Jenis Kelamin, Status Pernikahan)
   - Profil pekerjaan (Jenis Pekerjaan, Status Pensiun, Sumber Gaji, Tipe Pemberi Kerja)
   - Kondisi finansial (Pendapatan Bulanan, Uang Pensiun, Saldo Rekening, Volatilitas Gaji, Stabilitas Pendapatan, Rasio Pengeluaran Medis)
   - Histori kredit (Skor Kredit Biro OJK, Bulan Tunggakan, Lama Riwayat Kredit, Total Cicilan Aktif, Jumlah Pinjaman Lain, DTI)
   - Parameter risiko kesehatan (Skor Risiko Kesehatan, Penyakit Kronis, Grade Pemeriksaan Medis)
   - Informasi asuransi (Status Asuransi Jiwa, Uang Pertanggungan, Perusahaan Asuransi, Premi Bulanan)
   - Detail produk kredit (Loan ID, Jenis Produk, Tanggal Pencairan, Jumlah Pinjaman, Tenor, Suku Bunga, Agunan, LTV, Sisa Pinjaman)
   - Perilaku keuangan (Penarikan Tunai Terakhir, Frekuensi Transaksi Digital, Pengeluaran Kartu)
   - Metrik risiko turunan (PD, LGD, EAD, EL, Flag Gagal Bayar)
3. **Paragraf penutup** yang menyatakan bahwa dataset dinilai memadai dan pengujian tahap berikutnya dapat dimulai.

### Kondisi di Laporan Draft
**Tidak ada** sub-bab `4.4 Validasi Awal Dataset Sintetis` sama sekali. Laporan draft langsung dari `4.3 Hasil Pengujian dan Analisis` → `4.3.1 Hasil Preprocessing`. Ini berarti laporan draft **melewati seluruh bagian bukti kewajaran data sintetis** yang diuji oleh pembimbing lapangan.

### Dampak
Ini adalah gap paling kritis. Tanpa bagian ini, tidak ada bukti bahwa dataset yang digunakan telah diverifikasi secara domain (industri perbankan). Penguji dapat mempertanyakan validitas seluruh eksperimen.

### Draft Teks yang Direkomendasikan

Sisipkan sebagai sub-bab baru antara `## 4.2 Skenario Pengujian` (atau `4.3`) dan `## 4.3.1 Hasil Preprocessing`:

```markdown
## 4.4 Validasi Awal Dataset Sintetis

Bagian ini menyajikan hasil validasi awal terhadap dataset sintetis yang rancangan 
pembentukannya telah dijelaskan pada Bab III. Validasi dilakukan untuk memastikan bahwa 
struktur atribut, format nilai, dan relasi antarkolom pada sampel data tetap konsisten 
dengan karakteristik umum data kredit konsumtif pada praktik perbankan.

Sebanyak 50 baris sampel data sintetis ditinjau sebagai bagian dari pemeriksaan kewajaran 
struktur data. Tabel 4.4 menampilkan 10 baris sampel dari data yang telah ditinjau 
tersebut. Sampel ini digunakan sebagai bukti bahwa dataset tidak hanya dibangkitkan secara 
numerik, tetapi juga memiliki atribut identitas, pekerjaan, finansial, histori kredit, 
parameter risiko, dan indikator makroekonomi yang saling berhubungan secara logis.

[**Catatan: Sisipkan Tabel 4.4: Profil Data 10 Nasabah Normal di sini.**  
Tabel ini dapat diambil dari file `tabel_10_nasabah_normal.md` yang sudah ada di direktori proyek.]

Berdasarkan sampel tersebut, dataset sintetis dinilai memadai untuk digunakan pada tahap 
pengujian berikutnya karena telah memuat variasi atribut nasabah, produk kredit, indikator 
risiko, serta variabel makroekonomi yang dibutuhkan dalam proses credit risk scoring. Dengan 
demikian, pengujian pada bagian selanjutnya dapat difokuskan pada kualitas 
*preprocessing*, performa model, validasi *quality gate*, dan kesiapan implementasi MLOps.
```

> **Catatan:** File `tabel_10_nasabah_normal.md` **sudah ada** di direktori proyek dan siap diintegrasikan. Ini tinggal di-*embed* ke dalam laporan Bab 4.

---

## Gap 4 — Sub-Bagian `4.9.1 Evaluasi Aturan Bisnis Batas Usia dan Mitigasi Tenor Dinamis` (Tidak Lengkap)

### Kondisi di PDF
Template PDF memiliki sub-bagian **§4.9.1** yang sangat rinci (2 halaman, halaman 66–67) yang menjelaskan:

1. **Latar belakang masalah** — sebelum aturan batas usia diterapkan, model XGBoost murni bisa *meluluskan* nasabah berusia 75–80+ tahun karena profil keuangannya terlihat kuat (pendapatan pensiun stabil, bureau score tinggi, DTI rendah). Ini adalah *celah risiko bisnis* yang tidak bisa ditangkap model statistik.

2. **Tiga Zona Keputusan** yang eksplisit dengan formula matematis:
   - **Zona Lolos** (usia pelunasan ≤ 80 tahun): request diteruskan ke XGBoost normal.
   - **Zona Mitigasi Dinamis** (usia saat pengajuan < 80, tapi tenor menyebabkan pelunasan > 80): Grade `Undefined` + **Tenor Maksimal Dinamis** dihitung via rumus: `Tenor Maks (Bulan) = (80 − Usia Pengajuan) × 12`
   - **Zona Joint-Borrower** (usia ≥ 80 tahun saat pengajuan): pengurangan tenor tidak lagi memadai; sistem merekomendasikan skema **co-borrower** dengan anggota keluarga muda.

3. **Persamaan matematis formal** untuk kedua formula usia pelunasan dan tenor maksimal.

4. **Narasi penutup** yang menyatakan bahwa sistem menyeimbangkan kecerdasan prediktif model dengan kepatuhan kebijakan perbankan.

### Kondisi di Laporan Draft
Laporan draft memiliki `### 4.3.6 Hasil Pengujian API Serving dan Aturan Bisnis` yang mencakup Kasus Uji 1 (nasabah normal) dan Kasus Uji 2 (pelanggaran aturan usia). Namun:

1. **Tidak ada pembahasan tiga zona keputusan** — laporan hanya menyebutkan "Grade Undefined" dan mitigasi tenor, tanpa menjelaskan bahwa terdapat zona mitigasi dinamis dan zona joint-borrower yang berbeda secara konseptual.
2. **Formula matematis formal tidak ada** — laporan hanya menyebutkan "menghitung sisa batas usia secara otomatis" tanpa menampilkan persamaan `Tenor Maks = (80 − Usia) × 12` secara eksplisit.
3. **Latar belakang masalah tidak ada** — tidak dijelaskan *mengapa* aturan ini perlu ada (karena model ML murni bisa loloskan nasabah tua dengan profil keuangan bagus).
4. **Zona joint-borrower tidak disebutkan sama sekali** — skema co-borrower sebagai alternatif ketiga tidak dibahas.

### Draft Teks yang Direkomendasikan

Tambahkan setelah `#### B. Kasus Uji 2:` sebagai sub-bagian baru:

```markdown
### 4.9.1 Evaluasi Aturan Bisnis Batas Usia dan Mitigasi Tenor Dinamis

#### Latar Belakang Permasalahan

Sebelum aturan pembatasan usia pelunasan diterapkan pada modul *serving*, sistem evaluasi 
risiko kredit sepenuhnya bergantung pada keluaran model XGBoost. Kondisi ini menimbulkan 
celah risiko bisnis karena model dapat meluluskan nasabah berusia lanjut — misalnya berusia 
75 hingga lebih dari 80 tahun — dengan peringkat risiko baik dan probabilitas *default* 
yang rendah. Anomali tersebut muncul karena sebagian nasabah lanjut usia memiliki profil 
keuangan yang terlihat kuat bagi model: aliran pendapatan pensiun yang stabil, skor kredit 
biro yang tinggi, rasio DTI yang rendah, serta status asuransi jiwa aktif.

Kelemahan tersebut menunjukkan bahwa model *machine learning* murni hanya menangkap 
korelasi matematis antarfitur, tetapi tidak secara eksplisit memahami risiko biologis dan 
batas kelayakan usia pelunasan. Sebagai contoh, nasabah berusia 75 tahun dengan profil 
pensiun stabil dapat dinilai rendah risiko oleh model, walaupun pengajuan tenor 10–15 tahun 
secara praktis dapat melewati batas usia pelunasan yang aman. Oleh karena itu, diperlukan 
lapisan logika bisnis tambahan agar hasil prediksi model tetap selaras dengan kebijakan 
manajemen risiko institusi.

#### Tiga Zona Keputusan Hibrida

Untuk mengatasi kondisi tersebut, diterapkan sistem logika hibrida pada lapisan API. Sistem 
ini menghitung usia saat jatuh tempo sebagai berikut:

$$\text{Usia Pelunasan} = \text{Usia Saat Pengajuan} + \frac{\text{Tenor (Bulan)}}{12} \quad (4.1)$$

Mekanisme mitigasi tersebut dibagi ke dalam **tiga zona keputusan**:

1. **Zona Lolos Batas Usia** — kondisi ketika usia pelunasan ≤ 80 tahun. Pada zona ini, 
   data tetap dikirimkan ke model XGBoost untuk memperoleh probabilitas *default* dan 
   *risk grade* secara normal.

2. **Zona Mitigasi Dinamis** — kondisi ketika usia nasabah saat pengajuan masih di bawah 
   80 tahun, tetapi tenor yang diminta menyebabkan usia pelunasan melewati 80 tahun. Pada 
   zona ini, sistem memberikan status `Grade Undefined` dan menghitung rekomendasi tenor 
   maksimal secara dinamis:

   $$\text{Tenor Maksimal (Bulan)} = (80 - \text{Usia Saat Pengajuan}) \times 12 \quad (4.2)$$

   Sebagai contoh, nasabah berusia 75 tahun yang mengajukan tenor 120 bulan akan 
   direkomendasikan untuk menurunkan tenor maksimal menjadi 60 bulan. Jika nasabah 
   memiliki asuransi jiwa aktif, sistem juga dapat memberikan rekomendasi dispensasi 
   komite kredit berbasis perlindungan asuransi.

3. **Zona Joint-Borrower** — kondisi ketika nasabah telah berusia 80 tahun atau lebih 
   pada saat pengajuan. Pada zona ini, pengurangan tenor tidak lagi memadai karena sisa 
   batas usia bernilai nol atau negatif. Sistem menyarankan alternatif pengajuan berbasis 
   *joint-income* atau *co-borrower* dengan anggota keluarga inti yang lebih muda.

Setelah mekanisme ini diterapkan, sistem berhasil menyeimbangkan kecerdasan prediktif 
model dengan kepatuhan terhadap kebijakan risiko perbankan. Nasabah lanjut usia yang 
sebelumnya dapat lolos karena profil keuangannya terlihat baik kini terjaring oleh deteksi 
anomali usia pelunasan. Namun sistem tidak langsung menolak secara kaku, melainkan 
memberikan rekomendasi terukur melalui pengurangan tenor, pertimbangan asuransi jiwa 
kredit, atau skema *joint-borrower*. Dengan demikian, aturan bisnis ini berfungsi sebagai 
lapisan pengaman tambahan yang membuat hasil inferensi model lebih realistis dan dapat 
diterapkan pada proses kredit operasional.
```

---

## Gap 5 — Bagian `4.11 Evaluasi Kritis dan Batasan Operasional Sistem` (Hilang Sepenuhnya)

### Kondisi di PDF
Ini adalah bagian **terpenting secara akademis** yang ada di template namun **tidak ada sama sekali** di laporan draft. Template PDF memiliki **§4.11 Evaluasi Kritis dan Batasan Operasional Sistem** (halaman 69–70) dengan tiga sub-bagian:

#### §4.11.1 — Ketiadaan Mekanisme Human-in-the-Loop pada Zona Ambang
- Sistem bekerja otomatis dengan ambang keputusan statis.
- Pada rentang PD 40%–60% (zona abu-abu), tingkat ketidakpastian tinggi.
- Tanpa *mandatory human review*, model berpotensi menghasilkan *false positive* atau *false negative* di zona ambang.
- Solusi yang diperlukan: *review flag* untuk mengalihkan pengajuan ke analis kredit.

#### §4.11.2 — Beban Finansial Spesifik yang Tidak Tercermin pada Risk Grade
- Berdasarkan sampel nasabah di Tabel 4.4, ada nasabah dengan rasio pengeluaran medis 42,09% dari pendapatan Rp3.285.278 → Rp1,38 juta/bulan untuk medis.
- Beban ini bisa menjadi *fixed commitment* yang mengurangi kapasitas bayar, namun tidak selalu tertangkap oleh model.
- Solusi: *manual override flag* atau catatan validator sebagai sinyal tambahan.

#### §4.11.3 — Kerentanan Subjektivitas pada Proses WoE Binning
- Metode *binning* (equal width, equal frequency, atau berbasis aturan bisnis) menghasilkan batas interval berbeda.
- Perubahan kecil pada batas bin bureau score bisa mengubah interpretasi risiko.
- Solusi: proses *binning* perlu divalidasi oleh komite risiko, bukan hanya algoritma.

### Kondisi di Laporan Draft
**Tidak ada** sub-bab evaluasi kritis sama sekali. Laporan draft berakhir di `### 4.3.7 Hasil Pengujian Deteksi Drift dan Webhook Retraining` tanpa refleksi kritis terhadap keterbatasan sistem.

### Dampak
Ini adalah standar wajib akademik ITS: **setiap penelitian harus mengakui batasan dan kelemahannya**. Tanpa bagian ini, laporan terkesan mengklaim bahwa sistem sudah sempurna, yang justru mengurangi kredibilitas ilmiah.

### Draft Teks yang Direkomendasikan (Sisipkan sebelum Bab V/Penutup)

```markdown
---

## 4.11 Evaluasi Kritis dan Batasan Operasional Sistem

Meskipun hasil pengujian menunjukkan bahwa pipeline MLOps telah mampu menjalankan 
otomasi pelatihan, *deployment*, monitoring *drift*, dan integrasi aturan bisnis, sistem 
ini masih memiliki beberapa batasan jika dilihat dari sudut pandang manajemen risiko riil. 
Evaluasi kritis ini penting karena sistem penilaian kredit tidak hanya harus akurat secara 
teknis, tetapi juga harus aman secara operasional dan dapat dipertanggungjawabkan secara 
ekonomi.

### 4.11.1 Ketiadaan Mekanisme Human-in-the-Loop pada Zona Ambang

Sistem inferensi yang diuji masih bekerja secara otomatis berdasarkan ambang keputusan 
yang bersifat statis. Pada nilai probabilitas gagal bayar atau *Probability of Default* 
(PD) yang berada pada **zona ambang**, misalnya 40% hingga 60%, tingkat ketidakpastian 
model relatif tinggi. Jika keputusan pada rentang ini sepenuhnya diserahkan kepada model 
XGBoost tanpa mekanisme *mandatory human review*, maka sistem berpotensi menghasilkan 
kesalahan klasifikasi, baik berupa *false positive* maupun *false negative*.

Dalam praktik operasional perbankan, zona abu-abu tersebut sebaiknya tidak diperlakukan 
sebagai keputusan otomatis. Sistem perlu memberikan tanda peringatan atau *review flag* 
agar pengajuan dialihkan kepada analis kredit atau *credit underwriter*. Dengan demikian, 
hasil prediksi model dapat dikombinasikan dengan penilaian kualitatif — seperti stabilitas 
pekerjaan, kondisi keluarga, dokumen pendukung, dan informasi tambahan yang tidak selalu 
tercermin di dalam fitur model.

### 4.11.2 Beban Finansial Spesifik yang Tidak Sepenuhnya Tercermin pada Risk Grade

Berdasarkan penelaahan deskriptif terhadap sampel data nasabah (Tabel 4.4), terdapat 
celah antara validasi lapangan dan keluaran model. Sebagai contoh, terdapat nasabah dengan 
pendapatan bulanan sekitar Rp3.285.278 dan rasio pengeluaran medis yang tinggi. Jika rasio 
pengeluaran medis mencapai 42,09%, maka sekitar **Rp1,38 juta per bulan** digunakan untuk 
biaya medis rutin. Secara operasional, beban tersebut dapat bertindak sebagai *fixed 
commitment* yang mengurangi kapasitas membayar (*repayment capacity*).

Kondisi serupa juga dapat terjadi pada pengeluaran non-medis yang bersifat mengikat, 
seperti biaya pendidikan anak berkebutuhan khusus atau kewajiban keluarga lain yang tidak 
selalu tertangkap secara eksplisit oleh model. Dalam beberapa kasus, model masih dapat 
memberikan *grade* yang relatif moderat karena didorong oleh fitur lain seperti *bureau 
score* yang tinggi.

Dengan demikian, keluaran model sebaiknya diposisikan sebagai **alat bantu keputusan**, 
bukan pengganti penuh proses validasi kredit. *Pipeline* perlu dikembangkan agar dapat 
menerima *manual override flag* atau catatan validator sebagai sinyal tambahan untuk 
menurunkan *grade*, mengubah status menjadi `Undefined`, atau mengalihkan kasus ke proses 
*review* manual.

### 4.11.3 Kerentanan Subjektivitas pada Proses WoE Binning

Transformasi Weight of Evidence (WoE) terbukti membantu stabilitas fitur dan mendukung 
interpretabilitas model. Namun, proses pembentukan interval atau *binning* pada variabel 
numerik tetap memiliki unsur sensitivitas metodologis. Perbedaan metode *binning*, seperti 
*equal width*, *equal frequency*, atau *binning* berbasis aturan bisnis, dapat menghasilkan 
batas interval yang berbeda dan pada akhirnya mengubah nilai WoE.

Jika batas *bin* ditentukan semata-mata oleh algoritma tanpa validasi dari komite risiko, 
model dapat menjadi benar secara teknis tetapi kurang tepat secara ekonomi. Sebagai contoh, 
perubahan kecil pada batas kelompok `bureau_score` dapat mengubah interpretasi risiko dan 
memengaruhi skor akhir nasabah. Oleh karena itu, proses *binning* perlu divalidasi tidak 
hanya berdasarkan metrik statistik, tetapi juga berdasarkan kewajaran bisnis dan kebijakan 
risiko lembaga keuangan.

Batasan ini menunjukkan bahwa implementasi *pipeline* MLOps pada domain kredit perlu 
dilengkapi dengan tata kelola model yang kuat. Setiap perubahan fitur, *binning* WoE, 
*threshold* PD, aturan *grading*, dan mekanisme *retraining* sebaiknya terdokumentasi, 
ditinjau, dan disetujui oleh pihak yang bertanggung jawab terhadap risiko model.

---
```

---

## Gap 6 — Bagian `4.12 Ringkasan Bab` (Hilang)

### Kondisi di PDF
Template PDF memiliki **§4.12 Ringkasan Bab** (halaman 70) sebagai penutup Bab 4. Paragraf ini merangkum hasil pengujian secara holistik, menghubungkan semua komponen (preprocessing, model, quality gate, API, aturan bisnis, drift) dan menyimpulkan bahwa sistem tidak hanya memenuhi kebutuhan pemodelan prediktif, tetapi juga menunjukkan kesiapan sebagai pipeline MLOps operasional.

### Kondisi di Laporan Draft
Laporan draft tidak memiliki ringkasan bab.

### Draft Teks yang Direkomendasikan

```markdown
---

## 4.12 Ringkasan Bab

Bab ini telah menguraikan hasil pengujian sistem mulai dari *preprocessing*, penanganan 
*imbalance*, pelatihan model, evaluasi performa, interpretabilitas, *quality gate* data 
sintetis, *API serving*, aturan bisnis, hingga deteksi *drift*. Hasil pengujian menunjukkan 
bahwa model XGBoost memperoleh AUC 0,8262 dengan keseimbangan presisi dan *recall* yang 
baik. Transformasi WoE dan seleksi IV membantu memperkuat dasar pemilihan fitur, sedangkan 
SHAP memberikan transparansi terhadap faktor yang memengaruhi prediksi.

Selain performa model, hasil pengujian juga menunjukkan bahwa data sintetis lolos *quality 
gate* utilitas dan privasi, layanan FastAPI mampu mengembalikan prediksi beserta penjelasan, 
aturan bisnis dapat mencegah keputusan yang melanggar kebijakan, dan sistem *drift 
monitoring* mampu memicu *retraining* otomatis melalui n8n. Dengan demikian, sistem yang 
dikembangkan tidak hanya memenuhi kebutuhan pemodelan prediktif, tetapi juga menunjukkan 
kesiapan sebagai *pipeline* MLOps untuk operasionalisasi model risiko kredit.
```

---

## Tabel Ringkasan Gap

| # | Bagian di Template PDF | Ada di Draft? | Keparahan | Aksi |
|:---:|:---|:---:|:---:|:---|
| 1 | §4.1 Pendahuluan Pengujian | ❌ Tidak ada | 🔴 Tinggi | Tambahkan sebelum §4.1 Lingkungan Pengujian |
| 2 | §4.3 Tabel Skenario Formal (Tabel 4.3) | ⚠️ Parsial | 🟡 Sedang | Ganti bullet list dengan tabel formal + paragraf penutup |
| 3 | §4.4 Validasi Awal Dataset Sintetis | ❌ Tidak ada | 🔴 Sangat Tinggi | Tambahkan sub-bab baru + embed tabel_10_nasabah_normal.md |
| 4 | §4.9.1 Tiga Zona Keputusan Usia (joint-borrower, formula matematis) | ⚠️ Parsial | 🔴 Tinggi | Perluas sub-bagian aturan bisnis dengan zona ke-3 dan formula |
| 5 | §4.11 Evaluasi Kritis & Batasan Operasional (3 sub-poin) | ❌ Tidak ada | 🔴 Sangat Tinggi | Tambahkan sub-bab baru sebelum Bab V |
| 6 | §4.12 Ringkasan Bab | ❌ Tidak ada | 🟡 Sedang | Tambahkan paragraf penutup Bab 4 |

---

## Catatan Tambahan: Perbedaan Gambar dan Antarmuka Web

Template PDF juga menyebut **Gambar 4.15: Antarmuka web untuk pengujian prediksi risiko kredit** (halaman 66) — sebuah screenshot dari UI web yang digunakan untuk menguji layanan prediksi. Laporan draft tidak memiliki screenshot UI ini. Jika antarmuka web sudah ada (kode `script.js` terlihat terbuka di editor), sebaiknya tambahkan screenshot-nya sebagai Gambar 4.X di bagian API serving.

---

*Laporan ini dibuat berdasarkan analisis penuh terhadap `TemplateTaTeknikKomputer(8).pdf` (76 hal.) dan `Bab4_Pengujian_dan_Analisis.md` pada 10 Juli 2026.*
