# Data Understanding

### 1. Sumber Data

Dataset yang digunakan adalah Iris Flower Dataset yang diperoleh dari platform Kaggle. Dataset ini merupakan dataset klasik dalam bidang data mining dan machine learning yang berisi data pengukuran bunga iris dari tiga spesies berbeda, yaitu Setosa, Versicolor, dan Virginica. Dataset ini digunakan sebagai studi kasus untuk penerapan metode klasifikasi dalam penambangan data.
Berikut link dari dataset
[Dataset Iris](https://www.kaggle.com/datasets/arshid/iris-flower-dataset?resource=download)

### 2. Deskripsi Dataset

Dataset yang digunakan dalam penelitian ini adalah Iris Flower Dataset yang terdiri dari 150 data dengan 5 atribut. Dataset ini digunakan untuk mengklasifikasikan bunga iris ke dalam tiga spesies berdasarkan ukuran bagian bunganya.

### 3. Eksplorasi Dataset

Dalam proses mengidentifikasi dataset ini, python membantu untuk mempermudah pengidentifikasiannya.

#### Eksplorasi Dataset dengan menggunakan Python:

##### - Upload file CSV

```
from google.colab import files
files.upload()
```

Proses upload file CSV dilakukan untuk memasukkan dataset ke dalam lingkungan Google Colaboratory agar dapat diproses menggunakan bahasa pemrograman Python. Dataset diunggah secara manual dari perangkat pengguna menggunakan fungsi files.upload() yang disediakan oleh Google Colab.

##### - Struktur Dataset

```
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("IRIS.csv")
df.head()
```

Berikut tampilan dari code di atas setelah di jalankan:


| index | sepal\_length | sepal\_width | petal\_length | petal\_width | species |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 0 | 5\.1 | 3\.5 | 1\.4 | 0\.2 | Iris-setosa |
| 1 | 4\.9 | 3\.0 | 1\.4 | 0\.2 | Iris-setosa |
| 2 | 4\.7 | 3\.2 | 1\.3 | 0\.2 | Iris-setosa |
| 3 | 4\.6 | 3\.1 | 1\.5 | 0\.2 | Iris-setosa |
| 4 | 5\.0 | 3\.6 | 1\.4 | 0\.2 | Iris-setosa |

Dataset memiliki 5 kolom, yaitu:

1. Sepal Length (cm) (Numeric)
Menunjukkan panjang kelopak luar bunga iris dalam satuan sentimeter.
2. Sepal Width (cm) (Numeric)
Menunjukkan lebar kelopak luar bunga iris dalam satuan sentimeter.
3. Petal Length (cm) (Numeric)
Menunjukkan panjang mahkota bunga iris dalam satuan sentimeter.
4. Petal Width (cm) (Numeric)
Menunjukkan lebar mahkota bunga iris dalam satuan sentimeter.
5. Species (Kategorikal - String)
Menunjukkan jenis bunga iris, yang terdiri dari tiga kategori:
    - Setosa
    - Versicolor
    - Virginica

##### -Statistik Deskriptif Awal

```
df.describe()
```

Statistik deskriptif digunakan untuk mengetahui gambaran umum data seperti nilai rata-rata, nilai minimum, dan maksimum dari setiap atribut numerik.

Dataset Iris memiliki 4 atribut numerik, yaitu:

- Sepal Length
- Sepal Width
- Petal Length
- Petal Width

Berikut contoh statistik deskriptifnya:


| index | sepal\_length | sepal\_width | petal\_length | petal\_width |
| :-- | :-- | :-- | :-- | :-- |
| count | 150\.0 | 150\.0 | 150\.0 | 150\.0 |
| mean | 5\.843333333333334 | 3\.0540000000000003 | 3\.758666666666666 | 1\.1986666666666668 |
| std | 0\.8280661279778629 | 0\.4335943113621737 | 1\.7644204199522617 | 0\.7631607417008414 |
| min | 4\.3 | 2\.0 | 1\.0 | 0\.1 |
| 25% | 5\.1 | 2\.8 | 1\.6 | 0\.3 |
| 50% | 5\.8 | 3\.0 | 4\.35 | 1\.3 |
| 75% | 6\.4 | 3\.3 | 5\.1 | 1\.8 |
| max | 7\.9 | 4\.4 | 6\.9 | 2\.5 |

Berdasarkan hasil analisis statistik deskriptif, dataset Iris terdiri dari 150 data pada setiap atribut numerik, sehingga dapat disimpulkan tidak terdapat missing value. Nilai rata-rata (mean) menunjukkan bahwa panjang sepal bunga iris adalah 5.84 cm, lebar sepal 3.05 cm, panjang petal 3.76 cm, dan lebar petal 1.20 cm. Standar deviasi tertinggi terdapat pada atribut petal length sebesar 1.76, yang menunjukkan bahwa variasi panjang petal lebih besar dibandingkan atribut lainnya. Nilai minimum dan maksimum menunjukkan rentang data yang masih dalam batas wajar, yaitu sepal length antara 4.3 hingga 7.9 cm dan petal width antara 0.1 hingga 2.5 cm. Selain itu, nilai kuartil (25%, 50%, dan 75%) menunjukkan distribusi data yang relatif stabil tanpa adanya penyimpangan ekstrem yang signifikan. Secara umum, dataset memiliki distribusi yang baik dan layak digunakan untuk proses analisis dan pemodelan pada tahap selanjutnya.

##### - Pengecekan Data Duplikat

```
df.duplicated().sum()
```

Code di atas merupakan code untuk pengecekan data duplikat, berikut untuk hasil dari pengecekan data duplikat:

```
np.int64(3)
```

Jadi setelah dilakukan pengecekan, terdapat data duplikat yaitu ada 3 data duplikat.

##### - Pengecekan Data Null

```
df.isnull().sum()
```

Berikut untuk tampilan hasil dari pengecekan data Null:


|  |  |
| :-- | :-- |
| sepal_length | 0 |
| sepal_width | 0 |
| petal_length | 0 |
| petal_width | 0 |
| species | 0 |

### 4. Verifikasi Data

Berdasarkan proses verifikasi, dataset Iris terdiri dari 150 data dengan 4 atribut numerik dan 1 atribut kategorikal (species). Hasil pengecekan menunjukkan bahwa seluruh atribut memiliki jumlah data yang sama (count = 150), sehingga dapat disimpulkan tidak terdapat missing value pada dataset. Tipe data juga telah sesuai, di mana atribut sepal length, sepal width, petal length, dan petal width bertipe numerik, sedangkan species bertipe kategorikal. Berdasarkan hasil pengecekan menggunakan Pandas, ditemukan sebanyak 3 data duplikat pada dataset. Keberadaan data duplikat ini tidak menunjukkan kesalahan pada dataset, namun perlu ditangani pada tahap data preparation agar tidak memengaruhi proses pemodelan.

### 5. Visualisasi Data

##### - Distribusi Jumlah Data per Species

![original image](https://cdn.mathpix.com/snip/images/Gt4lANFXJ1tvApce6vXv0vo4hu08VXJb7_bTkbfgn-M.original.fullsize.png)

Grafik bar digunakan untuk melihat jumlah data pada setiap species. Berdasarkan grafik, diketahui bahwa setiap species yaitu Iris-setosa, Iris-versicolor, dan Iris-virginica masing-masing memiliki 50 data. Hal ini menunjukkan bahwa dataset dalam kondisi seimbang (balanced dataset), sehingga tidak terdapat ketimpangan jumlah data antar kelas. Kondisi ini sangat baik untuk proses modeling karena dapat membantu menghasilkan model klasifikasi yang lebih akurat dan tidak bias terhadap kelas tertentu.

##### - Distribusi Data Fitur Numerik pada Dataset Iris

![original image](https://cdn.mathpix.com/snip/images/6u595jYczu5oHxd1yDMvEGkfvzQYsyvFrhOICQnLnGY.original.fullsize.png)

Berdasarkan histogram, dapat disimpulkan bahwa seluruh fitur numerik memiliki distribusi yang bervariasi dan tidak terdapat anomali yang ekstrem. Fitur petal_length dan petal_width menunjukkan pola distribusi yang lebih jelas dalam membedakan kelompok data, sehingga kedua fitur ini sangat berpotensi menjadi fitur penting dalam proses klasifikasi pada tahap modeling. Visualisasi ini membantu dalam memahami karakteristik dan penyebaran data pada tahap Data Understanding dalam metodologi CRISP-DM.

##### - Analisis Penyebaran Data dan Deteksi Outlier Menggunakan Boxplot

![original image](https://cdn.mathpix.com/snip/images/YfGQj5-WjGnpPQF6mWpdtoRSXTPrJIw5sXZ6l769Xo0.original.fullsize.png)

Berdasarkan boxplot, dapat disimpulkan bahwa setiap fitur memiliki penyebaran data yang berbeda. Fitur sepal_width menunjukkan adanya beberapa outlier, sedangkan fitur lainnya memiliki distribusi yang relatif normal tanpa outlier yang signifikan. Fitur petal_length dan petal_width memiliki variasi data yang cukup besar, sehingga berpotensi menjadi fitur penting dalam membedakan species pada tahap modeling. Visualisasi boxplot ini membantu dalam memahami distribusi data dan mendeteksi outlier pada tahap Data Understanding dalam metodologi CRISP-DM.

#### - Analisa Korelasi

- Analisis korelasi sepal length dan sepal width:

![original image](https://cdn.mathpix.com/snip/images/sRKmyd2sVULbo8caUWfLT9eqt_lCk_W0yBKY_EifcEA.original.fullsize.png)

Analisis korelasi juga dilakukan pada atribut sepal length dan sepal width menggunakan scatter plot dengan bantuan Python. Berdasarkan hasil visualisasi, terlihat bahwa sebaran data tidak membentuk pola yang jelas, yang menunjukkan bahwa hubungan antara kedua atribut tersebut relatif lemah dibandingkan dengan atribut pada bagian petal.

- Analisis korelasi petal length dan petal width

![original image](https://cdn.mathpix.com/snip/images/8bCKkvYBfRcBPq1RZkXrOMc8sxvl-fqDQ-RhbE9lwRo.original.fullsize.png)

Analisis korelasi dilakukan secara visual menggunakan scatter plot dengan bantuan bahasa pemrograman Python. Scatter plot digunakan untuk melihat hubungan antara atribut petal length dan petal width. Berdasarkan hasil visualisasi, terlihat bahwa titik-titik data membentuk pola yang jelas, yang menunjukkan adanya hubungan atau korelasi yang kuat antara kedua atribut tersebut.

### Eksplorasi Dataset dengan menggunakan Aplikasi Orange:

##### - Statistik Deskriptif

Statistik deskriptif digunakan untuk memahami karakteristik dasar dataset Iris Flower. Analisis ini dilakukan menggunakan Column Statistics pada aplikasi Orange Data Mining. Hasil statistik deskriptif menunjukkan nilai minimum, maksimum, rata-rata, dan standar deviasi dari setiap atribut, sehingga memberikan gambaran umum mengenai sebaran dan variasi data.

Berikut contoh statistik deskriptifnya:

![original image](https://cdn.mathpix.com/snip/images/1QfYry6Ge2WhH-6mPvVdIRTrstVxnt-bPx3SCKn8EtY.original.fullsize.png)

##### - Analisa kolerasi

Analisis korelasi antar atribut dilakukan secara visual menggunakan widget Scatter Plot pada aplikasi Orange Data Mining. Scatter Plot digunakan untuk melihat hubungan antara dua atribut numerik berdasarkan pola sebaran data. Berdasarkan hasil visualisasi, terlihat bahwa beberapa atribut, khususnya pada bagian petal, memiliki hubungan yang kuat, sedangkan atribut lainnya menunjukkan hubungan yang lebih lemah. Analisis ini membantu dalam memahami keterkaitan antar atribut sebelum masuk ke tahap pemodelan.

##### Berikut contoh Analisa Kolerasinya:

- Analisis korelasi sepal length dan sepal width:

![original image](https://cdn.mathpix.com/snip/images/iaM0IQD8x8ELhZrhLL7d07iobX8siwpqS55Ud7nF7uA.original.fullsize.png)

Berdasarkan hasil visualisasi scatter plot antara sepal length dan sepal width, terlihat bahwa titik-titik data tersebar secara acak dan tidak membentuk pola linear yang jelas. Hal ini menunjukkan bahwa hubungan antara kedua atribut tersebut relatif lemah. Dengan kata lain, perubahan nilai sepal length tidak secara konsisten diikuti oleh perubahan sepal width.

- Analisis korelasi petal length dan petal width

![original image](https://cdn.mathpix.com/snip/images/iT4Xj8fRcAwT4AI_oDVDJKNmDaNI3cv0BFxkJoB2VBw.original.fullsize.png)

Berbeda dengan atribut sepal, scatter plot antara petal length dan petal width menunjukkan pola sebaran yang lebih teratur dan cenderung membentuk hubungan linear positif. Titik-titik data terlihat mengikuti arah tertentu, yang menandakan adanya korelasi yang kuat antara kedua atribut petal tersebut.

### 6. Pengukuran Jarak 
##### Similarity

Similarity adalah ukuran numerik yang menunjukkan tingkat kemiripan antara dua objek berdasarkan fitur yang mereka miliki. Secara umum, nilai similarity biasanya berada di rentang 0 sampai 1

- 0 -> tidak mirip sama sekali
- 1 -> sangat mirip / identik

Semakin besar nilainya → semakin mirip.

##### Dissimilarity
Dissimilarity adalah ukuran numerik yang menunjukkan seberapa berbeda dua objek/data.
- Semakin besar nilai dissimilarity -> semakin berbeda kedua objek tersebut.
- Semakin kecil nilainya -> semakin mirip.
  
Dalam data numerik, dissimilarity biasanya direpresentasikan dalam bentuk distance (jarak) di dalam ruang fitur (feature space). Secara umum dapat ditulis sebagai: $$
d(x,y)
$$ yang berarti jarak antara objek 𝑥 dan 𝑦. 

Untuk data bertipe numerik (misalnya tinggi, berat, nilai, umur), dissimilarity dihitung menggunakan metode berbasis jarak matematis.
Tiga metode yang paling umum digunakan adalah:

1. Minkowski Distance

Minkowski Distance adalah bentuk umum dari pengukuran jarak dalam ruang berdimensi. Rumusnya:
  $$d(2,1) = \sqrt{(1{,}9 - 5{,}1)^2 + (3{,}3 - 3{,}5)^2 + (1{,}4 - 1{,}4)^2 + (0{,}2 - 0{,}2)^2}$$

$$ = \sqrt{0{,}04 + 0{,}04 + 0 + 0{,}25} $$

$$ = 0{,}538 $$

$$d(3,1) = \sqrt{(4{,}7 - 5{,}1)^2 + (3{,}2 - 3{,}5)^2 + (1{,}3 - 1{,}4)^2 + (0{,}2 - 0{,}2)^2}$$

$$= \sqrt{0{,}16 + 0{,}09 + 0{,}01 + 0}$$

$$= 0{,}50990$$
Keterangan:

𝑥𝑖 dan 𝑦𝑖 = nilai atribut ke-i

𝑝 = parameter yang menentukan jenis jarak

𝑛 = jumlah atribut

Minkowski disebut sebagai bentuk umum karena ketika nilai p diubah, maka akan menghasilkan metode jarak yang berbeda.

2. Manhattan Distance

Manhattan Distance adalah metode pengukuran jarak yang digunakan untuk menghitung tingkat perbedaan (dissimilarity) antara dua objek dengan menjumlahkan selisih absolut dari setiap atributnya.
Metode ini menghitung jarak berdasarkan jalur horizontal dan vertikal (seperti pola jalan berbentuk grid), bukan garis lurus. Semakin besar nilai Manhattan Distance, maka semakin besar perbedaan antar objek. Nilai minimum jaraknya adalah 0, yang menunjukkan bahwa dua objek identik.

$$d(i,j) = {|x_{i1} - x_{j1}|+| x_{i2} - x_{j2}|^2 + \ldots + |x_{ip} - x_{jp}|}$$

3. Euclidean Distance

Euclidean Distance merupakan salah satu metode pengukuran jarak yang termasuk dalam kategori dissimilarity, karena metode ini mengukur tingkat perbedaan antara dua objek berdasarkan nilai atribut numeriknya. Semakin besar hasil perhitungan Euclidean Distance, maka semakin besar perbedaan (dissimilarity) antara dua objek tersebut. Sebaliknya, jika hasilnya mendekati 0, maka kedua objek semakin mirip.

$$d(i,j) = \sqrt{(x_{i1} - x_{j1}|^2 +| x_{i2} - x_{j2}|^2 + \ldots + |x_{ip} - x_{jp}|^2})$$


   
Jaccard Dissimilarity adalah ukuran untuk menghitung tingkat perbedaan antara dua objek pada data biner (0 dan 1) yang bersifat tidak simetris.
Tidak simetris artinya:

- Nilai 1 (kehadiran / ya) lebih penting.
- Nilai 0 (ketidakhadiran / tidak) tidak dianggap penting.
- Pasangan 0–0 diabaikan.

Metode ini banyak digunakan dalam:

- Market basket analysis.
- Data transaksi.
- Clustering data biner.
- Analisis kemunculan fitur.


### 7. Pengukuran Jarak pada Dataset Iris Flowers
#### 1). Mengukur jarak dataset Tipe Numerik (Iris Flowers)
Pada dataset Iris diatas, tipe data yang dimiliki adalah bernilai numerik, karena hal itu penghitungan jarak untuk tipe data Iris kali ini akan digunakan metode Eucledian Distance
##### Perhitungan manual 
Contoh perhitungan secara manual pada baris 2 dan 3 dengan kolom 1:
$$
d(2,1) = \sqrt{(|4{,}9 - 5{,}1|^2 + |3 - 3{,}5|^2 + |1{,}4 - 1{,}4|^2 + |0{,}2 - 0{,}2|^2)}
$$
$$
= \sqrt{0{,}04 + 0{,}25}
$$
$$
= 0{,}538
$$
$$
d(3,1) = \sqrt{(|4{,}7 - 5{,}1|^2 + |3{,}2 - 3{,}5|^2 + |1{,}3 - 1{,}4|^2 + |0{,}2 - 0{,}2|^2)}
$$

$$
= \sqrt{0{,}16 + 0{,}09 + 0{,}01 + 0}
$$

$$
= 0{,}50990
$$
##### Perhitungan Python
Kita juga dapat menggunakan tools seperti python untuk mempermudah perhitungan ini
```
from scipy.spatial.distance import pdist, squareform

X = df.select_dtypes(include=['float64', 'int64'])
dist = pdist(X, metric='euclidean')
distance_matrix = squareform(dist)

distance_df = pd.DataFrame(distance_matrix)
print(distance_df.iloc[:5, :5])
```

Terdapat tambahan library yang digunakan yaitu library scipy untuk mempermudah kita menghitung Euclidean Distance tanpa perlu coding manual. Selanjutnya data ditampilkan hanya sebesar 5x5 dihitung dari yang pertama

Output yang dihasilkan:
|   | 0        | 1        | 2        | 3        | 4        |
|---|----------|----------|----------|----------|----------|
| 0 | 0.000000 | 0.538516 | 0.509902 | 0.648074 | 0.141421 |
| 1 | 0.538516 | 0.000000 | 0.300000 | 0.331662 | 0.608276 |
| 2 | 0.509902 | 0.300000 | 0.000000 | 0.244949 | 0.509902 |
| 3 | 0.648074 | 0.331662 | 0.244949 | 0.000000 | 0.648074 |
| 4 | 0.141421 | 0.608276 | 0.509902 | 0.648074 | 0.000000 |

##### Perhitungan Orange
![original image](https://cdn.mathpix.com/snip/images/9PGgc7ny0uksXf9OeJWUUqJ1W74_wYqqPmGLGPGhBL8.original.fullsize.png)
3 perhitungan diatas, memiliki kesamaan nilai, artinya perhitungan yang dilakukan sudah dilakukan secara benar dan urut

#### 2). Mengukur jarak dataset Tipe data campuran

#### Perhitungan
Perhitungan manual antara baris 1 dan 2, lalu baris 1 dan 3: Untuk menghitung jarak pada data campuran, kita perlu menghitungnya pada masing-masing tipe data yang kemudian hasil akhirnya dijumlahkan. Dataset yang digunakan bersumser dari kaggle (https://www.kaggle.com/datasets/spscientist/students-performance-in-exams).

1. Ordinal dan Numerik Pada kolom parental level of education merupakan fitur dengan tipe ordinal. Pada tipe data ordinal ini sebelum dapat menghitung jarak pada baris 1 dan 2, lalu baris 1 dan 3 perlu melakukan normalisasi, yang setelah dinormalisasi nanti dapat dihitung jaraknya dengan tipe data numerik menggunakan metode Eucledien Distance.
- Some high school = 1
- High school = 2
- Some collage = 3
- Associate’s degree = 4
- Bachelor’s degree = 5
- Master’s degree = 6

Proses Normalisasi pada tiap tingkatan:
Normalisasi = r - min / max - min

$$
1 = \frac{1-1}{6-1} = \frac{0}{5} = 0
$$

$$
2 = \frac{2-1}{6-1} = \frac{1}{5} = 0.2
$$

$$
3 = \frac{3-1}{6-1} = \frac{2}{5} = 0.4
$$

$$
4 = \frac{4-1}{6-1} = \frac{3}{5} = 0.6
$$

$$
5 = \frac{5-1}{6-1} = \frac{4}{5} = 0.8
$$

$$
6 = \frac{6-1}{6-1} = \frac{5}{5} = 1
$$

Setelah dilakukan normalisasi pada tipe data ordinal, selanjutnya ada proses perhitungan menggunakan metode Eucledien Distance, yang dihitung dengan tipe data numerik yaitu pada kolom Math Score, Reading Score dan Writing Score.

| Parental Level of Education | Math Score | Reading Score | Writing Score |
|-----------------------------|------------|---------------|---------------|
| 0.6                         | 72         | 72            | 74            |
| 0.4                         | 69         | 90            | 88            |
| 1                           | 90         | 95            | 93            |
| 0.6                         | 47         | 57            | 44            |

$$
\begin{aligned}
d(1,2) &= \sqrt{(0.6-0.4)^2 + (72-69)^2 + (72-90)^2 + (74-88)^2} \\
       &= \sqrt{0.04 + 9 + 324 + 196} \\
       &= \sqrt{529.04} \\
       &= 23.00086
\end{aligned}
$$

$$
\begin{aligned}
d(1,3) &= \sqrt{(0.6-1)^2 + (72-90)^2 + (72-95)^2 + (74-93)^2} \\
       &= \sqrt{0.16 + 324 + 529 + 361} \\
       &= \sqrt{1214.16} \\
       &= 34.844
\end{aligned}
$$

Jadi jarak pada tipe data ordinal dan numerik pada d(1,2) = 23.00086 dan pada d(1,3)= 34.844 2. Binary Perhitungan jarak pada tipe data biner kita perlu menentukan simetris dan asimetrisnya terlebih dahulu, karena untuk perhitungan jarak yang simteris dan asimetris berbeda. Pada data campuran yang sekarang kita pake untuk yang tipe binary ada 3 fitur yaitu fitur gender yang simetris, lunch dan test preparation course yang asimetris.

a. d(1,2)
  - Simetris
  q = 0, r = 0, s = 0, t = 1 

$$
d = \frac{r+s}{q+r+s+t}
$$
  
$$
d = \frac{0+0}{0+0+0+1}
= 0
$$

- Asimetris
  q = 1, r = 0, s = 1, t = 0
$$
d = \frac{r+s}{q+r+s}
$$

$$
d = \frac{0+1}{1+0+1} = 0.5
$$

    d(1,2) = 0 + 0.5 = 0.5

b. d(1,3)
  - Simetris
  q = 0, r = 0, s = 0, t = 1 

$$
d = \frac{r+s}{q+r+s+t}
$$
  
$$
d = \frac{0+0}{0+0+0+1}
= 0
$$

- Asimetris
  q = 1, r = 0, s = 1, t = 1
$$
d = \frac{r+s}{q+r+s}
$$

$$
d = \frac{0+0}{1+0+0} = 0
$$

d(1,2) = 0 + 0 = 0

Jadi, jarak pada tipe data binary pada d(1,2) = 0.5 dan d(1,3) = 0.

3. Kategori Pada tipe data kategori menggunakan rumus p-m/p, dimana p adalah banyaknya fitur dan m adalah jumlah kategori apabila antara kategori data ke-1 dan ke-2 sama maka satu, dan apabila berbeda maka 0.

d(1,2) = 1-0/1 = 1/1 = 1

d(1,3) = 1-1/1 = 0/1 = 0

Jadi, jarak tipe data kategori pada d(1,2) = 1 dan d(1,3) = 0

Untuk menentukan hasil akhirnya, yaitu jarak d(1,2) dan d(1,3) adalah dengan menjumlahkan hasil hitung jarak dari masing-masing tipe data di atas.

d(1,2) = 23,00086 + 0,5 + 1 = 24,50086

d(1,3) = 34,844 + 0 + 0 = 34,844.

