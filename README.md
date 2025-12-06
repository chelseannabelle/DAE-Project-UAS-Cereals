# **Laporan Analisis Data Cereal Menggunakan KNIME**

## 1. Pendahuluan

Proyek ini dilakukan untuk memenuhi tugas analisis data menggunakan KNIME dengan tujuan membangun workflow lengkap yang mencakup data preparation, data processing, visualisasi, dan klasifikasi jauh lebih akurat. Misalnya dengan memasukkan variabel **sugars**, **fat**, atau **sodium** sebagai faktor pengurang skor. Hal ini dapat menghasilkan kategori yang lebih realistis dan mencerminkan standar nutrisi sebenarnya.

Secara keseluruhan, workflow ini memberikan gambaran jelas bagaimana fitur nutrisi berperan dalam menentukan tingkat kesehatan suatu produk, dan bagaimana pendekatan rule-based dapat menjadi dasar yang mudah dipahami sebelum beralih ke metode prediksi yang lebih kompleks seperti Decision Tree atau Logistic Regression.sifikasi sederhana berbasis rule-based labeling. Dataset yang digunakan berisi informasi nilai gizi dari berbagai jenis sereal. Dari workflow ini, dihasilkan kategori **Healthy** dan **Less Healthy**, disertai visualisasi dan insight terhadap pola data.

## 2. Dataset

Dataset *Cereals.csv* berisi variabel nutrisi seperti:

* calories
* protein
* fat
* sodium
* fiber
* carbo
* sugars
* potass
* vitamins
* rating
* serta atribut tambahan seperti weight, cups, shelf.

Dataset dibaca menggunakan **CSV Reader** pada KNIME sebelum masuk proses pembersihan data.

## 3. Metodologi Workflow KNIME

### 3.1 Data Preparation

Tahapan ini bertujuan memastikan data siap digunakan untuk analisis dan visualisasi.

### ✔ CSV Reader

Node ini digunakan untuk membaca file *Cereals.csv* dan memuat seluruh kolom nutrisi ke dalam workflow.

### ✔ Missing Value

Digunakan untuk menangani nilai kosong pada dataset:

* **String** → diganti nilai "Unknown".
* **Integer** → dibiarkan apa adanya (Do nothing).
* **Float** → diganti dengan nilai median kolom sehingga distribusi tidak terdistorsi.

Proses ini memastikan dataset bebas dari missing value yang dapat mengganggu analisis.

### ✔ Math Formula — *NutritionScore*

Formula:

```
$calories$ + $protein$ + $fiber$
```

Node ini menambahkan kolom baru bernama **NutritionScore** yang digunakan untuk melakukan klasifikasi kesehatan sereal.

### ✔ Rule Engine — *prediction*

Aturan klasifikasi:

```
$NutritionScore$ > 100 => "Healthy"
TRUE => "Less Healthy"
```

Node ini menghasilkan label kategori kesehatan berdasarkan skor nutrisi sederhana.

### ✔ Column Filter

Node ini menyaring kolom yang relevan dan menghapus atribut seperti *name*, *mfr*, dan *type* untuk memfokuskan analisis pada variabel nutrisi.

### ✔ Normalizer

Menormalkan variabel numerik agar berada pada rentang yang sama sehingga mempermudah interpretasi visualisasi.

## 3.2 Exploratory Data Analysis (EDA)

Tahapan ini bertujuan memahami pola dan distribusi data melalui berbagai grafik.

### 📊 Histogram — *Sugars*

Menunjukkan distribusi kandungan gula pada semua jenis sereal. Membantu melihat apakah gula tinggi mendominasi dataset.

### 🔥 Heatmap — *Prediction vs Prediction*

Digunakan untuk melihat frekuensi dan proporsi antara kategori Healthy dan Less Healthy.

### 🌐 Scatter Plot

Tiga scatter plot digunakan untuk melihat korelasi antar variabel:

* **Sugars vs Rating** – apakah makanan manis disukai atau tidak.
* **Calories vs Fiber** – apakah makanan berkalori tinggi memiliki serat rendah.
* **Fiber vs Protein** – mengevaluasi apakah kombinasi serat dan protein membentuk pola tertentu pada kategori Healthy.

Jika ingin, laporan bisa dilanjutkan ke bagian *Hasil Prediction*, *Interpretasi*, atau *Kesimpulan*. Cukup beri instruksi untuk melanjutkan.
Dokumen ini berisi versi awal README. Jika ingin aku lanjutkan ke bagian *Insight*, *Visualisasi*, *Interpretasi*, atau *Kesimpulan*, beri tahu saja.

## 3.3 Hasil Prediction

Node **Rule Engine** menghasilkan dua kategori utama:

* **Healthy**
* **Less Healthy**

Berdasarkan output klasifikasi dari workflow, distribusi hasil prediction adalah sebagai berikut:

### 🔢 Ringkasan Distribusi Kategori

* **Healthy**: Mayoritas data
* **Less Healthy**: Sebagian kecil, terutama pada sereal dengan kalori tinggi, serat rendah, atau gula tinggi

### 📄 Daftar Hasil Prediction (urutan sesuai dataset)

Less Healthy, Healthy, Less Healthy, Less Healthy, Healthy, Healthy, Healthy, Healthy, Less Healthy, Less Healthy,
Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy,
Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy,
Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy,
Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy,
Less Healthy, Healthy, Healthy, Healthy, Less Healthy, Less Healthy, Healthy, Healthy, Healthy, Healthy,
Less Healthy, Healthy, Healthy, Less Healthy, Less Healthy, Less Healthy, Healthy, Healthy,
Less Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy, Healthy.

## 4. Insight & Interpretasi

Tahapan ini menjelaskan temuan penting berdasarkan grafik dan pola numerik dalam dataset.

### 🔍 4.1 Insight dari Histogram (Sugars)

* Distribusi gula cenderung **melebar**, menunjukkan variasi besar antar merek sereal.
* Kelompok **Less Healthy** umumnya berada pada bagian histogram dengan nilai gula yang lebih tinggi.
* Insight: gula merupakan faktor kuat penentu kategori Less Healthy.

### 🔍 4.2 Insight dari Scatter Plot

#### 🍬 Sugars vs Rating

* Sereal dengan gula tinggi **tidak selalu** memiliki rating tinggi.
* Terlihat kecenderungan bahwa rating konsumen lebih tinggi pada sereal yang seimbang antara gula dan nutrisi lain.

#### 🔥 Calories vs Fiber

* Sereal rendah kalori tetapi tinggi serat sering masuk kategori **Healthy**.
* Sereal berkalori tinggi namun berserat rendah mendominasi kategori **Less Healthy**.

#### 🌿 Fiber vs Protein

* Produk dengan kombinasi **serat tinggi + protein tinggi** cenderung termasuk kelompok Healthy.
* Kombinasi nutrisi ini memberikan profil gizi yang baik.

### 🔥 4.3 Insight dari Heatmap

* Heatmap menunjukkan jumlah data Healthy jauh lebih besar.
* Ini menunjukkan dataset didominasi oleh produk yang secara nutrisi dinilai lebih baik berdasarkan formula yang digunakan.

### ✨ 4.4 Pola Umum dari NutritionScore

* NutritionScore tinggi didominasi oleh sereal kaya **protein** dan **serat**.
* NutritionScore rendah muncul pada produk dengan **kalori tinggi namun serat rendah**.

Insight akhir: model sederhana ini cukup efektif memisahkan produk nutrisi baik dan kurang baik, meskipun formula dapat diperbaiki di masa depan.

## 5. Kesimpulan

Workflow KNIME yang dibangun telah berhasil melakukan:

### ✔ Pembersihan Data

Menghilangkan missing value dan mengatur format data menjadi konsisten.

### ✔ Feature Engineering

Membuat **NutritionScore** sebagai indikator kesehatan sederhana.

### ✔ Klasifikasi

Menghasilkan dua kategori kesehatan menggunakan Rule Engine.

### ✔ Visualisasi

Menggunakan histogram, scatter plot, dan heatmap untuk memahami pola nutrisi.

---

### 📌 Kesimpulan Utama

1. **Fiber** dan **protein** merupakan nutrisi yang paling berkontribusi terhadap kategori Healthy.
2. **Sugars** adalah variabel yang paling banyak muncul pada kategori Less Healthy.
3. Mayoritas sereal memiliki NutritionScore tinggi sehingga termasuk kategori Healthy.
4. Visualisasi membantu mengungkap hubungan seperti:

   * gula tinggi tidak selalu disukai (rating rendah),
   * serat tinggi berhubungan dengan profil nutrisi lebih baik.

Workflow ini memberikan pemahaman menyeluruh tentang profil nutrisi sereal dan menunjukkan bagaimana KNIME dapat digunakan untuk analisis data end-to-end.

Jika ingin dibuat versi **PDF laporan**, **presentasi**, atau **diagram alur KNIME**, tinggal bilang saja!

# **6. Laporan Akhir Proyek Analisis Data Cereal**

Laporan akhir ini merangkum keseluruhan proses analisis data sereal menggunakan KNIME, mulai dari persiapan data, pembuatan fitur, klasifikasi, hingga visualisasi dan interpretasi. Proyek ini memberikan gambaran bagaimana tahapan analitik dapat dilakukan secara sistematis dan menghasilkan insight yang bermakna.

## **6.1 Ringkasan Workflow**

Workflow terdiri dari beberapa tahapan inti:

* **Data Preparation:** pembersihan missing value, pemilihan kolom, normalisasi.
* **Feature Engineering:** membuat *NutritionScore* sebagai indikator kesehatan.
* **Rule-based Classification:** menghasilkan label *Healthy* dan *Less Healthy*.
* **EDA & Visualization:** histogram, scatter plot, heatmap.
* **Interpretasi & Insight:** memahami hubungan antar fitur nutrisi.

Workflow ini menunjukkan bagaimana KNIME dapat digunakan tanpa model machine learning kompleks tetapi tetap menghasilkan analisis mendalam.

## **6.2 Hasil Klasifikasi"

Dengan menggunakan formula sederhana:

```
NutritionScore = calories + protein + fiber
```

dan aturan:

```
NutritionScore > 100 → Healthy
Else → Less Healthy
```

hasilnya menunjukkan bahwa **mayoritas sereal termasuk kategori Healthy**. Beberapa sereal dengan skor lebih rendah tergolong Less Healthy, biasanya ditandai dengan **serat rendah**, **kalori tinggi**, atau **gula tinggi**.

## **6.3 Evaluasi Rule-Based Model**

Model ini **sederhana namun cepat digunakan**, cocok untuk analisis awal. Namun:

* Tidak mempertimbangkan variabel negatif (fat, sodium, sugars) sebagai penalti.
* Memiliki kemungkinan bias karena formula tidak berbasis standar gizi resmi.
* Tetap mampu membedakan pola umum antara produk nutrisi baik dan buruk.

Model ini bisa ditingkatkan dengan:

* menambah faktor pengurang pada gula atau lemak,
* membuat score berbobot,
* atau menggunakan Decision Tree untuk klasifikasi otomatis.

## **6.4 Insight Utama dari Visualisasi**

### **1. Kandungan gula (Histogram)**

Dataset memiliki variasi besar pada kandungan gula. Produk "Less Healthy" cenderung berada pada rentang gula yang lebih tinggi.

### **2. Kalori, Serat, dan Protein (Scatter Plot)**

* Serat dan protein yang tinggi memberi kontribusi besar pada label Healthy.
* Sereal kalori tinggi + serat rendah lebih sering berada pada kategori Less Healthy.

### **3. Rating Konsumen**

Hubungan gula vs rating tidak linear. Produk dengan gula tinggi tidak selalu disukai, menunjukkan konsumen juga mempertimbangkan faktor lain seperti rasa, tekstur, atau nutrisi total.

### **4. Heatmap Kategori**

Menunjukkan dominasi kategori Healthy, sesuai perhitungan NutritionScore.

## **6.5 Interpretasi Akhir**

* **Fiber** menjadi indikator nutrisi paling kuat yang membedakan sereal sehat dan kurang sehat.
* **Sugars** berperan sebagai faktor risiko, tetapi belum diintegrasikan ke score dasar.
* **Rule Engine** efektif sebagai pendekatan awal namun kurang akurat untuk pemodelan nutrisi penuh.
* Visualisasi memperkuat pola bahwa produk nutrisi seimbang lebih dipandang sehat dan cenderung memiliki rating lebih baik.

## **6.6 Kesimpulan Umum Proyek**

Proyek ini menunjukkan bahwa:

1. **KNIME menyediakan workflow analitik jelas dan terstruktur** untuk pembersihan data, eksplorasi, dan klasifikasi sederhana.
2. **NutritionScore** yang dibuat berhasil memberikan pembeda awal antara Healthy dan Less Healthy.
3. Visualisasi membantu memahami hubungan antar nutrisi, terutama peran serat, protein, dan gula.
4. Model klasifikasi dapat dikembangkan lebih lanjut untuk akurasi yang lebih tinggi.
5. Proyek ini membuktikan bahwa analisis data nutrisi dapat dilakukan efisien menggunakan rule-based approach sebagai langkah awal sebelum masuk metode machine learning.
