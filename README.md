# 🛒 Customer Segmentation Analysis for Olist-E-Commerce

## Creator

1. **Annalia Alfia Rahma** – fiaannalia2833@gmail.com  
2. **M Narendra Atma Ghifari** – mnaghifari@gmail.com  
3. **Muhammad Rizky Indrawan** – mochrizkyindrawan99@gmail.com

##  Business Problem

### 📍 Background

Olist adalah platform e-commerce berbasis SaaS yang didirikan pada tahun 2014 di Curitiba, Brazil. Platform ini membantu pelaku bisnis, khususnya UMKM di sektor retail, untuk memasarkan produknya ke berbagai marketplace besar melalui satu dashboard terintegrasi.

Melalui Olist, penjual dapat:

* Mendaftarkan produk dengan mudah
* Menjangkau lebih banyak pelanggan secara otomatis
* Mengakses layanan logistik, keuangan, dan promosi yang mendukung kegiatan bisnis mereka

Pendapatan Olist diperoleh dari:

* **Biaya langganan** (mulai dari R\$45.90/bulan)
* **Komisi transaksi** sebesar 10% dari setiap penjualan
* **Layanan tambahan**, seperti logistik, iklan, kredit usaha, dan monetisasi data

### ❗ Problem Statement

Olist menghadapi tantangan dalam mengalokasikan layanan tambahan (seperti promosi, logistik, atau kredit usaha) secara efisien. Jika seluruh penjual diperlakukan secara setara tanpa mempertimbangkan kinerja mereka, maka alokasi sumber daya akan menjadi tidak optimal.

Untuk itu, Olist perlu:

* Mengidentifikasi penjual yang aktif, potensial, dan berkontribusi besar
* Menghindari pemborosan biaya promosi/logistik pada seller yang pasif
* Memfokuskan layanan strategis kepada segmen yang paling menguntungkan

Dengan pendekatan berbasis **machine learning dan segmentasi seller**, perusahaan dapat meningkatkan efisiensi operasional, kualitas layanan, serta pertumbuhan pendapatan.

---

## Stakeholders

| Stakeholder               | Kepentingan                                                                             |
| ------------------------- | --------------------------------------------------------------------------------------- |
| **Olist (Company)**       | Meningkatkan efisiensi platform dan pendapatan perusahaan melalui segmentasi yang tepat |
| **Seller (Penjual/UMKM)** | Mendapatkan dukungan layanan tambahan agar dapat berkembang di platform                 |
| **Buyer (Pembeli)**       | Mendapatkan pengalaman belanja terbaik dari penjual berkinerja tinggi di Olist          |

---

## Goals

* Meningkatkan efisiensi operasional dan peningkatan pendapatan perusahaan dengan mengidentifikasi penjual yang berpotensi.
* Memberikan peluang yang lebih besar bagi penjual yang berpotensi untuk mendapatkan dukungan layanan tambahan seperti promosi, logistik, dan fasilitas finansial yang lebih lanjut
* Mendorong pertumbuhan volume transaksi dan kualitas layanan dari para penjual untuk secara langsung berkontribusi pada peningkatan kinerja finansial dan keberhasilan bisnis perushaan

---

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah **Olist Brazilian E-commerce Dataset**, tersedia di [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce). Dataset ini berisi data transaksi e-commerce dari berbagai marketplace besar di Brasil (seperti Mercado Livre, B2W, dan lainnya), yang difasilitasi oleh platform Olist. Dataset mencakup transaksi dari tahun 2016 hingga 2018 dan terdiri dari 9 tabel utama dengan 115.723 baris dan 52 kolom.

Fokus utama dari proyek ini adalah melakukan segmentasi terhadap penjual (`seller`), sehingga dilakukan seleksi dan join data untuk menghasilkan satu dataset. Kolom-kolom yang digunakan dalam analisis ini meliputi: order_id, order_item_id, order_status, order_purchase_timestamp, order_approved_at, order_delivered_carrier_date, order_delivered_customer_date, order_estimated_delivery_date, customer_id, customer_unique_id, seller_id, seller_zip_code_prefix, seller_city, seller_state, product_id, shipping_limit_date, price, freight_value, payment_value, dan review_score. Kolom-kolom tersebut digunakan untuk memahami perilaku transaksi, performa penjual, serta kualitas layanan dalam platform Olist.

Dataset yang digunakan untuk proses clustering merupakan hasil dari pengolahan dan penggabungan beberapa tabel dari dataset Olist. Dataset ini difokuskan pada performa masing-masing penjual (seller_id) dan terdiri dari variabel-variabel yang mencerminkan dimensi LRFM (Length, Recency, Frequency, Monetary), serta metrik tambahan lain untuk menilai kualitas seller. Berikut adalah fitur-fitur dalam dataset clustering:
| **Nama Kolom** | **Deskripsi**                                                                |
| -------------- | ---------------------------------------------------------------------------- |
| `seller_id`    | ID unik dari penjual                                                         |
| `length`       | Lama penjual aktif di platform Olist (dalam bulan)                           |
| `recency`      | Waktu sejak terakhir transaksi dilakukan (dalam bulan)                       |
| `frequency`    | Rata-rata jumlah transaksi per bulan                                         |
| `monetary`     | Rata-rata nilai pembayaran per bulan (dalam BRL)                             |
| `review_score` | Rata-rata nilai ulasan yang diberikan oleh pelanggan                         |
| `time_process` | Rata-rata waktu yang dibutuhkan penjual untuk memproses dan mengirim pesanan |


## Exploratory Data Analysis (EDA)

Untuk memahami kontribusi revenue perusahaan secara menyeluruh dan menentukan arah analisa yang paling relevan, langkah awal yang dilakukan adalah proses eksplorasi data awal (EDA). Analisis ini bertujuan untuk mengidentifikasi karakteristik transaksi, distribusi order, dan potensi sumber pendapatan utama yang dimiliki Olist. Seluruh data transaksi terlebih dahulu difilter untuk hanya menyertakan transaksi dengan status `delivered`, karena hanya transaksi yang berhasil terkirim yang benar-benar memberikan kontribusi terhadap pendapatan aktual perusahaan.

### 1. Data Preparation & Cleaning

* Penggabungan data dilakukan dari beberapa tabel utama seperti `order_items`, `orders`, `order_reviews`, `payments`, `sellers`, dan `customers`.
* Data dibersihkan dari nilai duplikat dan missing value.
* Kolom bertipe waktu seperti `order_purchase_timestamp`, `order_approved_at`, dan lainnya dikonversi ke format datetime.
* Fitur baru `processing_time_days` ditambahkan untuk mengukur durasi proses pemesanan hingga pengiriman.

### 2. Revenue Analysis

#### 2.1 Transaction Fee Revenue
<img width="1189" height="490" alt="image" src="https://github.com/user-attachments/assets/b73c1f01-92aa-43ea-9be1-c50822de9e42" />

* Olist memperoleh pendapatan sebesar **10%** dari setiap transaksi yang berhasil.
* Total pendapatan dari komisi transaksi mencapai **R\$1.93 juta** selama periode 2016–2018.
* Lonjakan terbesar terjadi pada bulan **November 2017**, bertepatan dengan event **Black Friday**.
* **Insight**: Pendapatan dari transaksi menunjukkan pertumbuhan positif, namun sangat bergantung pada musim promo.

#### 2.2 Subscription Fee Revenue
<img width="1189" height="490" alt="image" src="https://github.com/user-attachments/assets/3cfbe80a-edb0-4d54-8411-335a282d4f96" />

* Olist mengenakan biaya berlangganan minimum sebesar **R\$45.90/bulan** untuk setiap penjual aktif.
* Total pendapatan dari biaya langganan selama periode analisis mencapai **R\$732.289**.
* Jumlah seller aktif meningkat signifikan dari **219** menjadi **1.248** dalam kurun waktu 1,5 tahun.
* **Insight**: Pendapatan dari subscription lebih stabil dan bertumbuh konsisten setiap bulan.

#### 2.3 Total Revenue Comparison
<img width="1389" height="590" alt="image" src="https://github.com/user-attachments/assets/096fb11c-0b3f-48b9-834f-c3cb66d64e28" />

* Total revenue dari pendekatan Buyer Focus (hanya komisi transaksi): **R\$1.93M**.
* Total revenue dari pendekatan Seller Focus (komisi + langganan): **R\$2.66M**.
* **Insight**: Seller Focus menghasilkan pendapatan **38% lebih tinggi** dibanding Buyer Focus.

### 3. Buyer Analysis
<img width="784" height="527" alt="image" src="https://github.com/user-attachments/assets/d0cd198e-19d3-4154-96be-3c53c9a357a1" />

* Terdapat **91.449** unique buyers, namun **85%** hanya melakukan **1 kali transaksi**.
* Hanya **15%** buyer yang melakukan **repeat order**.
* Tidak ada pola loyalitas atau buyer dominan yang signifikan terhadap pendapatan.
* **Insight**:

  * Buyer di platform cenderung bersifat **sporadis dan tidak loyal**.
  * **Peluang revenue dari buyer terbatas** untuk pertumbuhan jangka panjang.

---


## Clustering Seller dengan KMeans

**Tujuan:** Mengelompokkan seller Olist berdasarkan karakteristik perilaku mereka menggunakan pendekatan **LRFM (Length, Recency, Frequency, Monetary)** untuk mendukung strategi promosi yang lebih tepat sasaran.

### Definisi LRFM

* **Length**: Lama seller aktif di platform (bulan)
* **Recency**: Waktu sejak transaksi terakhir (bulan)
* **Frequency**: Jumlah transaksi
* **Monetary**: Total nilai transaksi

###  Preprocessing & Scaling

* `monetary` → log-transform + StandardScaler (atasi skew)
* `length`, `recency`, `frequency`, `review`, `processing_time` → RobustScaler (atasi outlier)

### Menentukan Jumlah Cluster

* **Elbow Method** & **Silhouette Score** → optimal pada **4 cluster**

### KMeans Clustering (k=4)
<img width="748" height="612" alt="{1A4E9EC3-6676-4925-B711-E9DFBF3F9216}" src="https://github.com/user-attachments/assets/c1ccf9f3-3444-45fb-ad80-099811211053" />


Model KMeans diterapkan untuk membagi seller menjadi 4 segmen:

| Cluster               | Persentase Seller     | Aktivitas Order                     | Length Aktif | Pendapatan Total | Review & Proses                      |
| --------------------- | --------------------- | ----------------------------------- | ------------ | ---------------- | ------------------------------------ |
| **Dormant Seller**    | ±3,3% (98 seller)     | 1 order, tidak aktif 12 bulan       | 1 bulan      | ±252 BRL         | Review **3/5**, proses **14,6 hari** |
| **Potensial Seller**  | ±6,3% (187 seller)    | 14 order/bulan, aktif konsisten     | 11 bulan     | ±20.500 BRL      | Review **5/5**, proses **1,7 hari**  |
| **Regular Seller**    | ±89,7% (2.653 seller) | 1 order/bulan, pasif                | 5 bulan      | ±1.138 BRL       | Review **5/5**, proses **1,72 hari** |
| **High-Value Seller** | ±0,6% (18 seller)     | 65 order/bulan, aktif terus-menerus | 15 bulan     | ±150.000 BRL     | Review **5/5**, proses **2,01 hari** |



## 🔎 Performance Analysis

1. **Total Orders**: High-Value Seller memiliki aktivitas transaksi tertinggi, Dormant dan Regular Seller hanya mencatat 1 order secara median.

2. **Delivery Time**: Potential dan Regular Seller paling cepat memproses pesanan (\~1,8 hari); Dormant Seller paling lambat (14 hari).

3. **Review Score**: Semua cluster kecuali Dormant mendapatkan median skor review 5; Dormant hanya mendapat skor 3.

4. **Top Seller State**: São Paulo (SP) mendominasi semua cluster; peluang pengembangan besar di PR, MG, dan RJ.

5. **Top Product by Quantity & Revenue**: High-Value Seller fokus pada produk bernilai tinggi, Potential Seller seimbang antara volume dan nilai.

6. **Monthly Revenue**: High-Value Seller mencatat pertumbuhan tertinggi (11,17%); Dormant Seller paling rendah (2,37%).

7. **Monthly Orders**: High-Value Seller konsisten tertinggi dan responsif terhadap event besar; Dormant Seller sangat fluktuatif.

8. **Black Friday Products**: Seller aktif menjual produk musiman bernilai tinggi; Dormant Seller cenderung sporadis.

9. **Review Score Trend**: Regular Seller paling konsisten menjaga kepuasan pelanggan; High-Value sempat turun saat lonjakan pesanan.

---

## 🎯 Strategy

### 1. Re-engagement Campaign (Regular & Dormant Seller)

Aktivasi ulang seller pasif melalui email personal (“Kami Merindukan Anda”) dengan highlight fitur baru, manfaat bergabung, dan permintaan feedback.

**Impact**:

* Target: 10% dari 2.659 seller
* Tambahan pendapatan seller: \~368.284 BRL
* Potensi pendapatan Olist: **\~49.038 BRL/bulan**


### 2. Diskon Upgrade Paket Langganan

#### a. Potential Seller → Paket Crescer

Diskon 30% (110 → 77 BRL) selama 3 bulan untuk dorong adopsi fitur lanjutan.

* Target adopsi: 60% (112 seller)
* Tambahan revenue seller: \~344.616 BRL
* Potensi pendapatan Olist: **\~41.641 BRL/bulan**

#### b. High-Value Seller → Paket Evoluir

Diskon 25% (240 → 180 BRL) selama 6 bulan sebagai strategi lock-in.

* Target adopsi: 80% (15 seller)
* Tambahan revenue seller: \~113.417 BRL
* Potensi pendapatan Olist: **\~14.253 BRL/bulan**


### 3. Onboarding untuk Regular Seller

Pelatihan awal dan dukungan fitur untuk tingkatkan performa seller pemula.

* Target adopsi: 60% (1.595 seller)
* Tambahan revenue seller: \~883.770 BRL
* Potensi pendapatan Olist (net): **\~43.797 BRL/bulan**


---

## 📌 Summary

**1. Seller adalah Kunci Pertumbuhan Olist**
Seller menjadi sumber pendapatan utama dan stabil melalui transaksi rutin serta subscription fee. Berbeda dengan buyer yang kurang loyal, fokus pada seller membuka peluang optimalisasi jangka panjang dan peningkatan efisiensi.

**2. Clustering Menghasilkan 4 Tipe Seller**
Hasil segmentasi KMeans membagi seller menjadi:

* **Dormant Seller**: Tidak aktif, pendapatan rendah, proses lambat.
* **Potential Seller**: Aktif & stabil, berpotensi naik kelas.
* **Regular Seller**: Jumlah terbesar, performa rendah tapi punya potensi.
* **High-Value Seller**: Jumlah sedikit, kontribusi revenue sangat besar.

**3. Revenue Berbeda Signifikan antar Cluster**

* **High-Value Seller**: Rata-rata pendapatan 9.677 BRL/bulan, pertumbuhan 11,17%.
* **Potential Seller**: 1.822 BRL/bulan, pertumbuhan 7,42%.
* **Regular Seller**: 321 BRL/bulan, pertumbuhan 2,81%.
* **Dormant Seller**: 448 BRL/bulan, pertumbuhan 2,37%.


---

## ✅ Recommendation

**1. Re-engagement Campaign untuk Regular & Dormant Seller**
Kampanye email personal bertema “Kami Merindukan Anda” ditujukan untuk mengaktifkan kembali seller pasif, dengan target reaktivasi 10% dan peningkatan pendapatan 25% per seller.
*Potensi pendapatan tambahan: ±R\$49.037/bulan.*

**2. Diskon Upgrade Paket Crescer untuk Potential Seller**
Diskon 30% selama 3 bulan untuk mendorong seller mengadopsi fitur lanjutan seperti manajemen retur & integrasi otomatis. Target adopsi 60% dengan kenaikan pendapatan 15%.
*Potensi pendapatan tambahan: ±R\$41.640/bulan.*

**3. Diskon Upgrade Paket Evoluir untuk High-Value Seller**
Diskon 25% selama 6 bulan sebagai strategi loyalitas dan lock-in, dengan fitur premium seperti otomatisasi dan analisis margin. Target adopsi 80% dengan kenaikan pendapatan 5%.
*Potensi pendapatan tambahan: ±R\$14.253/bulan.*


## 📊 Tableau Dashboard

Lihat visualisasi interaktif dari hasil segmentasi seller Olist pada dashboard berikut:

🔗 [Olist Seller Cluster Analytics – Tableau Public](https://public.tableau.com/app/profile/rizky.muhammad.indrawan/viz/OlistSellerClusterAnalytics/Dashboard1?publish=yes)

<img width="1286" height="890" alt="{E6F24E94-B9DA-47AA-BB0C-6E223BA1BF72}" src="https://github.com/user-attachments/assets/642adf76-3c0f-44fe-b504-47aceda5fce3" />
<img width="1286" height="885" alt="{87C98C25-7BEC-4BDD-A12C-30AE0E37E478}" src="https://github.com/user-attachments/assets/b5c34efb-5105-44d0-bacd-1af86503c5af" />
<img width="1285" height="882" alt="{99CDEB9B-B55E-4175-9117-2A5741816852}" src="https://github.com/user-attachments/assets/9cdd72ab-7f2a-448c-baca-4eb8434a6b1d" />

---

## 📊 Streamlit

Website ini disusun guna memvisualisasikan hasil segmentasi seller berdasarkan nilai clustering yang dimasukkan.

🔗 [Klik di sini untuk membuka aplikasi Streamlit](https://olistsellerclustering.streamlit.app/)

<img width="1906" height="899" alt="{B40CC304-2253-48E7-8476-83E267DF0051}" src="https://github.com/user-attachments/assets/2de8842c-e4ff-460a-9fa9-45c5d22ad60c" />
<img width="1894" height="892" alt="{2AB9FF21-257A-493C-AA8C-281D0121B6D8}" src="https://github.com/user-attachments/assets/f6855e0c-6553-4bb7-b74a-ef4e51dcc15a" />



