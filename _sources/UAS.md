# UAS | Analisis Dataset Higher Education Students Performance Evaluation

### Nama : Fitria Cahaya Kurnia

### NIM : 240411100192

### Kelas : Pendata B


---

## A. Penjelasan Dataset yang Digunakan

Dataset yang digunakan dalam UAS ini adalah *Higher Education Students Performance Evaluation* yang diperoleh dari UCI Machine Learning Repository (https://archive.ics.uci.edu/dataset/856/higher+education+students+performance+evaluation). Dataset ini berisi data 145 mahasiswa dengan 33 atribut yang mencakup faktor pribadi, kondisi keluarga, kebiasaan belajar, dan aktivitas akademik mahasiswa. Variabel target pada dataset ini adalah **GRADE**, yaitu nilai akhir mahasiswa yang digunakan untuk mengukur performa akademik. Dataset ini digunakan untuk menganalisis faktor-faktor yang mempengaruhi keberhasilan akademik mahasiswa menggunakan teknik Data Mining.

## B. Analisis Dataset Menggunakan KNIME

![](https://cdn.mathpix.com/snip/images/0yry3-OkLwGwRp1hoM4Omc4O_KAnKQDS6ctp7UVIix4.original.fullsize.png)

### 1. Data Understanding

Pada tahap ini saya menggunakan node **CSV Reader** untuk membaca dataset dan memahami struktur data yang akan dianalisis. Melalui node ini saya dapat melihat jumlah data, jumlah atribut, nama kolom, serta tipe data yang digunakan.

Berdasarkan hasil pengamatan, dataset terdiri dari 145 data mahasiswa dengan 33 atribut. Data yang tersedia mencakup berbagai faktor yang diduga mempengaruhi performa akademik mahasiswa, seperti pendidikan orang tua, jam belajar, kehadiran di kelas, aktivitas diskusi, serta nilai tugas dan proyek.

![original image](https://cdn.mathpix.com/snip/images/83t6BC06W4qzPtqUSrX7KMkQnLJ9aB03Kc5EzxqsYk0.original.fullsize.png)

Tahap ini dilakukan untuk memperoleh pemahaman awal mengenai dataset sebelum memasuki proses analisis yang lebih lanjut.

### 2. Data Cleaning

Pada tahap Data Cleaning saya menggunakan node **Number To String** dan **Domain Calculator**.

Node **Number To String** digunakan untuk mengubah atribut **GRADE** dari tipe numerik menjadi tipe kategorikal (string). Langkah ini diperlukan karena algoritma Random Forest yang digunakan merupakan metode klasifikasi sehingga target harus berupa kategori.

Selanjutnya saya menggunakan node **Domain Calculator** untuk membentuk domain atau daftar nilai yang mungkin muncul pada setiap atribut. Proses ini membantu KNIME mengenali kategori yang tersedia sehingga data dapat diproses dengan baik oleh model klasifikasi.

Berdasarkan hasil pemeriksaan, dataset tidak menunjukkan adanya masalah yang menghambat proses analisis sehingga data dapat langsung digunakan untuk tahap berikutnya.

### 3. Exploratory Data Analysis (EDA)

Pada tahap EDA saya menggunakan node **Statistics** dan **Statistics View** untuk memperoleh gambaran umum mengenai karakteristik dataset.

Node Statistics menghasilkan berbagai informasi statistik deskriptif seperti nilai minimum, maksimum, rata-rata, dan standar deviasi dari setiap atribut. Informasi ini digunakan untuk memahami penyebaran data dan melihat variasi yang terdapat pada masing-masing variabel.

Melalui Statistics View, saya dapat melihat ringkasan statistik dalam bentuk yang lebih mudah dibaca dan dianalisis. Hasil analisis menunjukkan bahwa setiap atribut memiliki karakteristik yang berbeda-beda, yang mengindikasikan adanya variasi kondisi antar mahasiswa dalam dataset.

Tahap EDA membantu saya memahami pola data sebelum dilakukan proses pemodelan menggunakan algoritma machine learning.

### 4. Data Mining Menggunakan Random Forest

Pada tahap Data Mining saya menggunakan algoritma **Random Forest** yang terdiri dari node **Table Partitioner**, **Random Forest Learner**, dan **Random Forest Predictor**.

Node **Table Partitioner** digunakan untuk membagi dataset menjadi data training dan data testing. Data training digunakan untuk melatih model sebanyak 70%, sedangkan data testing digunakan sebanyak 30% untuk menguji kemampuan model dalam melakukan prediksi terhadap data yang belum pernah dipelajari sebelumnya.

![original image](https://cdn.mathpix.com/snip/images/5FpmAhp_Jxx2aKrfOH59ZAnei7Rd4vu_Red4oL5db3c.original.fullsize.png)

Selanjutnya, node **Random Forest Learner** digunakan untuk membangun model klasifikasi. Algoritma Random Forest bekerja dengan membentuk banyak pohon keputusan (*decision tree*) dan menggabungkan hasil dari setiap pohon untuk menghasilkan prediksi yang lebih stabil. Berdasarkan hasil pemodelan, model Random Forest yang dibangun terdiri dari 100 pohon keputusan.

![original image](https://cdn.mathpix.com/snip/images/U4YfREv9NrBN95ASAiUZz0LNlBrDoTYi7CzIYqk8SKY.original.fullsize.png)

Setelah model berhasil dibentuk, node **Random Forest Predictor** digunakan untuk melakukan prediksi terhadap data testing. Hasil prediksi kemudian dibandingkan dengan nilai sebenarnya pada tahap evaluasi model.

### 5. Evaluasi Model

Evaluasi model dilakukan menggunakan node **Scorer**. Node ini digunakan untuk membandingkan hasil prediksi model dengan data aktual sehingga dapat diketahui tingkat performa model.

![original image](https://cdn.mathpix.com/snip/images/-uoPriCygNsN9R-5fa1w99YqN_Cu6CCTuwaL5pIAf3o.original.fullsize.png)

Berdasarkan hasil pengujian, diperoleh nilai **Accuracy sebesar 0,273 atau 27,3%**. Nilai ini menunjukkan bahwa model mampu memprediksi dengan benar sekitar 27,3% dari seluruh data testing yang digunakan.

Selain itu, diperoleh nilai **Cohen's Kappa sebesar 0,109**. Nilai tersebut menunjukkan bahwa tingkat kesesuaian antara hasil prediksi model dan data aktual masih tergolong rendah. Hal ini menunjukkan bahwa model masih mengalami kesulitan dalam menemukan pola yang kuat pada dataset.

Rendahnya nilai akurasi dapat disebabkan oleh jumlah data yang relatif sedikit, yaitu hanya 145 data mahasiswa, serta banyaknya kategori nilai (GRADE) yang harus diprediksi oleh model.

## C. Kesimpulan

Berdasarkan analisis yang telah saya lakukan menggunakan KNIME, algoritma Random Forest berhasil digunakan untuk melakukan klasifikasi nilai mahasiswa berdasarkan berbagai faktor yang terdapat dalam dataset. Model yang dibangun terdiri dari 100 pohon keputusan dan menghasilkan nilai accuracy sebesar 27,3% serta Cohen's Kappa sebesar 0,109.

Hasil tersebut menunjukkan bahwa model masih memiliki kemampuan prediksi yang rendah terhadap performa akademik mahasiswa. Meskipun demikian, proses analisis ini menunjukkan bagaimana teknik Data Mining dapat digunakan untuk mempelajari hubungan antara karakteristik mahasiswa dan hasil akademiknya melalui pendekatan machine learning.

