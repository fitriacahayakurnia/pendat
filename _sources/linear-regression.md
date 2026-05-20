# Analisis Data Menggunakan Linear Regression

## 1. Data yang Digunakan

$$
\begin{array}{|c|c|c|}
\hline
\text{Titik} & X & Y \\
\hline
A & 2 & 2 \\
B & 4 & 3 \\
C & 3 & 5 \\
D & 3 & 4 \\
E & 3 & 3 \\
F & 4 & 5 \\
G & 5 & 6 \\
\hline
\end{array}
$$

Model regresi linear:

$$
y = \beta_0 + \beta_1 x
$$

Rumus estimasi koefisien:

$$
\hat{\beta} = (X^T X)^{-1} X^T Y
$$

## 2. Perhitungan Manual

### 2.1 Membentuk Matriks $X$ dan $Y$

Matriks $X$:

$$
X =
\begin{bmatrix}
1 & 2 \\
1 & 4 \\
1 & 3 \\
1 & 3 \\
1 & 3 \\
1 & 4 \\
1 & 5
\end{bmatrix}
$$

Matriks $Y$:

$$
Y =
\begin{bmatrix}
2 \\
3 \\
5 \\
4 \\
3 \\
5 \\
6
\end{bmatrix}
$$

### 2.2 Menghitung Transpose Matriks $X^T$

$$
X^T =
\begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 \\
2 & 4 & 3 & 3 & 3 & 4 & 5
\end{bmatrix}
$$

### 2.3 Menghitung $X^T X$

$$
X^T X =
\begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 \\
2 & 4 & 3 & 3 & 3 & 4 & 5
\end{bmatrix}
\begin{bmatrix}
1 & 2 \\
1 & 4 \\
1 & 3 \\
1 & 3 \\
1 & 3 \\
1 & 4 \\
1 & 5
\end{bmatrix}
$$

Perhitungan:

$$
=\begin{bmatrix}
7 & 24 \\
24 & 88
\end{bmatrix}
$$

### 2.4 Menghitung Inverse $(X^T X)^{-1}$

Rumus inverse matriks:

$$
A^{-1}=
\frac{1}{ad-bc}
\begin{bmatrix}
d & -b \\
-c & a
\end{bmatrix}
$$

#### Determinan

$$
\det =
(7)(88) - (24)(24)
$$

$$
= 616 - 576
$$

$$
= 40
$$

#### Inverse Matriks

$$
(X^T X)^{-1}=
\frac{1}{40}
\begin{bmatrix}
88 & -24 \\
-24 & 7
\end{bmatrix}
$$

Hasil:

$$
(X^T X)^{-1}=
\begin{bmatrix}
2.2 & -0.6 \\
-0.6 & 0.175
\end{bmatrix}
$$

### 2.5 Menghitung $X^T Y$

$$
X^T Y =
\begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 \\
2 & 4 & 3 & 3 & 3 & 4 & 5
\end{bmatrix}
\begin{bmatrix}
2 \\
3 \\
5 \\
4 \\
3 \\
5 \\
6
\end{bmatrix}
$$

Perhitungan:

$$
=
\begin{bmatrix}
28 \\
102
\end{bmatrix}
$$

### 2.6 Menghitung Koefisien $\hat{\beta}$

$$
\hat{\beta}=
\begin{bmatrix}
2.2 & -0.6 \\
-0.6 & 0.175
\end{bmatrix}
\begin{bmatrix}
28 \\
102
\end{bmatrix}
$$

Perhitungan:

$$
=\begin{bmatrix}
0.4 \\
1.05
\end{bmatrix}
$$

### 2.7 Persamaan Regresi

Diperoleh:

$$
\beta_0 = 0.4
$$

$$
\beta_1 = 1.05
$$

Maka persamaan regresinya adalah:

$$
y = 0.4 + 1.05x
$$

### 2.8 Menentukan Titik pada Garis Regresi

Persamaan regresi yang diperoleh:

$$
y = 0.4 + 1.05x
$$

#### Menentukan Titik $H$

Diketahui:

$$
x = 0
$$

Substitusi ke persamaan regresi:

$$
y = 0.4 + 1.05(0)
$$

$$
y = 0.4 + 0
$$

$$
y = 0.4
$$

Maka diperoleh titik:

$$
H = (0,\ 0.4)
$$

### Menentukan Titik $I$

Diketahui:

$$
x = 2
$$

Substitusi ke persamaan regresi:

$$
y = 0.4 + 1.05(2)
$$

$$
y = 0.4 + 2.1
$$

$$
y = 2.5
$$

Maka diperoleh titik:

$$
I = (2,\ 2.5)
$$

### 2.9 Titik-Titik Garis Regresi

Beberapa titik yang berada pada garis regresi:

$$
\begin{array}{|c|c|c|}
\hline
X & Y = 0.4 + 1.05X & \text{Titik} \\
\hline
0 & 0.4 & (0,\ 0.4) \\
1 & 1.45 & (1,\ 1.45) \\
2 & 2.5 & (2,\ 2.5) \\
3 & 3.55 & (3,\ 3.55) \\
4 & 4.6 & (4,\ 4.6) \\
5 & 5.65 & (5,\ 5.65) \\
\hline
\end{array}
$$

## 3. Hasil GeoGebra Classic

Berdasarkan hasil visualisasi menggunakan GeoGebra Classic, diperoleh garis regresi linear seperti gambar berikut:
![original image](https://cdn.mathpix.com/snip/images/2GVG2My9lqxuW81ri7LUEUSKr9HIFn0KI_hlf_AvJEs.original.fullsize.png)

Garis regresi tersebut menunjukkan adanya hubungan positif antara variabel X dan Y. Hal ini terlihat dari nilai koefisien regresi β1=1.05 yang bernilai positif, sehingga setiap kenaikan nilai X sebesar 1 satuan akan meningkatkan nilai Y sekitar 1.05 satuan.

Pada grafik GeoGebra, titik-titik data A sampai G tersebar di sekitar garis regresi. Sebagian besar titik berada cukup dekat dengan garis, sehingga model regresi linear dapat menggambarkan pola data dengan baik.

Titik H=(0,0.4) merupakan titik potong garis regresi terhadap sumbu Y, sedangkan titik I=(2,2.5) merupakan salah satu titik hasil prediksi dari persamaan regresi.

Dari hasil perhitungan manual, program Python, dan visualisasi GeoGebra, diperoleh hasil yang konsisten yaitu persamaan regresi:

y=0.4+1.05x

Sehingga dapat disimpulkan bahwa metode regresi linear berhasil digunakan untuk memodelkan hubungan antara data X dan Y.

## 4. Perhitungan Menggunakan Python

```
# IMPORT LIBRARY
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split, KFold, cross_val_score
from sklearn import metrics
import statsmodels.formula.api as smf

# visualization
import seaborn as sns
import matplotlib.pyplot as plt


# MEMBUAT DATA
data = pd.DataFrame({
    'X': [2, 4, 3, 3, 3, 4, 5],
    'Y': [2, 3, 5, 4, 3, 5, 6]
})

print(data)

# PERHITUNGAN MANUAL
n = len(data)

sum_x = data['X'].sum()
sum_y = data['Y'].sum()

sum_x2 = (data['X'] ** 2).sum()

sum_xy = (data['X'] * data['Y']).sum()

print("\nJumlah X =", sum_x)
print("Jumlah Y =", sum_y)
print("Jumlah X^2 =", sum_x2)
print("Jumlah XY =", sum_xy)

# MENGHITUNG BETA 1
beta1 = (
    (n * sum_xy) - (sum_x * sum_y)
) / (
    (n * sum_x2) - (sum_x ** 2)
)

print("\nBeta 1 =", beta1)

# MENGHITUNG BETA 0
beta0 = (
    sum_y - (beta1 * sum_x)
) / n

print("Beta 0 =", beta0)

# PERSAMAAN REGRESI
print("\nPersamaan Regresi")
print(f"Y = {beta0:.2f} + {beta1:.2f}X")

# STATSMODELS
lm = smf.ols(formula='Y ~ X', data=data).fit()

print("\nKoefisien dari Statsmodels")
print(lm.params)

#VISUALISASI
sns.scatterplot(x='X', y='Y', data=data)

# garis regresi
y_pred = beta0 + beta1 * data['X']

plt.plot(data['X'], y_pred)

plt.title("Linear Regression")
plt.xlabel("X")
plt.ylabel("Y")

plt.show()

```

Dengan Output:

![original image](https://cdn.mathpix.com/snip/images/eTd95KJcSHyATcyEFcX5ENCjnlkU-STDEnXvgfs7ubI.original.fullsize.png)

