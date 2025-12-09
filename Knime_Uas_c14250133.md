# 🎓 **Exploratory Data Analysis (EDA) Project – KNIME Universities Dataset**

## 📌 **1. Project Overview**

Project ini bertujuan melakukan eksplorasi data (EDA) menggunakan **KNIME Analytics Platform** terhadap dataset *Universities.csv*. Fokus utama analisis meliputi:

* Data preparation
* Data transformation
* Visualisasi data (Scatter Plot, Histogram, Box Plot, Statistics)
* Feature engineering (pembuatan kolom baru)
* Pembuatan *components* untuk penyajian visual secara terstruktur

Proyek **tidak mencakup klasifikasi** karena fokus hanya pada eksplorasi data.

---

## 📂 **2. Dataset**

Dataset berisi beberapa variabel terkait universitas, termasuk:

* `appl_rec_d` — jumlah aplikasi diterima
* `appl_accpt_d` — jumlah aplikasi yang menerima tawaran
* `acceptance_rate` — variabel hasil perhitungan
* `new_stu_enrolled` — mahasiswa baru masuk
* `%_new_stu_top_10` — persentase mahasiswa baru dari top 10%
* `percent_top_ten_new_stud_enrolled` — variabel hasil perhitungan kedua

Dua kolom fitur baru dibuat menggunakan **Math Formula**:

1. `acceptance_rate = appl_accpt_d / appl_rec_d`
2. `percent_top_ten_new_stud_enrolled = %_new_stu_top_10 / new_stu_enrolled`

---

## ⚙️ **3. KNIME Workflow Summary**

Alur kerja KNIME terdiri dari beberapa tahap:

### **✔ 3.1 Data Preparation**

Node yang digunakan:

* `File Reader` – membaca dataset CSV
* `Column Rename` – menyesuaikan nama kolom agar lebih mudah dikenali
* `Column Filter` – menghapus kolom yang tidak dipakai
* `String Cleaner` / `Number to String` – mengatur tipe data untuk visualisasi
* `Statistics` – menghasilkan ringkasan statistik kolom

### **✔ 3.2 Feature Engineering**

Menggunakan node:

* `Math Formula` → menghitung acceptance rate
* `Math Formula` kedua → menghitung rasio mahasiswa top 10%

### **✔ 3.3 Data Exploration & Visualization**

Visualisasi utama:

#### **1) Scatter Plot**

* Scatter Plot 1:

  * X: `appl_rec_d`
  * Y: `appl_accpt_d`
  * Insight: tren naik → semakin banyak aplikasi, semakin banyak yang diterima.

* Scatter Plot 2:

  * X: `appl_rec_d`
  * Y: `acceptance_rate`
  * Insight: tren menurun → semakin banyak aplikasi, semakin kecil acceptance rate.

#### **2) Histogram**

* Menampilkan distribusi:

  * `acceptance_rate`
  * `percent_top_ten_new_stud_enrolled`

#### **3) Box Plot**

* Digunakan untuk melihat outliers pada:

  * `new_stu_enrolled`
  * `%_new_stu_top_10`

#### **4) Statistics**

* Melihat **mean**, **median**, **min**, **max**, dan **standard deviation** untuk kolom baru dan lama.

### **✔ 3.4 Component**

Untuk merapikan visualisasi, dibuat sebuah **component** berisi:

* Scatter Plot
* Histogram
* Statistics

Component ini memudahkan pengguna menjalankan ulang seluruh visualisasi secara terstruktur.

---

## 🔍 **4. Insights & Interpretation**

### **1. Hubungan antara aplikasi masuk dan aplikasi diterima**

Scatter Plot menunjukkan hubungan linear positif:

* Universitas dengan aplikasi masuk banyak → cenderung menerima banyak mahasiswa.
  Ini logis pada universitas besar atau populer.

### **2. Acceptance Rate vs jumlah aplikasi**

Tren menurun:

* Semakin besar jumlah aplikasi → Acceptance rate cenderung semakin kecil.
  Ini menandakan universitas lebih selektif ketika peminatnya tinggi.

### **3. Distribusi penerimaan mahasiswa top 10%**

Histogram menunjukkan variasi besar antar universitas:

* Ada universitas dengan proporsi tinggi mahasiswa top 10%
* Ada juga yang sangat rendah

### **4. Outliers**

Box plot menunjukkan beberapa universitas dengan mahasiswa baru sangat tinggi:

* Perlu diperiksa lebih lanjut karena ini bisa mempengaruhi interpretasi.

---

## 🧾 **5. Final KNIME Structure (Ideal Layout)**

```
📁 KNIME Project
│
├── Data Preparation
│   ├── File Reader
│   ├── Column Rename
│   ├── Column Filter
│   ├── String Cleaner / Number To String
│   └── Statistics
│
├── Feature Engineering
│   ├── Math Formula (acceptance rate)
│   └── Math Formula (top ten ratio)
│
├── Visualization Component
│   ├── Scatter Plot 1
│   ├── Scatter Plot 2
│   ├── Histogram
│   └── Statistics
│
└── (optional) Further Analysis
```

---

## 📌 **6. Conclusion**

Melalui eksplorasi dataset Universitas menggunakan KNIME, diperoleh beberapa temuan penting:

* Pola penerimaan mahasiswa mengikuti tren yang konsisten
* Acceptance rate berkaitan erat dengan jumlah aplikasi
* Kolom tambahan (feature engineering) memberikan wawasan baru
* Visualisasi mempermudah interpretasi selektivitas universitas
* Workflow modular KNIME memudahkan re-run dan pengembangan

Proyek ini telah mencakup seluruh bagian yang diminta: **Data preparation → Data processing → Visualization**.
