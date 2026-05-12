# Implementasi Decision Tree Menggunakan KNIME pada Dataset Heart Disease

Proyek ini merupakan implementasi algoritma Decision Tree menggunakan KNIME Analytics Platform untuk melakukan klasifikasi penyakit jantung berdasarkan dataset heart.csv yang didapatkan dari kaggle (https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction). Dataset yang digunakan berisi berbagai informasi kesehatan pasien seperti usia, tekanan darah, kolesterol, detak jantung maksimum, tipe nyeri dada, dan atribut medis lainnya yang digunakan untuk memprediksi kemungkinan seseorang memiliki penyakit jantung. Beberapa atribut pada dataset antara lain:

- Age → usia pasien
- Sex → jenis kelamin
- ChestPainType → tipe nyeri dada
- RestingBP → tekanan darah saat istirahat
- Cholesterol → kadar kolesterol
- FastingBS → gula darah puasa
- MaxHR → detak jantung maksimum
- ExerciseAngina → nyeri dada saat olahraga
- Oldpeak → penurunan segmen ST
- ST_Slope → kemiringan ST
- HeartDisease → target klasifikasi

Target utama dari proyek ini adalah membangun model klasifikasi menggunakan Decision Tree untuk memprediksi kemungkinan penyakit jantung berdasarkan data kesehatan pasien.

Pada workflow ini dilakukan beberapa tahapan pengolahan data mulai dari proses input data hingga evaluasi model klasifikasi. Algoritma Decision Tree dipilih karena mampu menghasilkan model klasifikasi dalam bentuk pohon keputusan yang mudah dipahami dan dapat menunjukkan aturan klasifikasi berdasarkan atribut data.

## Tahapan Workflow

![original image](https://cdn.mathpix.com/snip/images/KZyN1qV3y6_o932HQivEYrHz784P89_qvuFkUtJYc7o.original.fullsize.png)

### 1. CSV Reader — Membaca Dataset

Tahap pertama menggunakan node CSV Reader untuk membaca file heart.csv. Pada Proses ini:

- file dataset diimpor ke KNIME.
- struktur tabel dikenali.
- tipe data setiap kolom dibaca.
- seluruh data pasien dimasukkan ke workflow.

Dataset yang dibaca berisi data medis pasien dengan atribut numerik maupun kategorikal.
![original image](https://cdn.mathpix.com/snip/images/Wy1X9bKKD8Ov5kPmM4PE_Qc-iqTdMGCgFsnBmUAEa1k.original.fullsize.png)

Tahap ini sangat penting karena menjadi sumber data utama untuk seluruh proses berikutnya.

### 2. Rule Engine — Transformasi dan Penyesuaian Label

Setelah dataset dibaca, data diproses menggunakan node Rule Engine. Node ini digunakan untuk membuat aturan tertentu berdasarkan kondisi data. Pada proyek klasifikasi penyakit jantung, Rule Engine biasanya digunakan untuk:

- mengubah label angka menjadi teks.
- mengelompokkan kategori.
- membuat label yang lebih mudah dipahami.

Contoh Aturan:

```
$HeartDisease$ = 1 => "Punya Penyakit Jantung"
TRUE => "Tidak Punya Penyakit Jantung"
```

Hasil:

- 1 berubah menjadi "Punya Penyakit Jantung"
- 1 berubah menjadi "Tidak Punya Penyakit Jantung"

Tujuan tahap ini:

- memperjelas hasil klasifikasi.
- mempermudah visualisasi.
- membuat output lebih mudah dipahami pengguna.


### Missing Value — Menangani Data Kosong

Pada dataset kesehatan sering ditemukan data yang kosong atau hilang. Oleh karena itu digunakan node Missing Value. Node ini bertugas mendeteksi nilai kosong, lalu mengganti nilai tersebut dengan metode tertentu.

Metode yang umum digunakan:

- rata-rata (mean)
- median
- modus
- nilai default tertentu.

Tujuan tahap ini:

- mencegah error saat training model,
- menjaga kualitas data,
- meningkatkan performa klasifikasi.

Karena algoritma Decision Tree membutuhkan data yang lengkap, preprocessing ini sangat penting dilakukan.

### Column Filter — Memilih Atribut Penting

Setelah data dibersihkan, digunakan node Column Filter untuk memilih atribut yang relevan.

Tidak semua kolom harus digunakan dalam proses klasifikasi. Oleh karena itu atribut yang dianggap tidak penting dapat dihapus.

Atribut yang dipilih:

- Age
- ChestPainType
- Cholesterol
- RestingBP
- MaxHR
- ExerciseAngina
- ST_Slope

Atribut yang tidak dipilih:

- HeartDisease

Tujuan tahap ini:

- mengurangi kompleksitas model
- mempercepat proses training
- meningkatkan akurasi klasifikasi.

Dengan memilih atribut penting, Decision Tree dapat fokus mempelajari pola yang benar-benar berpengaruh terhadap penyakit jantung.

### Table Partitioner — Membagi Data Training dan Testing

Node Table Partitioner digunakan untuk membagi dataset menjadi:

- data training,
- dan data testing.

Contoh pembagian:

- 70% training
- 30% testing

Penjelasan:

**Data Training**
Digunakan untuk:

- melatih model Decision Tree
- mempelajari pola hubungan antar atribut
- membangun aturan klasifikasi

**Data Testing**
Digunakan untuk:

- menguji performa model
- mengetahui kemampuan prediksi model pada data baru.

Contoh:
Jika total data = 1000:

- 700 data → training
- 300 data → testing

Tahap ini penting agar model tidak hanya menghafal data training tetapi juga mampu melakukan generalisasi.

### Color Manager — Memberikan Warna pada Kelas Data

Node Color Manager digunakan untuk memberikan warna pada masing-masing kelas target.

Contoh:

- merah → pasien memiliki penyakit jantung
- hijau → pasien tidak memiliki penyakit jantung

Tujuan:

- memperjelas visualisasi data
- memudahkan identifikasi kelas
- membantu interpretasi hasil klasifikasi

Node ini tidak mempengaruhi perhitungan model, tetapi membantu tampilan visual pada workflow KNIME.

### Color Appender — Menambahkan Informasi Warna ke Dataset

Setelah warna dibuat pada Color Manager, node Color Appender digunakan untuk menambahkan informasi warna tersebut ke data.

Hasilnya:

- dataset memiliki atribut visual tambahan
- visualisasi pada node berikutnya menjadi lebih jelas.

Tahap ini membantu saat melakukan analisis hasil klasifikas

### Decision Tree Learner — Membangun Model Decision Tree

Node inti dari workflow ini adalah Decision Tree Learner.

Pada tahap ini:

- model Decision Tree dibangun menggunakan data training
- algoritma mempelajari pola dari atribut kesehatan pasien
- membuat aturan keputusan dalam bentuk pohon

Decision Tree bekerja dengan:

memilih atribut terbaik,
membagi data menjadi beberapa cabang,
hingga terbentuk keputusan akhir.

### Decision Tree Predictor — Melakukan Prediksi

Setelah model selesai dibuat, node Decision Tree Predictor digunakan untuk melakukan prediksi pada data testing.

Node ini menerima:

- model dari Decision Tree Learner
- data testing dari Table Partitioner

Kemudian model akan menentukan prediksi:

- memiliki penyakit jantung
- tidak memiliki penyakit jantung

Tahap ini menunjukkan kemampuan model dalam memprediksi data pasien baru.

### 10. Scorer — Evaluasi Performa Model

Tahap terakhir menggunakan node Scorer untuk mengevaluasi hasil klasifikasi.

Node ini membandingkan nilai asli (HeartDisease) dengan hasil prediksi (Prediction(HeartDisease))

Hasil evaluasi yang diperoleh:

- Accuracy
- Confusion Matrix
- Precision
- Recall

![original image](https://cdn.mathpix.com/snip/images/pDUQg-VyGSyRhcqc-LHNSAmDe-zx46MluX4AtU14Yds.original.fullsize.png)

Berdasarkan hasil pengujian, diperoleh nilai accuracy sebesar 86,6% atau dibulatkan menjadi 86%. Nilai tersebut menunjukkan bahwa algoritma Decision Tree mampu mengklasifikasikan data pasien penyakit jantung dengan tingkat ketepatan yang cukup tinggi. Dengan demikian, model klasifikasi yang dibentuk dapat dikatakan memiliki performa yang baik dalam membedakan pasien yang memiliki penyakit jantung dan yang tidak memiliki penyakit jantung berdasarkan atribut kesehatan pada dataset heart.csv.

## Kesimpulan

Berdasarkan hasil implementasi algoritma Decision Tree menggunakan KNIME pada dataset heart.csv, dapat disimpulkan bahwa teknik klasifikasi dalam penambangan data dapat digunakan untuk menganalisis dan memprediksi kemungkinan penyakit jantung berdasarkan data kesehatan pasien.

Proses klasifikasi dilakukan melalui beberapa tahapan, mulai dari pembacaan dataset, preprocessing data, penanganan missing value, pemilihan atribut, pembagian data latih dan data uji, pembentukan pohon keputusan, hingga evaluasi hasil klasifikasi menggunakan node Scorer.

Hasil pengujian menunjukkan bahwa algoritma Decision Tree mampu menghasilkan performa klasifikasi yang baik dengan nilai accuracy sebesar 86,6%. Nilai tersebut menunjukkan bahwa sebagian besar data pasien berhasil diklasifikasikan dengan benar berdasarkan atribut kesehatan yang digunakan pada dataset.

Dengan demikian, algoritma Decision Tree dapat digunakan sebagai salah satu metode klasifikasi dalam penambangan data untuk membantu analisis data kesehatan, khususnya dalam memprediksi kemungkinan penyakit jantung.

