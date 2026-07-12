# Analisis Tata Bahasa — Bab 2 & Bab 3
**Standar**: Penulisan Akademik S1 Indonesia (PUEBI + Pedoman Penulisan TA ITS)

---

## 📌 Penilaian Umum

Secara keseluruhan, bahasa yang digunakan **sudah di atas rata-rata** untuk penulisan TA S1. Penulis mampu menggunakan kosakata teknis dengan benar dan memahami gaya akademik. Namun, terdapat beberapa pola berulang yang perlu diperbaiki agar laporan terasa lebih matang dan konsisten secara ilmiah.

| Aspek | Nilai | Catatan |
|---|---|---|
| Kosakata teknis | ✅ Baik | Istilah digunakan dengan tepat |
| Keefektifan kalimat | ⚠️ Cukup | Banyak kalimat terlalu panjang |
| Konsistensi istilah | ⚠️ Cukup | Ada pencampuran Bahasa Indonesia & Inggris |
| Struktur paragraf | ⚠️ Cukup | Beberapa paragraf tidak memiliki kalimat topik |
| Penulisan kata asing (PUEBI) | ❌ Perlu perbaikan | Tidak konsisten dalam italicize |
| Penghindaran kalimat pasif ganda | ❌ Perlu perbaikan | Terlalu banyak konstruksi pasif bertumpuk |
| Penggunaan kata hubung | ✅ Baik | Cukup variatif |

---

## 🔴 Masalah 1: Kalimat Terlalu Panjang dan Bertumpuk

### Prinsip
Kalimat akademik S1 yang baik idealnya **tidak melebihi 30–35 kata**. Kalimat panjang membuat pembaca kesulitan memahami inti gagasan.

### Contoh dari Bab 2 (baris 5)
> *"Penelitian ini menggunakan beberapa penelitian terdahulu yang relevan sebagai dasar pengembangan, pembanding, dan penentuan **celah penelitian (research gap)** terhadap penelitian yang dilakukan. Beberapa penelitian utama yang dijadikan referensi terkait Machine Learning dalam analisis risiko kredit dan implementasi Machine Learning Operations (MLOps) dijelaskan sebagai berikut."*

**Masalah**: Kalimat kedua hanya berfungsi sebagai "basa-basi" dan tidak menambah informasi baru.

**Saran Perbaikan**:
> *"Penelitian ini mengacu pada beberapa penelitian terdahulu yang relevan sebagai dasar pengembangan dan pembanding dalam menentukan celah penelitian. Referensi utama yang digunakan meliputi studi mengenai Machine Learning dalam analisis risiko kredit serta implementasi MLOps, yang diuraikan sebagai berikut."*

---

### Contoh dari Bab 3 (baris 7)
> *"Dalam konteks ini, permasalahan utama yang diangkat adalah kebutuhan akan sistem penilaian risiko kredit yang akurat, dapat dijelaskan, dan mampu dipelihara secara berkelanjutan ketika distribusi data berubah dari waktu ke waktu."*

**Masalah**: 30+ kata dalam satu kalimat, dengan tiga predikat paralel yang panjang.

**Saran Perbaikan**:
> *"Permasalahan utama yang diangkat dalam penelitian ini adalah kebutuhan akan sistem penilaian risiko kredit yang akurat, dapat dijelaskan, dan berkelanjutan. Ketiga aspek tersebut menjadi krusial ketika distribusi data berubah seiring waktu."*

---

## 🔴 Masalah 2: Konstruksi Pasif Bertumpuk (Passive Stacking)

### Prinsip
Penulisan akademik Indonesia menggunakan kalimat pasif (ber- / di-) secara wajar, tetapi penggunaan **pasif yang ditumpuk** dalam satu kalimat membuat kalimat terasa kaku dan tidak efisien.

### Contoh dari Bab 3 (baris 13)
> *"Tahap ini mencakup penentuan tujuan model, definisi target prediksi, identifikasi kendala regulasi, serta penetapan kebutuhan interpretabilitas model."*

**Masalah**: Empat nomina deverbal (*penentuan, definisi, identifikasi, penetapan*) dalam satu kalimat menciptakan kepadatan yang sulit dicerna.

**Saran Perbaikan**:
> *"Pada tahap ini, tujuan model dan target prediksi ditetapkan, kendala regulasi diidentifikasi, serta kebutuhan interpretabilitas model ditentukan."*

---

### Contoh dari Bab 2 (baris 35)
> *"Langkah-langkah data preprocessing yang robust sangat penting, terutama dalam menangani isu class imbalance (ketidakseimbangan kelas), di mana kasus gagal bayar (default) selalu merupakan kelas minoritas dalam dataset kredit."*

**Masalah**: Penggunaan "di mana" sebagai klausa relatif adalah terjemahan harfiah dari "where" dalam bahasa Inggris — **tidak sesuai PUEBI**.

**Saran Perbaikan**:
> *"Langkah pra-pemrosesan yang baik sangat penting, terutama dalam menangani ketidakseimbangan kelas (class imbalance). Kasus gagal bayar (default) umumnya merupakan kelas minoritas dalam dataset kredit sehingga penanganan khusus diperlukan."*

---

## 🟠 Masalah 3: Penggunaan "di mana" yang Tidak Sesuai PUEBI

### Prinsip
Kata **"di mana"** dalam bahasa Indonesia adalah kata tanya lokasi (*"Di mana kamu?"*). Penggunaannya sebagai konjungsi penghubung klausa relatif (seperti "where" dalam Inggris) adalah **kesalahan yang sangat umum** dalam penulisan akademik Indonesia dan **tidak sesuai PUEBI**.

**Alternatif yang benar**: "yang", "sehingga", "dengan", "hal ini", "kondisi ini"

| ❌ Salah | ✅ Benar |
|---|---|
| *...model drift, di mana performa turun...* | *...model drift, yaitu kondisi ketika performa model turun...* |
| *...pipeline MLOps, di mana setiap tahap...* | *...pipeline MLOps yang menghubungkan setiap tahap...* |
| *...data sintetis, di mana distribusinya...* | *...data sintetis dengan distribusi yang...* |

**Temuan**: Kata "di mana" digunakan setidaknya **3 kali** dalam Bab 2 dan Bab 3 dengan konteks yang keliru.

---

## 🟠 Masalah 4: Penulisan Istilah Asing Tidak Konsisten

### Prinsip (PUEBI Pasal 12)
Kata atau ungkapan asing yang belum diserap ke dalam Bahasa Indonesia harus ditulis **miring (italic)**. Jika sudah ada padanan Bahasa Indonesia yang baku, gunakan padanan tersebut, dengan istilah asingnya dalam kurung italic di penyebutan pertama.

### Temuan

| Pola Bermasalah | Contoh di Teks | Seharusnya |
|---|---|---|
| Istilah Inggris tanpa italic | `data preprocessing`, `class imbalance` (baris 35) | *data preprocessing*, *class imbalance* |
| Italic hanya sebagian | `\textit{data preprocessing}` di satu tempat, tapi tidak di tempat lain | Konsisten |
| Campuran italic dan non-italic untuk istilah yang sama | `Machine Learning` (kadang italic, kadang tidak) | Pilih satu: selalu italic atau sudah diserap |
| Bold untuk istilah teknis | `\textbf{ensemble learning}` — seharusnya italic, bukan bold | `\textit{ensemble learning}` |

### Contoh Perbaikan (Bab 2, baris 17)
```
❌ sebelum:
Model berbasis \textbf{ensemble learning} seperti \textbf{XGBoost} memiliki performa...

✅ sesudah:
Model berbasis \textit{ensemble learning} seperti XGBoost memiliki performa...
```
> **Catatan**: XGBoost adalah nama produk/akronim yang tidak perlu italic maupun bold.

---

## 🟠 Masalah 5: Penggunaan Bold (\textbf) Berlebihan

### Prinsip
Bold dalam teks akademik digunakan untuk **mendefinisikan istilah kunci** pada kemunculan pertama, bukan untuk sekadar menekankan kata yang dianggap penting. Penggunaan bold yang berlebihan mengurangi efeknya.

### Temuan di Bab 2
Dalam satu paragraph (baris 5) saja ditemukan: `\textbf{celah penelitian (research gap)}` — bold satu frasa sekaligus mengandung istilah asing, padahal idealnya hanya istilah asing yang di-italic, dan definisinya tidak perlu di-bold.

**Prinsip penggunaan yang benar**:
- **Bold**: Hanya untuk istilah yang sedang didefinisikan pertama kali (max 1–2x per subbab)
- *Italic*: Untuk semua kata/frasa asing
- Normal: Untuk penekanan biasa → ganti dengan struktur kalimat yang lebih kuat

---

## 🟡 Masalah 6: Kalimat Topik Paragraf yang Lemah atau Tidak Ada

### Prinsip
Setiap paragraf dalam penulisan akademik harus memiliki **kalimat topik** (topic sentence) di awal yang merangkum ide utama paragraf tersebut. Kalimat-kalimat berikutnya mengembangkan dan mendukung kalimat topik.

### Contoh dari Bab 3 (baris 39)
> *"Variabel yang disimulasikan mencakup atribut demografis, atribut finansial, dan atribut perilaku pembayaran. Atribut demografis meliputi usia, status pekerjaan, dan masa kerja. Atribut finansial meliputi pendapatan bulanan, rasio angsuran terhadap pendapatan, jumlah pinjaman, dan tenor pinjaman. Atribut perilaku mencakup riwayat keterlambatan pembayaran dan intensitas penggunaan fasilitas kredit. Struktur variabel tersebut dipilih karena secara praktis sering digunakan dalam proses penilaian risiko kredit dan memiliki keterkaitan langsung dengan kemungkinan gagal bayar."*

**Masalah**: Paragraf ini hanya mendaftar — kalimat topik yang baik harusnya menjelaskan **mengapa** variabel-variabel ini dipilih, baru kemudian dijabarkan.

**Saran Perbaikan Kalimat Topik**:
> *"Pemilihan variabel dalam dataset sintetis didasarkan pada relevansinya terhadap proses penilaian risiko kredit secara praktis."* [lanjutkan dengan daftar]

---

## 🟡 Masalah 7: Repetisi Kata Transisi yang Sama

### Prinsip
Variasi kata penghubung membuat tulisan lebih hidup. Menggunakan kata yang sama berulang kali di awal kalimat terkesan monoton.

### Temuan di Bab 3
Kata **"Selain itu"** dan **"Oleh karena itu"** digunakan sangat sering sebagai pembuka kalimat/paragraf. Dalam satu subbab bisa muncul 2–3 kali.

**Alternatif kata transisi yang lebih bervariasi**:
| Fungsi | Variasi yang Bisa Digunakan |
|---|---|
| Tambahan | *Selain itu, Lebih lanjut, Di samping itu, Lebih jauh* |
| Kesimpulan | *Oleh karena itu, Dengan demikian, Berdasarkan hal tersebut, Oleh sebab itu* |
| Kontras | *Namun, Akan tetapi, Meskipun demikian, Di sisi lain* |
| Urutan | *Pertama, Selanjutnya, Kemudian, Pada tahap berikutnya* |

---

## 🟡 Masalah 8: Penggunaan "Dapat" yang Berlebihan

### Temuan
Kata **"dapat"** digunakan sangat sering untuk menyatakan kemungkinan, padahal dalam banyak konteks tidak diperlukan karena pernyataannya sudah bersifat faktual/prosedural.

| ❌ Terlalu banyak "dapat" | ✅ Lebih tegas |
|---|---|
| *"MLflow dapat digunakan untuk menyimpan parameter..."* | *"MLflow digunakan untuk menyimpan parameter..."* |
| *"Pendekatan ini dapat menjadi solusi yang lebih praktis..."* | *"Pendekatan ini merupakan solusi yang lebih praktis..."* |
| *"Model dapat diperbarui secara otomatis..."* | *"Model diperbarui secara otomatis..."* |

**Kapan "dapat" memang diperlukan**: Ketika menyatakan kemampuan opsional atau potensi yang belum tentu terjadi (misalnya: *"Sistem ini dapat dikembangkan lebih lanjut untuk..."*).

---

## 🟢 Masalah 9: Penulisan Angka Tidak Konsisten

### Prinsip (PUEBI)
- Angka **1–10** ditulis dengan huruf: *satu, dua, tiga*
- Angka **>10** ditulis dengan angka: *11, 12, 50*
- Angka di awal kalimat **selalu ditulis dengan huruf**

### Temuan di Bab 3 (baris 37)
> *"Data yang dibangkitkan terdiri atas **10.000** sampel..."* — ✅ Ini benar, angka besar ditulis dengan angka.

> *"...label kelas minoritas ditetapkan sebesar **14.8%**..."* — ✅ Benar untuk persentase.

> *"...sebanyak **50** baris sampel data..."* (baris 51) — ⚠️ Karena 50 > 10, ini benar ditulis dengan angka, **tetapi** konsistensinya perlu dijaga di seluruh dokumen. Pastikan tidak ada "dua puluh lima" dan "25" untuk konteks yang setara di tempat berbeda.

---

## 📋 Pedoman Cepat Penulisan Akademik S1 (Ringkasan)

### ✅ Yang Harus Dilakukan
1. **Satu ide utama per paragraf** — awali dengan kalimat topik
2. **Kalimat ≤ 30 kata** — jika lebih, pecah jadi dua kalimat
3. **Kata asing = italic** — konsisten di seluruh dokumen
4. **Angka sesuai PUEBI** — huruf untuk 1–10, angka untuk >10
5. **Variasikan kata transisi** — jangan gunakan "selain itu" >2x per halaman
6. **Bold hanya untuk definisi** — bukan penekanan umum
7. **Hindari "di mana" sebagai konjungsi** — ganti dengan "yang", "sehingga", dll.

### ❌ Yang Harus Dihindari
1. Kalimat pasif bertumpuk (*penentuan, penetapan, identifikasi* dalam satu kalimat)
2. "Di mana" sebagai *relative clause conjunction*
3. "Dapat" berlebihan untuk pernyataan faktual
4. Bold pada istilah asing (seharusnya italic)
5. Paragraf yang hanya berupa daftar tanpa kalimat topik dan penutup

---

## 🔧 Contoh Revisi Lengkap — Sebelum vs Sesudah

### Contoh 1 (Bab 2, baris 34–35)

**❌ Sebelum:**
> *"Studi komparatif dan penelitian mengenai credit scoring sangat menekankan bahwa performa prediktif model ML, terlepas dari kompleksitas algoritmanya, sangat bergantung pada kualitas dan persiapan data awal. Langkah-langkah data preprocessing yang robust sangat penting, terutama dalam menangani isu class imbalance (ketidakseimbangan kelas), di mana kasus gagal bayar (default) selalu merupakan kelas minoritas dalam dataset kredit."*

**✅ Sesudah:**
> *"Berbagai studi komparatif mengenai credit scoring menunjukkan bahwa performa prediktif model ML sangat bergantung pada kualitas data awal, terlepas dari kompleksitas algoritmanya. Langkah pra-pemrosesan yang andal sangat penting, terutama dalam menangani ketidakseimbangan kelas (class imbalance). Pada dataset kredit, kasus gagal bayar (default) umumnya merupakan kelas minoritas sehingga teknik penyeimbangan khusus diperlukan untuk pelatihan model yang efektif."*

---

### Contoh 2 (Bab 3, baris 15–16)

**❌ Sebelum:**
> *"Penelitian ini menggunakan pendekatan pipeline machine learning end-to-end. Arsitektur sistem dirancang secara modular agar setiap komponen dapat dikembangkan, diuji, dan dipelihara secara terpisah. Pemisahan ini penting untuk mendukung skalabilitas sistem, mempermudah pelacakan eksperimen, serta mengurangi ketergantungan antarproses ketika terjadi pembaruan model atau perubahan data."*

**✅ Sesudah:**
> *"Arsitektur sistem dirancang menggunakan pendekatan \textit{pipeline machine learning end-to-end} yang bersifat modular. Modularitas ini memungkinkan setiap komponen dikembangkan, diuji, dan dipelihara secara terpisah, sehingga mendukung skalabilitas sistem dan mempermudah pelacakan eksperimen. Selain itu, pemisahan antarkomponen mengurangi ketergantungan antarproses ketika terjadi pembaruan model atau perubahan data."*

---

> [!TIP]
> **Rekomendasi Tools**: Gunakan [LanguageTool](https://languagetool.org/) dengan bahasa Indonesia untuk pengecekan otomatis, atau minta pembimbing/peer untuk melakukan *proofreading* dengan fokus pada: (1) kalimat > 30 kata, (2) penggunaan "di mana", dan (3) konsistensi italic.
