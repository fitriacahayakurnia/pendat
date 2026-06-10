# Analisis Skforecast Explainability

## Prediksi Permintaan Listrik Victoria, Australia

---

## 1. Analisis Prediksi Tentang Apa?

Model memprediksi **total konsumsi energi listrik harian (Electricity Demand)** di negara bagian **Victoria, Australia**, dalam satuan **Megawatt (MW)**.

Dataset aslinya (`vic_electricity`) berisi data per **30 menit** dengan 52.608 baris, kemudian diagregasi menjadi **data harian** menggunakan `resample('D')`:

- `Demand` → dijumlah per hari (`sum`)
- `Temperature` → dirata-rata per hari (`mean`)

| Info | Nilai |
| :-- | :-- |
| Total data asli | 52.608 baris (interval 30 menit) |
| Setelah resample | 1.097 hari (2012–2014) |
| Data training | s.d. 21 Desember 2014 (1.087 hari) |
| Data testing | 22–31 Desember 2014 (10 hari) |
| Target prediksi | 10 hari ke depan |

## 2. Bentuk Data Training (Input dan Output)

Model menggunakan `ForecasterRecursive` dengan `lags=7` dan variabel eksogen `Temperature`. Matriks training yang terbentuk adalah:


| Kolom | Peran | Tipe | Keterangan |
| :-- | :-- | :-- | :-- |
| `lag_1` | INPUT | Fitur lag | Demand 1 hari sebelumnya |
| `lag_2` | INPUT | Fitur lag | Demand 2 hari sebelumnya |
| `lag_3` | INPUT | Fitur lag | Demand 3 hari sebelumnya |
| `lag_4` | INPUT | Fitur lag | Demand 4 hari sebelumnya |
| `lag_5` | INPUT | Fitur lag | Demand 5 hari sebelumnya |
| `lag_6` | INPUT | Fitur lag | Demand 6 hari sebelumnya |
| `lag_7` | INPUT | Fitur lag | Demand seminggu lalu |
| `Temperature` | INPUT | Eksogen | Suhu rata-rata harian Victoria (°C) |
| `y (Demand)` | **OUTPUT** | Target | Total demand listrik hari yang diprediksi (MW) |

**Contoh 1 baris X_train:**

```
lag_1=205.338 | lag_2=211.066 | lag_3=213.792 | lag_4=258.955
lag_5=275.490 | lag_6=227.778 | lag_7=82.531  | Temp=24.1
→ y = 200.693 MW
```

Artinya: **8 kolom input → 1 nilai output** per baris data training.

## 3. Apa Itu Lag?

**Lag** adalah nilai dari deret waktu (time series) pada **waktu sebelumnya**, yang digunakan sebagai fitur (input) untuk memprediksi nilai saat ini.

### Definisi matematis

Jika `y(t)` = demand hari ke-t, maka:

```
lag_1 = y(t-1)  ← demand kemarin
lag_2 = y(t-2)  ← demand 2 hari lalu
lag_3 = y(t-3)  ← demand 3 hari lalu
...
lag_7 = y(t-7)  ← demand seminggu lalu
```


### Mengapa lags=7?

Dengan `lags=7`, model diberi **"memori" satu minggu penuh**. Ini penting karena:

- Konsumsi listrik berpola **mingguan** (hari kerja vs akhir pekan berbeda)
- `lag_7` menangkap pola yang sama dari minggu lalu di hari yang sama
- Model bisa "tahu" apakah hari ini adalah hari kerja atau libur, secara implisit dari pola historis


### Analogi sederhana

> Untuk memprediksi listrik yang dipakai *hari ini*, kita gunakan data listrik yang dipakai *kemarin, 2 hari lalu, ..., hingga 7 hari lalu* sebagai petunjuk — ditambah informasi suhu udara hari ini.

## 4. Proses Analisis yang Dilakukan

Analisis dilakukan dalam **7 tahap utama**:

### Tahap 1 — Persiapan Data

- Download dataset `vic_electricity` dari skforecast datasets
- Resample dari interval 30 menit → harian (`resample('D')`)
- Split data: training (2012–21 Des 2014) dan testing (22–31 Des 2014)


### Tahap 2 — Membangun dan Melatih Model

- Buat `ForecasterRecursive` dengan regressor `LGBMRegressor`
- Parameter: `lags=7`, `random_state=123`
- Fit model dengan `y=Demand` (target) dan `exog=Temperature` (variabel eksogen)
- Skforecast otomatis membuat matriks lag dan melatih model regresi di atasnya


### Tahap 3 — Feature Importance (LightGBM Bawaan)

Menggunakan atribut `feature_importances_` dari LGBMRegressor:


| Fitur | Importance |
| :-- | :-- |
| Temperature | 570 |
| lag_1 | 470 |
| lag_3 | 387 |
| lag_2 | 362 |
| lag_7 | 325 |
| lag_6 | 313 |
| lag_5 | 298 |
| lag_4 | 275 |

**Kesimpulan:** Temperature dan lag_1 adalah dua fitur paling dominan.

### Tahap 4 — SHAP Values

Menggunakan `shap.TreeExplainer(forecaster.estimator)` untuk menjelaskan kontribusi setiap fitur terhadap setiap prediksi secara individual.

Empat visualisasi yang dihasilkan:


| Plot | Fungsi |
| :-- | :-- |
| **Summary Bar** | Ranking kepentingan fitur secara global |
| **Summary Dot (Beeswarm)** | Arah pengaruh fitur (positif/negatif) dan distribusinya |
| **Waterfall** | Penjelasan detail untuk satu observasi spesifik |
| **Scatter (Dependence)** | Hubungan nilai Temperature vs kontribusi SHAP-nya |

### Tahap 5 — Prediksi 10 Hari ke Depan

```python
prediksi = forecaster.predict(steps=10, exog=data_test['Temperature'])
```

Hasil prediksi:


| Tanggal | Demand (MW) |
| :-- | :-- |
| 22 Des 2014 | 241.514 |
| 23 Des 2014 | 226.165 |
| 24 Des 2014 | 220.506 |
| 25 Des 2014 | 209.260 |
| 26 Des 2014 | 184.885 ← terendah |
| 27 Des 2014 | 195.623 |
| 28 Des 2014 | 222.766 |
| 29 Des 2014 | 223.112 |
| 30 Des 2014 | 219.103 |
| 31 Des 2014 | 217.948 |

Model bekerja **secara rekursif**: prediksi hari ini menjadi `lag_1` untuk prediksi hari berikutnya.

### Tahap 6 — Permutation Importance

Mengacak satu fitur secara acak, lalu mengukur seberapa besar performa model turun.


| Fitur | Mean Importance | Std |
| :-- | :-- | :-- |
| lag_1 | 0.617 | 0.015 |
| Temperature | 0.411 | 0.014 |
| lag_7 | 0.196 | 0.002 |
| lag_2 | 0.122 | 0.008 |
| lag_6 | 0.084 | 0.004 |
| lag_3 | 0.041 | 0.002 |
| lag_5 | 0.031 | 0.001 |
| lag_4 | 0.025 | 0.001 |

**Catatan penting:** Hasil berbeda dengan LightGBM importance — di sini `lag_1` (0.617) lebih krusial dari `Temperature` (0.411). Ini karena permutation importance mengukur **degradasi performa nyata** saat fitur dihilangkan.

### Tahap 7 — Partial Dependence Plot (PDP)

Menggunakan `PartialDependenceDisplay.from_estimator` dari scikit-learn untuk memvisualisasikan pengaruh **murni** satu fitur terhadap prediksi, dengan fitur lain dirata-ratakan. Fitur yang divisualisasikan: `Temperature` dan `lag_1`.

PDP menunjukkan pola marginal: misalnya suhu tinggi → demand cenderung meningkat.

## Kesimpulan

Dari ketiga metode explainability yang digunakan (Feature Importance, SHAP, Permutation Importance), secara konsisten ditemukan bahwa:

1. **Suhu udara (Temperature)** dan **demand hari sebelumnya (lag_1)** adalah dua faktor paling menentukan konsumsi listrik harian di Victoria.
2. Kedua faktor ini saling melengkapi: `Temperature` penting karena cuaca panas/dingin langsung memengaruhi penggunaan AC/pemanas, sementara `lag_1` menangkap inersia pola konsumsi harian.
3. `lag_7` (seminggu lalu) juga cukup penting, mengonfirmasi adanya **pola mingguan** dalam data konsumsi listrik.
