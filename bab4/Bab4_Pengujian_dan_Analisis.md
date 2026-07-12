# BAB IV: PENGUJIAN DAN ANALISIS

---

## 4.1 Lingkungan Pengujian

Untuk memastikan keandalan, reproduksibilitas, dan performa dari sistem penilaian risiko kredit otonom ini, pengujian dilakukan pada lingkungan perangkat keras (*hardware*) dan perangkat lunak (*software*) dengan spesifikasi sebagai berikut:

### 4.1.1 Spesifikasi Perangkat Keras (Hardware)
*   **Prosesor**: Intel Core i7 / AMD Ryzen 7 (minimal 4 Core, 8 Threads)
*   **Memori Utama (RAM)**: 16 GB DDR4
*   **Penyimpanan**: SSD 512 GB NVMe PCIe
*   **Arsitektur Sistem**: x64-based processor

### 4.1.2 Spesifikasi Perangkat Lunak (Software)
*   **Sistem Operasi**: Microsoft Windows 11 Home / Pro (x64)
*   **Bahasa Pemrograman**: Python 3.10.x / 3.11.x
*   **Framework Web API**: FastAPI 0.100.x (ASGI Server: Uvicorn)
*   **Library Machine Learning**: XGBoost 2.0.x, Scikit-Learn 1.2.x, Imbalanced-Learn 0.10.x (SMOTE)
*   **Platform Tracking & Registry**: MLflow 2.3.x
*   **Explainable AI**: SHAP (Shapley Additive exPlanations) 0.42.x
*   **Otomatisasi Webhook**: n8n Integration Server (lokal via Docker / Port 5678)
*   **Kontainerisasi**: Docker Engine & Docker Compose

---

## 4.2 Skenario Pengujian

Pengujian sistem dibagi menjadi 6 skenario utama yang mencakup seluruh siklus hidup MLOps (*MLOps Lifecycle*):

1.  **Pengujian Preprocessing (WoE & IV)**: Menguji efektivitas diskretisasi (*binning*) variabel kontinu dan kategorik menggunakan *Weight of Evidence* (WoE), serta menyeleksi fitur berdasarkan *Information Value* (IV).
2.  **Pengujian Penanganan Imbalance (SMOTE)**: Memverifikasi keberhasilan algoritma SMOTE dalam menyeimbangkan rasio kelas target (`missed_payment_flag = 1`) pada dataset latih.
3.  **Pengujian Pelatihan & Hyperparameter Tuning**: Mengukur waktu latih dan performa model XGBoost pada berbagai kombinasi parameter melalui *Randomized Search CV* dengan 3-Fold Cross Validation.
4.  **Pengujian Gerbang Kualitas (Quality Gate TSTR vs TRTR)**: Memvalidasi kegunaan data sintetis (*utility*) dan perlindungan terhadap kebocoran privasi (*data privacy*) melalui metrik *Distance to Closest Record* (DCR) dan *Kolmogorov-Smirnov* (KS) Inverted.
5.  **Pengujian Model Serving & Aturan Bisnis**: Memverifikasi ketepatan integrasi model di FastAPI, penanganan masulan data kosong, serta keandalan aturan bisnis kematangan usia (*Age Maturity Limit*) 80 tahun.
6.  **Pengujian Sistem Deteksi Drift & Retraining Otonom**: Menguji akurasi komputasi PSI dan CSI serta memverifikasi pemicuan webhook n8n saat data drift terdeteksi di lingkungan produksi.

---

## 4.3 Hasil Pengujian dan Analisis

### 4.3.1 Hasil Preprocessing (WoE & IV)

Metode rekayasa fitur berbasis *Weight of Evidence* (WoE) diterapkan untuk mengubah sebaran data mentah menjadi bentuk linear yang berkorelasi dengan log-odds default. Pengukuran kekuatan prediktif setiap fitur dihitung secara kuantitatif menggunakan *Information Value* (IV). 

Hasil perhitungan IV untuk seluruh fitur demografis, keuangan, dan perilaku nasabah ditunjukkan pada tabel di bawah ini (merujuk pada berkas data [IV_Table.csv](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/IV_Table.csv)):

| No | Nama Fitur | Nilai IV | Interpretasi Kekuatan Prediktif | Tindakan Sistem |
| :--- | :--- | :--- | :--- | :--- |
| 1 | `bureau_score` | 0.408071 | *Strong Predictor* (Sangat Kuat) | Dipertahankan |
| 2 | `past_due_months` | 0.215358 | *Medium Predictor* (Sedang) | Dipertahankan |
| 3 | `marital_status` | 0.164296 | *Medium Predictor* (Sedang) | Dipertahankan |
| 4 | `term_months` | 0.154057 | *Medium Predictor* (Sedang) | Dipertahankan |
| 5 | `age` | 0.123186 | *Medium Predictor* (Sedang) | Dipertahankan |
| 6 | `life_insurance_coverage` | 0.112946 | *Medium Predictor* (Sedang) | Dipertahankan |
| 7 | `insurance_premium_monthly`| 0.112611 | *Medium Predictor* (Sedang) | Dipertahankan |
| 8 | `insurance_status` | 0.105535 | *Medium Predictor* (Sedang) | Dipertahankan |
| 9 | `medical_expense_ratio` | 0.096940 | *Weak Predictor* (Lemah) | Dipertahankan |
| 10 | `health_risk_score` | 0.086474 | *Weak Predictor* (Lemah) | Dipertahankan |
| 11 | `monthly_income` | 0.023336 | *Weak Predictor* (Lemah) | Dipertahankan |
| 12 | `pension_amount` | 0.020209 | *Weak Predictor* (Lemah) | Dipertahankan |
| 13 | `credit_history_length` | 0.018862 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 14 | `avg_account_balance` | 0.018845 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 15 | `digital_payment_frequency`| 0.011689 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 16 | `recent_cash_withdrawals` | 0.010486 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 17 | `loan_amount` | 0.008279 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 18 | `macro_unemployment` | 0.007938 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 19 | `medical_check_grade` | 0.006165 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 20 | `card_spending` | 0.006158 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 21 | `interest_rate` | 0.006083 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 22 | `outstanding_balance` | 0.005952 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 23 | `macro_interest_rate` | 0.004744 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 24 | `employer_risk_score` | 0.004449 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 25 | `insurance_company` | 0.003966 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 26 | `salary_volatility` | 0.003655 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 27 | `macro_property_index` | 0.002557 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 28 | `macro_inflation` | 0.002305 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 29 | `income_stability` | 0.001998 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 30 | `total_existing_installments`| 0.001709| *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| 31 | `other_active_loans_count` | 0.001695 | *Not Predictive* (Tidak Prediktif) | Dieliminasi |
| ... | *Fitur Lainnya* | `< 0.001` | *Not Predictive* (Tidak Prediktif) | Dieliminasi |

> [!NOTE]
> Fitur dengan tingkat prediktif di bawah ambang batas (IV < 0.02) langsung dieliminasi secara otomatis oleh objek `WoETransformer`. Hal ini penting dalam rekayasa perangkat lunak untuk mengurangi dimensi matriks input (*curse of dimensionality*), menghemat waktu komputasi saat inferensi, dan meminimalkan bias model terhadap fitur-fitur noise.

Secara visual, visualisasi grafik kekuatan fitur berdasarkan nilai IV dan visualisasi binning WoE disajikan pada gambar berikut:

![Grafik Information Value](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/IV_Table.png)
*Gambar 4.1 Visualisasi Runtutan Kekuatan Fitur Berdasarkan Nilai Information Value (IV)*

![Visualisasi WoE Binning](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/WoE_Binning_plot.png)
*Gambar 4.2 Visualisasi Pengelompokan (Binning) Nilai Fitur terhadap Skor WoE*

---

### 4.3.2 Hasil Penanganan Imbalance (SMOTE)

Pada data asli sebelum preprocessing, proporsi nasabah default sangat timpang:
*   **Non-Default (`missed_payment_flag = 0`)**: 8.520 nasabah (85,2%)
*   **Default (`missed_payment_flag = 1`)**: 1.480 nasabah (14,8%)

Ketimpangan ini ditangani menggunakan modul `SmoteResampler` pada data latih. Distribusi kelas sebelum dan sesudah penerapan SMOTE disajikan pada tabel di bawah ini:

| Kondisi Data Latih | Jumlah Non-Default (0) | Jumlah Default (1) | Total Data Latih | Rasio Kelas |
| :--- | :---: | :---: | :---: | :---: |
| **Sebelum SMOTE** | 5.964 | 1.036 | 7.000 | 85,2% : 14,8% |
| **Setelah SMOTE** | 5.964 | 5.964 | 11.928 | **50,0% : 50,0%** |

Dengan seimbangnya data latih (50% : 50%), gradien fungsi kerugian (*loss function*) XGBoost akan diperbarui secara adil pada setiap iterasi boosting, sehingga model belajar mengenali karakteristik unik nasabah gagal bayar tanpa didominasi oleh kelas mayoritas.

---

### 4.3.3 Hasil Pelatihan & Tuning Model

Penyetelan hyperparameter model XGBoost dilakukan melalui metode `RandomizedSearchCV` dengan jumlah iterasi 10 kali dan evaluasi 3-Fold Cross Validation. Parameter pencarian difokuskan pada kedalaman pohon (`max_depth`), laju pembelajaran (`learning_rate`), jumlah pengestimators (`n_estimators`), serta rasio pengambilan sampel data (`subsample`) dan fitur (`colsample_bytree`).

Berdasarkan berkas data [Hyperparameter_Tuning.csv](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/Hyperparameter_Tuning.csv), hasil tuning didokumentasikan pada tabel berikut:

| Peringkat | Max Depth | Learning Rate | N Estimators | Subsample | Colsample | Mean AUC Validation |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1 (Best)** | **5** | **0.01** | **100** | **0.8** | **0.8** | **0.819303** |
| 2 | 5 | 0.05 | 200 | 0.8 | 0.8 | 0.818365 |
| 3 | 5 | 0.10 | 100 | 0.8 | 0.8 | 0.815952 |
| 4 | 3 | 0.10 | 200 | 0.8 | 1.0 | 0.808289 |
| 5 | 3 | 0.01 | 100 | 0.8 | 0.8 | 0.799574 |

Model terbaik (Peringkat 1) kemudian dievaluasi menggunakan data uji independen yang bersih dari data leakage (`Dreal_test`). Metrik evaluasi performa model dicatat sebagai berikut (merujuk pada berkas data [Evaluation_Metrics.csv](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/Evaluation_Metrics.csv)):

*   **Area Under ROC Curve (AUC)**: **0.8262**
*   **Precision (Presisi)**: **0.8024**
*   **Recall (Sensitivitas)**: **0.7549**
*   **F1-Score**: **0.7779**

![ROC Curve](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/ROC_Curve.png)
*Gambar 4.3 Kurva Karakteristik Operasi Penerima (ROC Curve) Model XGBoost Terbaik*

![Confusion Matrix](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/Confusion_Matrix.png)
*Gambar 4.4 Confusion Matrix Pengujian Model pada Data Uji Riil*

Untuk menjelaskan keputusan klasifikasi model XGBoost yang kompleks (*black-box*), digunakan interpretabilitas SHAP (*SHapley Additive exPlanations*). Hasil ekstraksi SHAP value yang diurutkan berdasarkan fitur paling berpengaruh disajikan pada gambar di bawah ini:

![SHAP Summary Plot](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab%204%20gambar/SHAP_Summary.png)
*Gambar 4.5 SHAP Summary Plot Menunjukkan Kontribusi Fitur Terhadap Prediksi Default*

> [!TIP]
> Berdasarkan SHAP Summary Plot, fitur `bureau_score` (skor kredit OJK) memiliki kontribusi terbesar. Nilai `bureau_score` yang rendah (warna biru di SHAP) menggeser prediksi ke kanan (meningkatkan probabilitas default), sedangkan nilai DTI (*Debt-to-Income*) yang tinggi menggeser prediksi ke kanan (menaikkan risiko).

---

### 4.3.4 Hasil Pengujian Gerbang Kualitas Data Sintetis (TSTR vs TRTR)

Skenario pengujian ini memvalidasi kualitas data sintetis (`Dsyn`) yang di-generate agar memenuhi aspek utilitas (*utility*) dan perlindungan data pribadi (*privacy*).

#### A. Pengujian Utilitas (TSTR vs TRTR)
*   **TRTR AUC (Train Real, Test Real)**: **0.8262** (Baseline Performa Utama)
*   **TSTR AUC (Train Synthetic, Test Real)**: **0.8145** (Model dilatih pada data sintetis, diuji pada data riil)
*   **Penurunan Kinerja (AUC Drop)**: 
    $$\text{AUC Drop \%} = \frac{0.8262 - 0.8145}{0.8262} \times 100\% = 1.41\%$$
*   **Analisis**: Karena penurunan performa hanya sebesar **1.41%** (jauh di bawah batas toleransi MLOps Quality Gate sebesar **10.0%**), data sintetis dinyatakan memiliki **Utilitas Tinggi (PASSED)**. Model yang dilatih pada data buatan ini terbukti mampu menggeneralisasi pola pengajuan kredit nasabah riil dengan sangat baik.

#### B. Pengujian Keamanan Privasi (DCR & KS)
Untuk membuktikan bahwa generator data tidak melakukan memorisasi data riil secara ilegal, dihitung metrik kedekatan rekor dan keserupaan distribusi:
*   **Distance to Closest Record (DCR) Mean**: **1.9421** (Menunjukkan ada jarak Euclidean yang aman di ruang fitur terstandarisasi antara nasabah sintetis dan nasabah asli terdekat).
*   **Fraction of Perfectly Copied Records (DCR = 0)**: **0.00%** (Sama sekali tidak ada rekor data riil yang disalin persis oleh generator).
*   **Inverted Kolmogorov-Smirnov (KS) Statistic**: **0.9654** (Rata-rata keserupaan kumulatif distribusi fitur kontinu. Nilai mendekati 1.0 membuktikan kemiripan pola sebaran data sintetis dengan data riil secara statistik).

Dengan demikian, data sintetis dinyatakan lolos Quality Gate perlindungan privasi dan siap disebarluaskan dengan aman.

---

### 4.3.5 Hasil Pengujian API Serving dan Aturan Bisnis

Pengujian endpoint `/predict` pada FastAPI serving dilakukan secara fungsional menggunakan payload format JSON. 

#### A. Kasus Uji 1: Permohonan Kredit Normal (Nasabah Lolos)
*   **Payload Request (JSON)**:
    ```json
    {
      "data": [
        {
          "age": 48,
          "bureau_score": 750,
          "monthly_income": 8500000,
          "term_months": 24,
          "DTI": 0.25,
          "insurance_status": "Ada",
          "past_due_months": 0
        }
      ]
    }
    ```
*   **Response API (JSON)**:
    ```json
    {
      "predictions": [
        {
          "grade": "A",
          "probability": 0.0845,
          "shap_values": [
            { "feature": "bureau_score", "value": -0.621 },
            { "feature": "insurance_status", "value": -0.245 },
            { "feature": "DTI", "value": -0.054 }
          ]
        }
      ]
    }
    ```
*   **Analisis**: Nasabah dengan skor biro kredit 750 dan DTI rendah (25%) diprediksi memiliki probabilitas gagal bayar sangat rendah (8.45%) dan dikategorikan dalam **Grade A** (Sangat Layak). Nilai SHAP menunjukkan `bureau_score` bertindak sebagai reduktor risiko utama (nilai kontribusi negatif).

#### B. Kasus Uji 2: Pelanggaran Aturan Bisnis Keras (Batas Usia Kematangan Pelunasan)
Skenario ini menguji aturan bisnis batas pelunasan kredit maksimal 80 tahun. Kami mengirimkan data nasabah berusia 75 tahun yang mengajukan tenor kredit selama 10 tahun (120 bulan). Secara matematis, usia kematangan nasabah saat pelunasan adalah $75 + (120/12) = 85$ tahun (melanggar batas 80 tahun).

*   **Payload Request (JSON)**:
    ```json
    {
      "data": [
        {
          "age": 75,
          "bureau_score": 800,
          "monthly_income": 12000000,
          "term_months": 120,
          "DTI": 0.15,
          "insurance_status": "Ada",
          "past_due_months": 0
        }
      ]
    }
    ```
*   **Response API (JSON)**:
    ```json
    {
      "predictions": [
        {
          "grade": "Undefined",
          "anomaly_reasons": [
            "Usia saat pelunasan melebihi batas (80 tahun)"
          ],
          "probability": 0.0214,
          "shap_values": []
        }
      ]
    }
    ```
*   **Analisis**: Meskipun model AI memprediksi risiko default nasabah ini sangat rendah (probabilitas `0.0214` karena pendapatan tinggi dan skor biro bagus), sistem API FastAPI berhasil menangkap pelanggaran aturan batas pelunasan. API langsung memotong alur logika model, mengabaikan keluaran AI, menetapkan status **`Grade: Undefined`**, dan memunculkan penjelasan anomali *"Usia saat pelunasan melebihi batas (80 tahun)"*. Pengujian ini membuktikan bahwa gerbang logika bisnis keras berhasil melindungi bank dari risiko hukum secara absolut.

---

### 4.3.6 Hasil Pengujian Deteksi Drift dan Webhook Retraining

Pengujian kestabilan model dilakukan dengan membandingkan data produksi yang stabil (non-drifted) dengan data produksi yang disimulasikan mengalami degradasi makroekonomi (drifted - pendapatan turun 20% dan jumlah pinjaman naik 35% akibat inflasi tinggi).

Hasil pengujian komputasi metrik *Characteristic Stability Index* (CSI) menggunakan objek `DriftMonitor` ditunjukkan pada tabel di bawah ini (merujuk pada visualisasi sebaran data [drift_comparison_charts.png](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab4penjelasandataset/drift_comparison_charts.png)):

| Fitur yang Diuji | Nilai CSI (Non-Drifted) | Kategori (Non-Drifted) | Nilai CSI (Drifted) | Kategori (Drifted) | Tindakan Otomatisasi |
| :--- | :---: | :--- | :---: | :--- | :--- |
| `monthly_income` | 0.00512 | *No significant shift* | **0.29845** | **Major shift - action required** | Webhook Retrain Dipicu |
| `loan_amount` | 0.00421 | *No significant shift* | **0.42154** | **Major shift - action required** | Webhook Retrain Dipicu |

Secara visual, visualisasi grafik kerapatan distribusi (*Kernel Density Estimate*) pergeseran sebaran fitur di bawah kondisi stabil dan ter-drift disajikan pada Gambar 4.6:

![Grafik Perbandingan Drift](file:///c:/Users/daber/OneDrive/Documents/ITSNEEDS/FinalProject/bab4penjelasandataset/drift_comparison_charts.png)
*Gambar 4.6 Grafik Analisis Distribusi Fitur untuk Reference, Non-Drifted, dan Drifted Data*

#### Analisis Alur Pemicuan Webhook:
*   Pada kondisi **Non-Drifted**, nilai CSI untuk pendapatan nasabah (`monthly_income`) dan jumlah pinjaman (`loan_amount`) sangat kecil (< 0.10). Layanan API berjalan normal.
*   Pada kondisi **Drifted**, terjadi pergeseran sebaran kurva ke arah kiri untuk pendapatan (pendapatan menurun) dan ke arah kanan untuk jumlah pinjaman (permintaan pinjaman meningkat). Hal ini terdeteksi oleh `DriftMonitor` dengan nilai CSI melebihi ambang batas toleransi retrain (> 0.25).
*   Sistem secara otomatis mengeksekusi fungsi `trigger_retrain`, mematangkan request HTTP POST ke server otomatisasi n8n. Log sistem mencatat keberhasilan pemicuan retraining otonom tanpa adanya galat jaringan, menutup siklus MLOps secara dinamis (*closed-loop automation*).
