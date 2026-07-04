# UAS | Analisis Dataset Higher Education Students Performance Evaluation

### Nama : Fitria Cahaya Kurnia

### NIM : 240411100192

### Kelas : Pendata B


---

# A. Penjelasan Dataset yang Digunakan

Dataset yang digunakan dalam UAS ini adalah **Higher Education Students Performance Evaluation** yang diperoleh dari **UCI Machine Learning Repository** (https://archive.ics.uci.edu/dataset/856/higher+education+students+performance+evaluation). Dataset ini berisi data **145 mahasiswa** dengan **33 atribut** yang mencakup faktor pribadi, kondisi keluarga, kebiasaan belajar, dan aktivitas akademik mahasiswa. Variabel target pada dataset ini adalah **GRADE**, yaitu nilai akhir mahasiswa yang digunakan untuk mengukur performa akademik. Dataset ini digunakan untuk menganalisis faktor-faktor yang mempengaruhi keberhasilan akademik mahasiswa menggunakan teknik Data Mining.

---

# B. Analisis Dataset Menggunakan KNIME

![](https://cdn.mathpix.com/snip/images/0yry3-OkLwGwRp1hoM4Omc4O_KAnKQDS6ctp7UVIix4.original.fullsize.png)

## 1. Business Understanding

Pada tahap **Business Understanding**, tujuan utama analisis adalah mengetahui faktor-faktor yang mempengaruhi performa akademik mahasiswa berdasarkan data yang tersedia. Informasi tersebut diharapkan dapat membantu pihak perguruan tinggi dalam memahami karakteristik mahasiswa yang memperoleh nilai akademik tinggi maupun rendah sehingga dapat dijadikan sebagai bahan evaluasi dalam meningkatkan kualitas pembelajaran.

Dataset yang digunakan berisi berbagai informasi mengenai mahasiswa, seperti kondisi keluarga, kebiasaan belajar, aktivitas selama perkuliahan, serta faktor-faktor lain yang diduga berpengaruh terhadap hasil akademik. Variabel yang menjadi target prediksi adalah **GRADE**, yaitu nilai akhir mahasiswa.

Permasalahan yang ingin diselesaikan adalah bagaimana membangun model klasifikasi yang mampu memprediksi kategori nilai mahasiswa berdasarkan atribut-atribut yang tersedia. Untuk menyelesaikan permasalahan tersebut digunakan algoritma **Random Forest**, karena algoritma ini mampu melakukan klasifikasi dengan menggabungkan banyak pohon keputusan (*decision tree*) sehingga hasil prediksi menjadi lebih stabil.

Melalui proses data mining ini diharapkan dapat diketahui apakah atribut-atribut yang terdapat pada dataset mampu digunakan untuk memprediksi performa akademik mahasiswa serta memberikan gambaran mengenai faktor-faktor yang berhubungan dengan nilai akhir mahasiswa.

---

## 2. Data Understanding

Pada tahap ini saya menggunakan node **CSV Reader** untuk membaca dataset dan memahami struktur data yang akan dianalisis. Melalui node ini saya dapat melihat jumlah data, jumlah atribut, nama kolom, serta tipe data yang digunakan.

Berdasarkan hasil pengamatan, dataset terdiri dari **145 data mahasiswa** dengan **33 atribut**. Data yang tersedia mencakup berbagai faktor yang diduga mempengaruhi performa akademik mahasiswa, seperti pendidikan orang tua, jam belajar, kehadiran di kelas, aktivitas diskusi, serta nilai tugas dan proyek.

![](https://cdn.mathpix.com/snip/images/83t6BC06W4qzPtqUSrX7KMkQnLJ9aB03Kc5EzxqsYk0.original.fullsize.png)

Tahap ini dilakukan untuk memperoleh pemahaman awal mengenai dataset sebelum memasuki proses analisis yang lebih lanjut.

### Analisis Data

Berdasarkan hasil pengamatan terhadap dataset, diketahui bahwa setiap atribut memiliki fungsi yang berbeda dalam menggambarkan karakteristik mahasiswa. Variabel-variabel tersebut mencerminkan berbagai aspek yang dapat mempengaruhi performa akademik, mulai dari kondisi keluarga, kebiasaan belajar, hingga aktivitas mahasiswa selama mengikuti perkuliahan.

Jumlah atribut yang cukup banyak dibandingkan jumlah data menunjukkan bahwa model harus mempelajari hubungan yang kompleks antar variabel. Selain itu, keberagaman karakteristik mahasiswa menunjukkan bahwa dataset memiliki variasi data yang cukup baik untuk dilakukan proses klasifikasi menggunakan algoritma machine learning.

---

## 3. Data Preparation

### Data Cleaning

Pada tahap Data Cleaning saya menggunakan node **Number To String** dan **Domain Calculator**.

Node **Number To String** digunakan untuk mengubah atribut **GRADE** dari tipe numerik menjadi tipe kategorikal (string). Langkah ini diperlukan karena algoritma Random Forest yang digunakan merupakan metode klasifikasi sehingga target harus berupa kategori.

Selanjutnya saya menggunakan node **Domain Calculator** untuk membentuk domain atau daftar nilai yang mungkin muncul pada setiap atribut. Proses ini membantu KNIME mengenali kategori yang tersedia sehingga data dapat diproses dengan baik oleh model klasifikasi.

Berdasarkan hasil pemeriksaan, dataset tidak menunjukkan adanya masalah yang menghambat proses analisis sehingga data dapat langsung digunakan untuk tahap berikutnya.

### Exploratory Data Analysis (EDA)

Pada tahap EDA saya menggunakan node **Statistics** dan **Statistics View** untuk memperoleh gambaran umum mengenai karakteristik dataset.

Node Statistics menghasilkan berbagai informasi statistik deskriptif seperti nilai minimum, maksimum, rata-rata, dan standar deviasi dari setiap atribut. Informasi ini digunakan untuk memahami penyebaran data dan melihat variasi yang terdapat pada masing-masing variabel.

Melalui Statistics View, saya dapat melihat ringkasan statistik dalam bentuk yang lebih mudah dibaca dan dianalisis. Hasil analisis menunjukkan bahwa setiap atribut memiliki karakteristik yang berbeda-beda, yang mengindikasikan adanya variasi kondisi antar mahasiswa dalam dataset.

Tahap EDA membantu saya memahami pola data sebelum dilakukan proses pemodelan menggunakan algoritma machine learning.

### Analisis Hasil EDA

Berdasarkan hasil Statistics View, setiap atribut memiliki distribusi nilai yang berbeda-beda. Hal ini menunjukkan bahwa karakteristik mahasiswa dalam dataset cukup beragam. Perbedaan tersebut terlihat dari variasi kebiasaan belajar, kehadiran, aktivitas diskusi, maupun faktor-faktor lainnya.

Selain itu, hasil pemeriksaan menunjukkan bahwa dataset dapat diproses tanpa mengalami kendala yang berarti sehingga seluruh data dapat langsung digunakan pada tahap pemodelan. Variasi data yang dimiliki setiap mahasiswa diharapkan dapat membantu algoritma Random Forest dalam menemukan pola hubungan antara atribut dengan nilai akhir mahasiswa.

---

## 4. Modeling

### Data Mining Menggunakan Random Forest

Pada tahap Data Mining saya menggunakan algoritma **Random Forest** yang terdiri dari node **Table Partitioner**, **Random Forest Learner**, dan **Random Forest Predictor**.

Node **Table Partitioner** digunakan untuk membagi dataset menjadi data training dan data testing. Data training digunakan untuk melatih model sebanyak **70%**, sedangkan data testing digunakan sebanyak **30%** untuk menguji kemampuan model dalam melakukan prediksi terhadap data yang belum pernah dipelajari sebelumnya.

![](https://cdn.mathpix.com/snip/images/5FpmAhp_Jxx2aKrfOH59ZAnei7Rd4vu_Red4oL5db3c.original.fullsize.png)

Selanjutnya, node **Random Forest Learner** digunakan untuk membangun model klasifikasi. Algoritma Random Forest bekerja dengan membentuk banyak pohon keputusan (*decision tree*) dan menggabungkan hasil dari setiap pohon untuk menghasilkan prediksi yang lebih stabil. Berdasarkan hasil pemodelan, model Random Forest yang dibangun terdiri dari **100 pohon keputusan**.

![](https://cdn.mathpix.com/snip/images/U4YfREv9NrBN95ASAiUZz0LNlBrDoTYi7CzIYqk8SKY.original.fullsize.png)

Setelah model berhasil dibentuk, node **Random Forest Predictor** digunakan untuk melakukan prediksi terhadap data testing. Hasil prediksi kemudian dibandingkan dengan nilai sebenarnya pada tahap evaluasi model.

### Analisis Model

Random Forest dipilih karena mampu menangani dataset dengan jumlah atribut yang cukup banyak serta memiliki kemampuan mengurangi risiko overfitting melalui pembentukan banyak pohon keputusan. Pada penelitian ini model dibangun menggunakan **100 decision tree** sehingga setiap pohon memberikan hasil prediksi yang kemudian digabungkan melalui proses voting untuk menghasilkan prediksi akhir.

Pembagian data menjadi **70% data training** dan **30% data testing** bertujuan agar model dapat dilatih menggunakan sebagian besar data, kemudian diuji menggunakan data yang belum pernah dipelajari sebelumnya sehingga hasil evaluasi dapat menggambarkan kemampuan model dalam melakukan prediksi terhadap data baru.

---

## 5. Evaluation

### Evaluasi Model

Evaluasi model dilakukan menggunakan node **Scorer**. Node ini digunakan untuk membandingkan hasil prediksi model dengan data aktual sehingga dapat diketahui tingkat performa model.

![](https://cdn.mathpix.com/snip/images/-uoPriCygNsN9R-5fa1w99YqN_Cu6CCTuwaL5pIAf3o.original.fullsize.png)

Berdasarkan hasil pengujian, diperoleh nilai **Accuracy sebesar 0,273 atau 27,3%**. Nilai ini menunjukkan bahwa model mampu memprediksi dengan benar sekitar **27,3%** dari seluruh data testing yang digunakan.

Selain itu, diperoleh nilai **Cohen's Kappa sebesar 0,109**. Nilai tersebut menunjukkan bahwa tingkat kesesuaian antara hasil prediksi model dan data aktual masih tergolong rendah. Hal ini menunjukkan bahwa model masih mengalami kesulitan dalam menemukan pola yang kuat pada dataset.

Rendahnya nilai akurasi dapat disebabkan oleh jumlah data yang relatif sedikit, yaitu hanya **145 data mahasiswa**, serta banyaknya kategori nilai (**GRADE**) yang harus diprediksi oleh model.

### Analisis Hasil Evaluasi

Berdasarkan hasil evaluasi, model Random Forest belum mampu memberikan performa yang optimal. Nilai akurasi sebesar **27,3%** menunjukkan bahwa model masih sering melakukan kesalahan dalam mengklasifikasikan kategori nilai mahasiswa.

Nilai **Cohen's Kappa sebesar 0,109** juga menunjukkan bahwa tingkat kesepakatan antara hasil prediksi dengan data aktual masih rendah. Hal ini mengindikasikan bahwa hubungan antara atribut-atribut dalam dataset dengan nilai akhir mahasiswa belum dapat dipelajari secara optimal oleh model.

Beberapa faktor yang diduga mempengaruhi rendahnya performa model antara lain jumlah data yang relatif sedikit, banyaknya kategori pada variabel target (**GRADE**), serta kemungkinan distribusi data yang tidak seimbang pada setiap kategori nilai. Oleh karena itu, model masih mengalami kesulitan dalam menemukan pola yang kuat untuk menghasilkan prediksi yang akurat.

---

# C. Kesimpulan

Berdasarkan analisis yang telah dilakukan menggunakan KNIME, dataset **Higher Education Students Performance Evaluation** memiliki **145 data mahasiswa** dengan **33 atribut** yang menggambarkan berbagai faktor, seperti kondisi keluarga, kebiasaan belajar, aktivitas selama perkuliahan, serta faktor akademik lainnya. Hasil analisis menunjukkan bahwa setiap mahasiswa memiliki karakteristik yang beragam, sehingga memungkinkan dilakukan proses klasifikasi untuk memprediksi nilai akhir (**GRADE**).

Pada tahap **Data Preparation**, data telah dipersiapkan dengan mengubah atribut **GRADE** menjadi tipe kategorikal menggunakan **Number To String** serta membentuk domain data menggunakan **Domain Calculator**. Hasil pemeriksaan menunjukkan bahwa dataset dapat digunakan untuk proses pemodelan tanpa mengalami kendala yang menghambat analisis.

Berdasarkan hasil **Exploratory Data Analysis (EDA)**, setiap atribut memiliki karakteristik dan penyebaran nilai yang berbeda-beda. Hal ini menunjukkan adanya variasi kondisi antar mahasiswa, sehingga diperkirakan terdapat hubungan antara faktor-faktor tersebut dengan performa akademik mahasiswa.

Proses pemodelan menggunakan algoritma **Random Forest** berhasil dilakukan dengan membangun model yang terdiri dari **100 pohon keputusan**. Model kemudian dievaluasi menggunakan node **Scorer** dan menghasilkan nilai **Accuracy sebesar 27,3%** serta **Cohen's Kappa sebesar 0,109**. Hasil tersebut menunjukkan bahwa kemampuan model dalam memprediksi kategori nilai mahasiswa masih tergolong rendah.

Rendahnya performa model diduga dipengaruhi oleh jumlah data yang relatif sedikit, yaitu hanya **145 data mahasiswa**, banyaknya kategori pada variabel target (**GRADE**), serta kemungkinan distribusi data yang kurang seimbang. Kondisi tersebut menyebabkan model masih mengalami kesulitan dalam mempelajari pola hubungan antar atribut secara optimal.

Secara keseluruhan, penelitian ini menunjukkan bahwa tahapan **Business Understanding**, **Data Understanding**, **Data Preparation**, **Modeling**, dan **Evaluation** telah berhasil diterapkan menggunakan KNIME. Selain itu, hasil analisis memberikan gambaran bahwa karakteristik mahasiswa yang beragam dapat dimanfaatkan sebagai dasar dalam membangun model prediksi performa akademik, meskipun diperlukan pengembangan lebih lanjut, seperti penambahan jumlah data, seleksi fitur, atau penggunaan algoritma lain agar diperoleh tingkat akurasi yang lebih baik.

