# Prediksi Kadar Nitrogen Dioksida (NO₂) di Kota Probolinggo Menggunakan Algoritma K-Nearest Neighbor (KNN)

## Latar belakang

Kualitas udara merupakan salah satu faktor penting yang memengaruhi kesehatan masyarakat dan lingkungan. Salah satu polutan yang sering digunakan sebagai indikator pencemaran udara adalah **Nitrogen Dioksida (NO₂)**, yang merupakan gas beracun yang dihasilkan terutama dari proses pembakaran bahan bakar fosil seperti kendaraan bermotor, pembangkit listrik, dan kegiatan industri. Oleh karena itu, pemantauan dan peramalan kadar NO₂ perlu dilakukan untuk mengetahui kondisi kualitas udara serta mengantisipasi peningkatan tingkat pencemaran.

Dengan perkembangan teknologi penginderaan jauh, data konsentrasi NO₂ dapat diperoleh melalui satelit Sentinel-5P secara berkala. Data tersebut dapat dimanfaatkan untuk membangun model peramalan berdasarkan pola historis yang tersedia. Pada penelitian ini digunakan metode K-Nearest Neighbor (KNN) Regression untuk memprediksi konsentrasi NO₂ di Kota Probolinggo. Hasil peramalan diharapkan dapat memberikan gambaran mengenai kondisi kualitas udara pada periode mendatang serta menjadi informasi pendukung dalam pengelolaan lingkungan.

## A. Pengumpulan Data

Tahap pertama dalam penelitian ini adalah mengumpulkan data deret waktu (time series) harian konsentrasi NO₂ di wilayah Bangkalan, Madura. Data diperoleh melalui platform Copernicus Data Space Ecosystem. Sebelum melakukan pengambilan data, pengguna perlu membuat akun terlebih dahulu pada platform tersebut. Proses pengambilan data dilakukan dengan mengikuti dokumentasi OpenEO yang disediakan oleh Copernicus, yang dapat diakses melalui Panduan Pengambilan Data NO₂ dengan OpenEO. Dokumentasi tersebut menjelaskan langkah-langkah pengambilan dan pengolahan data NO₂ menggunakan layanan OpenEO yang terintegrasi dengan data satelit Sentinel-5P.

### 1. Instalasi Library OpenEO

```
!pip install openeo
```

Kode ini digunakan untuk menginstal library OpenEO pada lingkungan Python. Library OpenEO diperlukan untuk menghubungkan program dengan platform Copernicus Data Space Ecosystem dan mengakses data satelit.

### 2. Import Library OpenEO

```
import openeo
```

Kode di atas digunakan untuk mengimpor library OpenEO ke dalam program Python. Setelah library berhasil diinstal, perintah import openeo diperlukan agar seluruh fungsi dan fitur yang tersedia pada OpenEO dapat digunakan. Library ini berperan sebagai antarmuka yang menghubungkan program Python dengan platform Copernicus Data Space Ecosystem, sehingga pengguna dapat melakukan koneksi ke server, mengakses data satelit, serta menjalankan proses pengolahan data secara langsung melalui kode Python. Langkah ini merupakan tahap awal sebelum melakukan autentikasi dan pengambilan data NO₂ dari satelit Sentinel-5P.

### 3. Koneksi ke Copernicus Data Space Ecosystem

```
connection = openeo.connect("openeo.dataspace.copernicus.eu").authenticate_oidc()
```

Pada saat menjalankan baris code diatas (connection), nanti akan diminta authentikasi. Authentikasi dapat dilakukan dengan mengklik link authentikasi lalu login menggunakan akun “copernicus” yang tengah dibuat sebelumnya.
Output autentikasi berhasil:

```
Visit https://identity.dataspace.copernicus.eu/auth/realms/CDSE/device?user_code=OGME-UTEB 📋 to authenticate.
✅ Authorized successfully
Authenticated using device code flow.
```

Setelah proses autentikasi berhasil, objek connection akan digunakan untuk mengakses berbagai layanan yang tersedia, seperti mengambil data satelit Sentinel-5P, menentukan wilayah penelitian, serta melakukan proses pengolahan data secara langsung melalui platform Copernicus Data Space Ecosystem. Tahap ini merupakan langkah penting karena seluruh proses pengambilan data memerlukan koneksi yang valid ke server OpenEO.

### 4. Menentukan Area Penelitian dan Mengambil Data NO₂ Sentinel-5P

```
aoi = {
    "type": "Polygon",
    "coordinates": [
        [
            [113.16, -7.73],
            [113.25, -7.73],
            [113.25, -7.82],
            [113.16, -7.82],
            [113.16, -7.73]
        ]
    ]
}

s5post = connection.load_collection(
    "SENTINEL_5P_L2",
    temporal_extent=["2023-10-01", "2025-10-01"],
    spatial_extent={
        "west": 113.16,
        "south": -7.82,
        "east": 113.25,
        "north": -7.73
    },
    bands=["NO2"],
)

# Now aggregate by day to avoid having multiple data per day
s5p_no2_daily = s5post.aggregate_temporal_period(reducer="mean", period="day")

# Now create a spatial aggregation to generate mean timeseries data
s5p_no2_aoi = s5p_no2_daily.aggregate_spatial(reducer="mean", geometries=aoi)
```

Kode diatas memerlukan titik koordinasi area yang akan digunakan untuk melakukan perhitungan, untuk mengambil titik koordinasi kaian kunjungi webiste https://geojson.io/\#map=14.8/-7.04732/112.69463 atau ikuti langkah yang tengah dijelaskan pada website https://maul112.github.io/psd/no2_2.html\#

Kode tersebut digunakan untuk mengambil data konsentrasi NO₂ dari satelit Sentinel-5P pada wilayah Kota Probolinggo selama periode 1 Oktober 2023 hingga 1 Oktober 2025. Data kemudian diolah dengan menghitung rata-rata harian (temporal aggregation) dan rata-rata seluruh area penelitian (spatial aggregation), sehingga menghasilkan data time series harian NO₂ yang siap digunakan untuk analisis dan peramalan.

### 5. Mengekspor Data ke Format NetCDF

```
job = s5post.execute_batch(title="NO2 in Probolinggo", outputfile="NO2Probolinggo.nc")
```

Kode ini digunakan untuk menjalankan proses pengolahan data NO₂ (Nitrogen Dioksida) secara batch menggunakan layanan pemrosesan data satelit. Proses ini akan mengambil data yang sudah ditentukan, lalu menjalankannya di server untuk menghasilkan output berupa file berformat NetCDF (.nc). File tersebut diberi nama NO2Probolinggo.nc dan berisi hasil data konsentrasi NO₂ di wilayah Probolinggo yang nantinya bisa digunakan untuk analisis lebih lanjut.

Output:

```
0:00:00 Job 'j-2606030333364fa09d15b71a1f0056c1': send 'start'
0:00:05 Job 'j-2606030333364fa09d15b71a1f0056c1': queued (progress 0%)
0:00:10 Job 'j-2606030333364fa09d15b71a1f0056c1': queued (progress 0%)
0:00:16 Job 'j-2606030333364fa09d15b71a1f0056c1': queued (progress 0%)
0:00:25 Job 'j-2606030333364fa09d15b71a1f0056c1': queued (progress 0%)
0:00:35 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:00:47 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:01:03 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:01:22 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:01:46 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:02:16 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:02:54 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:03:41 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:04:39 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:05:39 Job 'j-2606030333364fa09d15b71a1f0056c1': running (progress N/A)
0:06:40 Job 'j-2606030333364fa09d15b71a1f0056c1': finished (progress 100%)
```


## B. Preprocessing Data

### 1. Instalasi Library netCDF4

```
!pip install netCDF4
```

Perintah ini digunakan untuk menginstal library netCDF4 pada Python.

### 2. Membaca dan Mengeksplorasi Data NetCDF (NO₂ Probolinggo)

```
import netCDF4

file_path = "NO2Probolinggo.nc"
ds = netCDF4.Dataset(file_path)

# Lihat seluruh variabel yang tersedia
print("📦 Variabel dalam file:")
print(ds.variables.keys())
# dict_keys(['t', 'x', 'y', 'crs', 'NO2'])

# Ambil NO2
no2 = ds.variables["NO2"][:]

# Ambil Time
time = ds.variables["t"][:]

# Konversi waktu ke format tanggal jika punya atribut 'units'
try:
    time_units = ds.variables["t"].units
    dates = netCDF4.num2date(time, units=time_units)
except Exception:
    dates = time  # fallback kalau tidak ada units

# Tampilkan struktur data NO2
print(type(no2))
# type <class 'numpy.ma.core.MaskedArray'>

print(len(no2))
# banyaknya data record NO2 725

print(len(no2[0]))
# panjang data perbaris 9

print(len(no2[0][0]))
# panjang perdata 8

print(no2[0][0][0])
# 3.7701793e-05
```

Kode ini digunakan untuk membuka file hasil olahan data satelit NO₂ Probolinggo (NO2Probolinggo.nc) dan melihat isi serta struktur datanya menggunakan library netCDF4.

Secara keseluruhan, tahap ini berfungsi untuk memahami isi data, struktur data, dan memastikan data siap digunakan untuk tahap preprocessing dan analisis selanjutnya.

### 3. Interpolasi Missing Value Data NO₂

```
import numpy as np
import pandas as pd

# Interpolasi missing value
no2_filled = np.zeros_like(no2)
no2_filled = no2_filled.filled(0)

# Loop tiap grid (y,x)
for i in range(no2.shape[1]):     # 9 baris
    for j in range(no2.shape[2]): # 8 kolom
        series = pd.Series(no2[:, i, j])
        no2_filled[:, i, j] = series.interpolate(
            method='linear',
            limit_direction='both'
        ).to_numpy()
```

Kode ini digunakan untuk mengisi nilai yang hilang (missing value) pada data NO₂ Probolinggo menggunakan metode interpolasi linear.

Prosesnya dilakukan dengan mengubah setiap titik grid menjadi deret waktu (time series), lalu nilai yang kosong diisi berdasarkan perhitungan nilai sebelum dan sesudahnya. Hasil akhirnya disimpan ke variabel no2_filled agar data lebih lengkap dan siap dianalisis.

### 4. Mengubah Format Tanggal dan Merata-ratakan Data NO₂

```
new_dates = []
new_no2 = []

for i in range(len(dates)):
    new_dates.append(dates[i].strftime('%Y-%m-%d'))
    new_no2.append(np.mean(no2_filled[i]))
```

Kode ini digunakan untuk mengubah format tanggal dan menghitung rata-rata konsentrasi NO₂ setiap waktu.

Secara keseluruhan, tahap ini bertujuan untuk meringkas data NO₂ menjadi nilai rata-rata per hari beserta tanggalnya agar lebih mudah dianalisis atau divisualisasikan.

### 5. Membuat DataFrame dan Menyimpan ke CSV

```
df = pd.DataFrame({
    "date": new_dates,
    "NO2": new_no2
})

# Tampilkan 5 data pertama
print(df.head())

# Simpan ke CSV
df.to_csv("NO2_Probolinggo_timeseries.csv", index=False)

print("File berhasil disimpan!")
```

Output:

```
         date       NO2
0  2023-10-01  0.000020
1  2023-10-02  0.000028
2  2023-10-03  0.000046
3  2023-10-04  0.000031
4  2023-10-05  0.000038
File berhasil disimpan!
```

Kode ini digunakan untuk menggabungkan data tanggal dan nilai rata-rata NO₂ ke dalam bentuk DataFrame menggunakan Pandas.

Setelah itu, data ditampilkan sebanyak 5 baris pertama untuk memastikan hasilnya benar. Kemudian, data disimpan ke dalam file berformat CSV dengan nama NO2_Probolinggo_timeseries.csv agar bisa digunakan untuk analisis atau visualisasi di aplikasi lain seperti Excel atau Python.

Secara keseluruhan, tahap ini berfungsi untuk mengubah hasil pengolahan data menjadi dataset akhir dan menyimpannya dalam bentuk file.

### 6. Mengecek Missing Date pada Data Time Series NO₂

```
import pandas as pd
import numpy as np

df = pd.read_csv("NO2_Probolinggo_timeseries.csv")

# Pastikan kolom 'date' bertipe datetime
df['date'] = pd.to_datetime(df['date'])

# Buat rentang tanggal lengkap
start_date = "2023-10-01"
end_date = "2025-09-30"
full_range = pd.date_range(start=start_date, end=end_date, freq='D')

# Cek tanggal yang hilang
missing_dates = full_range.difference(df['date'])

print(f"Jumlah hari missing: {len(missing_dates)}")
print("Daftar tanggal missing:")
print(missing_dates)
```

Output:

```
Jumlah hari missing: 7
Daftar tanggal missing:
DatetimeIndex(['2023-11-11', '2024-01-01', '2024-03-11', '2024-03-23',
               '2024-08-12', '2025-01-30', '2025-01-31'],
              dtype='datetime64[ns]', freq=None)
```

Kode ini digunakan untuk mengecek apakah ada tanggal yang hilang (missing date) pada data NO₂ yang sudah berbentuk time series.

Pertama, data CSV dibaca dan kolom date diubah menjadi format datetime agar bisa diproses sebagai data waktu. Kemudian dibuat rentang tanggal lengkap dari 1 Oktober 2023 sampai 30 September 2025 dengan interval harian.

Selanjutnya, program membandingkan rentang tanggal lengkap tersebut dengan tanggal yang ada di dataset untuk menemukan tanggal yang tidak tersedia.

Hasilnya berupa jumlah hari yang hilang dan daftar tanggal yang tidak memiliki data NO₂.

### 7. Menangani Missing Date dan Interpolasi Time Series NO₂

```
import pandas as pd

# Pastikan datetime dan sorting
df['date'] = pd.to_datetime(df['date'])
df = df.sort_values('date')

# Buat rentang tanggal lengkap
full_range = pd.date_range(start="2023-10-01", end="2025-09-30", freq='D')

# Reindex agar tanggal yang hilang muncul sebagai NaN
df = df.set_index('date').reindex(full_range)
df.index.name = 'date'

# Interpolasi linear berdasarkan waktu
df['NO2'] = df['NO2'].interpolate(method='time')

# Mengisi nilai kosong di awal/akhir data
df['NO2'] = df['NO2'].fillna(method='bfill').fillna(method='ffill')

# Simpan kembali ke CSV
df.to_csv("no2_timeseries_interpolated.csv")
```

Kode ini digunakan untuk memperbaiki data time series NO₂ yang masih memiliki tanggal hilang (missing date).

Pertama, data diurutkan berdasarkan tanggal dan diubah ke format datetime. Kemudian dibuat rentang tanggal lengkap dari 1 Oktober 2023 sampai 30 September 2025. Setelah itu, data direindex sehingga tanggal yang hilang akan muncul sebagai nilai kosong (NaN). Nilai kosong tersebut kemudian diisi menggunakan interpolasi berbasis waktu (time interpolation).

Jika masih ada nilai kosong di awal atau akhir data, digunakan metode forward/backward fill untuk mengisi nilai tersebut. Terakhir, data yang sudah lengkap disimpan kembali ke file CSV baru agar siap digunakan untuk analisis lanjutan.

### 8. Mengecek Ulang Missing Date Setelah Interpolasi

```
import pandas as pd
import numpy as np

df = pd.read_csv("no2_timeseries_interpolated.csv")

# Pastikan kolom 'date' bertipe datetime
df['date'] = pd.to_datetime(df['date'])

# Buat rentang tanggal lengkap
start_date = "2023-10-01"
end_date = "2025-09-30"
full_range = pd.date_range(start=start_date, end=end_date, freq='D')

# Cek tanggal yang hilang
missing_dates = full_range.difference(df['date'])

print(f"Jumlah hari missing: {len(missing_dates)}")
print("Daftar tanggal missing:")
print(missing_dates)
```

Output:

```
Jumlah hari missing: 0
Daftar tanggal missing:
DatetimeIndex([], dtype='datetime64[ns]', freq='D')
```

Kode ini digunakan untuk mengecek kembali apakah masih ada tanggal yang hilang (missing date) setelah proses interpolasi dilakukan.

### 9. Deteksi Outlier Data NO₂ Menggunakan Metode IQR

```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("no2_timeseries_interpolated.csv")

df['date'] = pd.to_datetime(df['date'])

# Hitung IQR
Q1 = df['NO2'].quantile(0.25)
Q3 = df['NO2'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# Filter outlier
outliers_iqr = df[(df['NO2'] < lower_bound) | (df['NO2'] > upper_bound)]

print("Jumlah Outlier (IQR):", len(outliers_iqr))
print(outliers_iqr[['date', 'NO2']].head())
```

Output:

```
Jumlah Outlier (IQR): 13
          date       NO2
38  2023-11-08  0.000062
239 2024-05-27  0.000072
255 2024-06-12  0.000071
278 2024-07-05  0.000069
282 2024-07-09  0.000081
```

Kode ini digunakan untuk mendeteksi data outlier (nilai yang terlalu ekstrem) pada data NO₂ menggunakan metode Interquartile Range (IQR).

Pertama, data dibaca dan kolom date diubah ke format datetime. Selanjutnya dihitung nilai Q1 (kuartil 1) dan Q3 (kuartil 3), lalu dihitung selisihnya yang disebut IQR. Setelah itu ditentukan batas bawah dan batas atas data normal. Data yang berada di luar batas tersebut dianggap sebagai outlier.

Terakhir, program menampilkan jumlah outlier serta beberapa contoh data yang terdeteksi sebagai outlier.

### 10. Visualisasi Deteksi Outlier Data NO₂ (Metode IQR)

```
# === Visualisasi ===
plt.figure(figsize=(15,5))
plt.plot(df['date'], df['NO2'], label="NO2", linewidth=1)

# Titik Outlier
plt.scatter(outliers_iqr['date'], outliers_iqr['NO2'], 
            color='red', marker='o', label="Outliers")

# Garis batas atas & bawah
plt.axhline(upper_bound, color='orange', linestyle='dashed', label="Upper Bound (IQR)")
plt.axhline(lower_bound, color='blue', linestyle='dashed', label="Lower Bound (IQR)")

plt.title("Deteksi Outlier Data NO2 (Metode IQR)")
plt.xlabel("Tanggal")
plt.ylabel("Kadar NO2")
plt.legend()
plt.tight_layout()

plt.xticks(
    ticks=[df['date'].iloc[0], df['date'].iloc[-1]],
    labels=[df['date'].iloc[0].strftime('%Y-%m-%d'),
            df['date'].iloc[-1].strftime('%Y-%m-%d')]
)

plt.show()
```

![original image](https://cdn.mathpix.com/snip/images/08_NCZJRhYQx7NP5rWWC3fcoa1mRvzZCEq5EsPwPKY0.original.fullsize.png)

### 11. Membersihkan Outlier dan Mengisi Kembali Data NO₂

```
# Tandai outlier menjadi NaN
df['NO2_cleaned'] = df['NO2'].mask((df['NO2'] < lower_bound) | (df['NO2'] > upper_bound))

print("Jumlah nilai yang dinyatakan sebagai outlier:", df['NO2_cleaned'].isna().sum())

# Interpolasi linear untuk mengisi kembali nilai outlier
df['NO2_filled'] = df['NO2_cleaned'].interpolate(method='linear')

# Jika masih tersisa NaN di ujung data, isi dengan forward/backward fill
df['NO2_filled'] = df['NO2_filled'].bfill().ffill()

print("Jumlah missing setelah interpolasi:", df['NO2_filled'].isna().sum())
```

Kode ini digunakan untuk membersihkan data NO₂ dari outlier dan mengisi kembali nilai yang hilang.

### 12. Visualisasi Data NO₂ Setelah Cleaning (Outlier Removal \& Interpolasi)

```
plt.figure(figsize=(15,5))

# Plot data hasil interpolasi
plt.plot(df['date'], df['NO2_filled'], label="NO2 (Interpolated)", linewidth=1)

# Tampilkan hanya tanggal awal dan akhir di sumbu X
plt.xticks(
    ticks=[df['date'].iloc[0], df['date'].iloc[-1]],
    labels=[df['date'].iloc[0].strftime('%Y-%m-%d'),
            df['date'].iloc[-1].strftime('%Y-%m-%d')]
)

plt.title("Plot Data NO2 Setelah Outlier Removal & Interpolasi")
plt.xlabel("Tanggal")
plt.ylabel("Kadar NO2")
plt.legend()
plt.tight_layout()
plt.show()
```

![original image](https://cdn.mathpix.com/snip/images/3_NQD0XxyDB0_tUz6mQNUndMvkLTq9HoOsNkp48i_R8.original.fullsize.png)

