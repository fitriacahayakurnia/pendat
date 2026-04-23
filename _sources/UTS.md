# UTS | Analisa Kesuburan Tanah

### Nama : Fitria Cahaya Kurnia

### Nim : 240411100192

### Kelas : Pendata B

## Soal UTS

A. Lakukan analisis data dengan menggunakan
K-Nearest Neighbors (KNN)
B. Lakukan Pemrosesan Data tersebut
C. Hitung Metrik Evaluasi

| **Metrik** | **Keterangan** |
| :---: | :--- |
| Accuracy | Persentase prediksi benar dari total data |
| Precision | Ketepatan prediksi kelas positif |
| Recall | Kemampuan mendeteksi seluruh kelas positif |
| F1-Score | Harmonic mean antara Precision dan Recall |

Table 1: Metrik Evaluasi Model

## Jawaban UTS

### A. Lakukan analisis data dengan menggunakan K-Nearest Neighbors (KNN)


#### 1. Excel Reader
Node Excel Reader digunakan untuk membaca dataset dari file Excel ke dalam KNIME. Pada tugas ini , dataset yang digunakan adalah data klasifikasi kesuburan tanah yang terdiri dari 2000 sampel dengan 10 fitur dan 1 label target. Node ini merupakan langkah awal untuk memasukkan data sebelum dilakukan proses analisis lebih lanjut.

Setelah saya konfigurasi path file-nya, node ini menghasilkan tabel data mentah berisi 2.000 baris dengan 12 kolom, yaitu 11 fitur dan 1 kolom label. Pada node ini,  bisa lihat bahwa dataset yang digunakan masih mempunyai missing value yang perlu ditangani.
![original image](https://cdn.mathpix.com/snip/images/pev8aGJ1wwHFGq1A1Jxvcuxf02Xz-Y9ux-XyxxG3l0s.original.fullsize.png)

#### 2. Missing Value
Node Missing Value digunakan untuk menangani data yang hilang atau kosong pada dataset. Data yang memiliki missing value dapat memengaruhi performa model sehingga perlu dilakukan imputasi. Untuk atribut numerik biasanya digunakan metode mean atau median, sedangkan untuk atribut kategorikal digunakan metode most frequent value (modus).

Dari hasil eksporasi awal, ditemukan missing value pada beberapa kolo, seperti:
- total
- P tersedia
- K tersedia
- C organik
- Tekstur tanah
- Kadar air
- Bulk density
  
![original image](https://cdn.mathpix.com/snip/images/XCHeF6QOgi429ujDWzhTeBw0fyx-YaNjOx5NQ1tyBpE.original.fullsize.png)

Setelah node ini selesai dijalankan, semua baris data sudah bersih dari nilai kosong dan siap untuk tahapan selanjutnya.

#### 3. Duplicate Row Filter
Node Duplicate Row Filter digunakan untuk menghapus data yang duplikat atau data yang sama persis. Data duplikat dapat menyebabkan bias pada proses klasifikasi karena model dapat terlalu mudah mengenali pola tertentu. Oleh karena itu, penghapusan data duplikat membantu meningkatkan kualitas data.
![original image](https://cdn.mathpix.com/snip/images/c2AfkLbAlE0CwWn2U5tsk_mum4lxflcKvWn0wcyhvxI.original.fullsize.png)

#### 4. One to Many
Node One to Many digunakan untuk mengubah data kategorikal menjadi data numerik menggunakan metode one-hot encoding. Pada dataset ini, atribut Tekstur Tanah merupakan data kategorikal sehingga harus diubah menjadi bentuk numerik agar dapat diproses oleh algoritma KNN yang hanya menerima input berupa angka.

![original image](https://cdn.mathpix.com/snip/images/WpEIUYu22zs-w2u8WGxwJWUGLGVfz42LSUuiZrtUARM.original.fullsize.png)

#### 5. Column Filter
Node Column Filter digunakan untuk memilih kolom yang akan digunakan dalam proses klasifikasi. Kolom yang tidak diperlukan seperti ID biasanya dihapus karena tidak berpengaruh terhadap hasil prediksi. Selain itu, node ini juga membantu memastikan hanya fitur yang relevan yang digunakan oleh model. Kolom yang lain, selain kolom Id tetap dipertahankan pada panel includes.

![original image](https://cdn.mathpix.com/snip/images/Kk5U0GacFGEzujywD7EwoXdXgLGKCWVhmvLXTJI84VM.original.fullsize.png)

Gambar diatas adalah output setelah kolom id berhasil di pindahkah ke panel excludes, jadi kolom tersebut tidak akan ikut di proses pada proses selanjutnya.

#### 6. Normalizer
Setelah semua fitur sudah berbentuk numerik, saya tambahkan node Normalizer untuk menyeragamkan skala semua fitur menggunakan metode Min-Max Normalization. Rumus yang digunakan:

$$
X' = \frac{X - X_{min}}{X_{max} - X_{min}}
$$

Kenapa perlu normalisasi? Karena setiap fitur punya satuan dan rentang nilai yang berbeda. Misalnya, P Tersedia nilainya bisa puluhan (ppm), sementara N Total hanya berkisar antara 0,01 sampai 0,50 (%). Kalau tidak saya samakan skalanya, fitur dengan angka besar akan terlalu mendominasi perhitungan jarak, sementara fitur yang nilainya kecil jadi kurang diperhitungkan.

Setelah normalisasi, semua nilai fitur berada di rentang 0 sampai 1, jadi pengaruh setiap fitur lebih seimbang.

![original image](https://cdn.mathpix.com/snip/images/EYQypY1K6pUacO2YNcxVhkV4gjLpwFIzbPGIO0HFyl8.original.fullsize.png)

#### 7. Table Partitioner
Sebelum saya masukkan data ke model KNN, saya membagi data terlebih dahulu menggunakan node Table Partitioner.

Konfigurasi yang saya pakai:

- First partition type: Relative (%)
- Relative size: 80
- Sampling strategy: Random

Dari total 2.000 data, hasil pembagiannya adalah:

- 1.600 data masuk ke partisi pertama sebagai data latih (training)
- 400 data masuk ke partisi kedua sebagai data uji (testing)
![original image](https://cdn.mathpix.com/snip/images/UHjzoxMVdvpqhAcckCoqPzlupQOUAWCeneD89lqDHgw.original.fullsize.png)

#### 8. K Nearest Neighbor
Node K Nearest Neighbor merupakan node utama yang digunakan untuk proses klasifikasi menggunakan algoritma KNN. Algoritma ini bekerja dengan mencari sejumlah tetangga terdekat berdasarkan nilai K tertentu, kemudian menentukan kelas berdasarkan mayoritas tetangga tersebut. Pada penelitian ini digunakan nilai K tertentu untuk mengklasifikasikan data menjadi kelas Subur dan Tidak Subur.

Konfigurasi yang saya set:

- Column with class labels: Label
- Number of neighbors (k): 3

Cara kerja KNN yang saya pahami: setiap data uji akan dicari sebanyak k data paling dekat dari data latih. Kelas yang paling banyak muncul di antara k tetangga tersebut akan menjadi hasil prediksi. Di sini saya pakai k = 3, artinya saya lihat 3 tetangga terdekat untuk menentukan apakah tanah tersebut Subur atau Tidak Subur.

![original image](https://cdn.mathpix.com/snip/images/oAiY9RYUSKR6ntGziZH0kMmeVKd0fEqzNq8enqiQxPA.original.fullsize.png)

#### 9. Scorer
Node Scorer digunakan untuk mengevaluasi hasil klasifikasi model KNN. Node ini menghasilkan berbagai metrik evaluasi seperti accuracy, precision, recall, F1-score, dan confusion matrix. Hasil dari node ini digunakan untuk menilai seberapa baik performa model dalam memprediksi kelas target.

Perlu dicatat bahwa F1-Score pada knime ditampilkan dengan nama F-measure.

![original image](https://cdn.mathpix.com/snip/images/GcWAy7iEryDBnGN7DDoPYcVy2DN_vut0upMWLWlPl4I.original.fullsize.png)

### B. Lakukan Pemrosesan Data tersebut

Pemrosesan data dilakukan untuk memastikan dataset siap digunakan dalam proses klasifikasi menggunakan algoritma K-Nearest Neighbors (KNN). Tahapan preprocessing sangat penting karena kualitas data sangat memengaruhi hasil prediksi model. Pada penelitian ini, proses pemrosesan data dilakukan melalui beberapa tahap sebagai berikut:

#### 1. Import Dataset
Tahap pertama adalah memasukkan dataset ke dalam KNIME menggunakan node Excel Reader. Dataset yang digunakan adalah dataset klasifikasi kesuburan tanah yang terdiri dari 2000 sampel data dengan 11 fitur agronomis dan 1 kolom label sebagai target klasifikasi, yaitu Subur dan Tidak Subur.

#### 2. Menangani Missing Value
Karena dataset memiliki data yang hilang (missing value), maka dilakukan penanganan menggunakan node Missing Value. Untuk atribut numerik digunakan metode mean atau median, sedangkan untuk atribut kategorikal digunakan metode most frequent value (modus). Langkah ini bertujuan agar tidak ada data kosong yang dapat mengganggu proses klasifikasi. Karena terdapat 1 data kosong, maka digunakan metode Mean Imputation.

**Contoh Perhitungan Missing Value**
Misalnya pada kolom pH Tanah terdapat data:
| Data |  Nilai |
| ---- | -----: |
| 1    |    6.5 |
| 2    |    7.0 |
| 3    | ? |
| 4    |    6.8 |
| 5    |    6.7 |

**Rumus Mean**

Rumus mean digunakan untuk menghitung nilai rata-rata dari data yang tersedia.

$$
\bar{X} = \frac{\sum X}{n}
$$

**Perhitungan Mean**

Misalnya terdapat data pH Tanah sebagai berikut: 6.5, 7.0, 6.8, dan 6.7, dengan 1 data kosong.

Maka perhitungan mean dilakukan sebagai berikut:

$$
\bar{X} = \frac{6.5 + 7.0 + 6.8 + 6.7}{4}
$$

$$
\bar{X} = \frac{27.0}{4}
$$

$$
\bar{X} = 6.75
$$

Sehingga nilai missing value digantikan dengan 6.75.
Maka nilai missing pada kolom pH Tanah diganti menjadi: 6.75


#### 3. Menghapus Data Duplikat
Selanjutnya dilakukan penghapusan data yang duplikat menggunakan node Duplicate Row Filter. Data duplikat dapat menyebabkan bias pada model karena pola yang sama muncul berulang kali, sehingga hasil klasifikasi bisa menjadi kurang akurat.

**Contoh Data Duplikat**
Misalnya terdapat data berikut:
| ID  | pH Tanah | N Total | Label       |
| --- | -------: | ------: | ----------- |
| 001 |      6.8 |    0.35 | Subur       |
| 002 |      5.2 |    0.12 | Tidak Subur |
| 003 |      6.8 |    0.35 | Subur       |

Pada contoh tersebut:
Data ID 001 dan Data ID 003 memiliki nilai atribut yang sama persis, sehingga dianggap sebagai data duplikat. Maka salah satu data harus dihapus agar model tidak bias.


#### 4. Transformasi Data Kategorikal
Pada dataset terdapat atribut Tekstur Tanah yang bertipe kategorikal. Karena algoritma KNN hanya dapat memproses data numerik, maka atribut tersebut diubah menjadi bentuk numerik menggunakan node One to Many dengan metode one-hot encoding.
**Contoh Sebelum Transformasi**
| ID | Tekstur Tanah |
| -- | ------------- |
| 1  | Lempung       |
| 2  | Pasir         |
| 3  | Liat          |
| 4  | Lempung       |
Karena masih berupa teks, data ini belum bisa diproses oleh KNN.
**Setelah Menggunakan One-Hot Encoding**
| ID | Lempung | Pasir | Liat |
| -- | ------: | ----: | ---: |
| 1  |       1 |     0 |    0 |
| 2  |       0 |     1 |    0 |
| 3  |       0 |     0 |    1 |
| 4  |       1 |     0 |    0 |
Penjelasan:
- jika data = Lempung → kolom Lempung bernilai 1
- jika data = Pasir → kolom Pasir bernilai 1
- jika data = Liat → kolom Liat bernilai 1
kolom lainnya bernilai 0.



#### 5. Pemilihan Kolom
Pada tahap pemilihan kolom digunakan node Column Filter untuk memilih atribut yang relevan dalam proses klasifikasi. Kolom yang tidak memiliki pengaruh terhadap prediksi, seperti ID, dihapus karena hanya berfungsi sebagai identitas data. Hanya fitur yang berkaitan dengan kesuburan tanah seperti pH Tanah, N Total, P Tersedia, K Tersedia, dan atribut lainnya yang digunakan sebagai input model. Langkah ini bertujuan agar proses klasifikasi lebih fokus, efisien, dan menghasilkan performa yang lebih baik.
**Contoh Sebelum Column Filter**
| ID  | pH Tanah | N Total | P Tersedia | Label       |
| --- | -------: | ------: | ---------: | ----------- |
| 001 |      6.8 |    0.35 |         25 | Subur       |
| 002 |      5.2 |    0.12 |          8 | Tidak Subur |

Pada tabel tersebut, kolom ID tidak memiliki pengaruh terhadap tingkat kesuburan tanah karena hanya berfungsi sebagai nomor identitas data. Oleh karena itu, kolom ID harus dihapus.


#### 6. Normalisasi Data

Tahap berikutnya adalah normalisasi menggunakan node Normalizer. Hal ini dilakukan karena KNN menggunakan perhitungan jarak antar data. Jika skala antar fitur berbeda terlalu jauh, maka hasil klasifikasi dapat menjadi bias. Oleh karena itu, semua fitur disamakan skalanya agar proses perhitungan jarak menjadi lebih adil.

**Rumus Normalisasi (Min-Max Normalization)**

Rumus normalisasi digunakan untuk menyamakan skala seluruh fitur agar perhitungan jarak pada algoritma KNN menjadi lebih adil dan akurat.

$$
X' = \frac{X - X_{min}}{X_{max} - X_{min}}
$$

Keterangan:

- $X'$ = nilai hasil normalisasi  
- $X$ = nilai asli data  
- $X_{min}$ = nilai minimum pada kolom  
- $X_{max}$ = nilai maksimum pada kolom  

**Contoh Perhitungan**

Misalnya pada kolom pH Tanah:

- Nilai asli ($X$) = 6.8  
- Nilai minimum ($X_{min}$) = 4.0  
- Nilai maksimum ($X_{max}$) = 8.0  

Maka perhitungannya adalah:

$$
X' = \frac{6.8 - 4.0}{8.0 - 4.0}
$$

$$
X' = \frac{2.8}{4.0}
$$

$$
X' = 0.70
$$

Jadi hasil normalisasi dari nilai pH Tanah 6.8 adalah **0.70**.

#### 7. Pembagian Data Training dan Testing

Dataset kemudian dibagi menjadi data training dan data testing menggunakan node Table Partitioner. Data training digunakan untuk melatih model KNN, sedangkan data testing digunakan untuk menguji performa model. Umumnya pembagian dilakukan dengan rasio 80% untuk training dan 20% untuk testing.

**Contoh perhitungan**
Jumlah total dataset:

- 2000 data

Rasio pembagian yang digunakan:

- 80% Training
- 20% Testing

**Perhitungan Data Training**

Jumlah data training dihitung berdasarkan 80\% dari total dataset sebanyak 2000 data.

$$
Training = 80\% \times 2000
$$

$$
Training = 0.8 \times 2000
$$

$$
Training = 1600
$$

Jadi jumlah data training adalah **1600 data**.

---

**Perhitungan Data Testing**

Jumlah data testing dihitung berdasarkan 20\% dari total dataset sebanyak 2000 data.

$$
Testing = 20\% \times 2000
$$

$$
Testing = 0.2 \times 2000
$$

$$
Testing = 400
$$

Jadi jumlah data testing adalah **400 data**.

#### 8. Data Siap Digunakan

Setelah seluruh tahapan preprocessing selesai dilakukan, dataset telah siap digunakan untuk proses klasifikasi menggunakan algoritma K-Nearest Neighbors (KNN). Tahapan seperti penanganan missing value, penghapusan data duplikat, transformasi data kategorikal, pemilihan kolom, normalisasi, dan pembagian data training serta testing telah dilakukan dengan baik. Dataset yang bersih, lengkap, dan terstruktur akan membantu model KNN menghasilkan prediksi yang lebih akurat dan performa evaluasi yang lebih optimal.

### C. Hitung Metrik Evaluasi

Setelah proses klasifikasi menggunakan algoritma K-Nearest Neighbors (KNN) selesai dilakukan, langkah berikutnya adalah menghitung metrik evaluasi untuk mengetahui seberapa baik performa model dalam mengklasifikasikan data kesuburan tanah. Evaluasi dilakukan menggunakan node Scorer pada KNIME.

Metrik evaluasi yang digunakan meliputi Accuracy, Precision, Recall, dan F1-Score.

1. Accuracy
Accuracy digunakan untuk mengukur persentase prediksi yang benar dari seluruh data yang diuji. Semakin tinggi nilai accuracy, maka semakin baik performa model dalam melakukan klasifikasi.

Rumus Accuracy:

$$
Accuracy = \frac{TP + TN}{TP + TN + FP + FN}
$$

Keterangan:

TP (True Positive): data positif yang diprediksi benar
TN (True Negative): data negatif yang diprediksi benar
FP (False Positive): data negatif tetapi diprediksi positif
FN (False Negative): data positif tetapi diprediksi negatif

Pada penelitian ini diperoleh nilai accuracy sebesar 100%, yang menunjukkan bahwa seluruh data testing berhasil diklasifikasikan dengan benar oleh model KNN.

2. Precision
Precision digunakan untuk mengukur ketepatan model dalam memprediksi kelas positif. Precision menunjukkan seberapa banyak prediksi positif yang benar-benar positif.

Rumus Precision:

$$
Precision = \frac{TP}{TP + FP}
$$

Semakin tinggi nilai precision, maka semakin kecil kemungkinan model memberikan prediksi positif yang salah.

3. Recall

Recall digunakan untuk mengukur kemampuan model dalam menemukan seluruh data positif yang sebenarnya ada dalam dataset.

Rumus Recall:

$$
Recall = \frac{TP}{TP + FN}
$$

Semakin tinggi nilai recall, maka semakin baik model dalam mendeteksi seluruh kelas positif.

4. F1-Score

F1-Score merupakan harmonic mean antara Precision dan Recall. Nilai ini digunakan untuk menyeimbangkan precision dan recall, terutama ketika keduanya memiliki nilai yang berbeda.

Rumus F1-Score:

$$
F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}
$$

Semakin tinggi nilai F1-Score, maka semakin baik keseimbangan performa model.

**Tabel Hasil Evaluasi**

| **Metrik** | **Nilai** |
| :---: | :---: |
| Accuracy | 1.00 (100\%) |
| Precision | 1.00 (100\%) |
| Recall | 1.00 (100\%) |
| F1-Score (F-measure) | 1.00 (100\%) |

Table 2: Hasil Metrik Evaluasi

![original image](https://cdn.mathpix.com/snip/images/U1UkgZZaHYv1O_BnsWXmD60PGv5UMs__PhTVm7R5izo.original.fullsize.png)
