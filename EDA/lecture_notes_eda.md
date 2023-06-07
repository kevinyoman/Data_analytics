# Summary Exploratory Data Analysis

## Introduction to EDA

Exploratory Data Analysis (EDA) adalah suatu proses untuk melakukan eksplorasi lebih jauh terhadap data, seperti melihat struktur data dan sebaran data. Hal ini dapat membantu menentukan apakah teknik statistik yang Anda pertimbangkan untuk analisis data sudah sesuai.

Tujuan utama EDA adalah untuk membantu melihat data sebelum membuat asumsi apa pun.

- Mengidentifikasi dan memahami pola dalam data
- Mendeteksi adanya kejadian anomali
- Menemukan hubungan yang menarik antara variabel

Seorang data analyst dan data scientist dapat menggunakan analisis eksplorasi untuk:

- Memastikan hasil valid dan berlaku untuk tujuan bisnis yang diinginkan
- Membantu pemangku kepentingan mengambil keputusan yang tepat
- Melanjutkan ke tahapan analisis yang lebih dalam, misalnya untuk pemodelan machine learning (predictive)

Beberapa teknik yang dilakukan pada proses EDA:

- `.head()` dan `.tail()` untuk inspeksi data
- `.describe()` untuk mendeskripsikan data secara statistik
- `.shape` dan `.size` untuk cek dimensi data
- `.axes` untuk cek label index kolom dan baris
- `.dtypes` untuk cek tipe data
- `.info()` untuk melihat informasi data secara detail
- Membuat tabel frekuensi dan tabel agregasi
- Pengecekan missing value dan duplicated data

## Datetime Data Type

`datetime64` merupakan tipe data dari library `pandas` yang digunakan untuk menyimpan data tanggal dan waktu. Alasan sebuah kolom berisi data tanggal dan waktu perlu untuk diubah ke tipe data ini, yaitu:

- Memiliki accessor nya sendiri, yaitu `.dt`, yang digunakan untuk mengakses method dan attribute yang khusus dimiliki dan hanya bisa digunakan oleh tipe data ini.
- Dapat mengekstrak informasi/komponennya seperti tanggal, bulan, atau tahun, dan dapat melakukan transformasi menjadi periode waktu tertentu.
- Dapat dilakukan operasi seperti menghitung selisih antar dua rentang waktu (`timedelta`).

### Convert to Datetime

Terdapat tiga cara untuk melakukan konversi ke tipe data `datetime64`:

- Method `.astype('datetime64')`: ketika data tanggal yang dimiliki formatnya bulan-tanggal-tahun (**month first**).
- Method `pd.to_datetime()`: ketika ada parameter yang ingin kita tambahkan, atau jika format data tanggalnya adalah **selain** bulan-tanggal-tahun.
    + Tambahkan parameter `dayfirst=True` jika data tanggal memiliki format **day first**.
- Parameter `parse_dates=['nama_kolom']`: merupakan parameter pada fungsi `pd.read_csv()`, digunakan ketika kita sudah tahu kolom mana yang ingin diubah menjadi `datetime64`.

### Datetime Partition

Ketika sebuah kolom sudah menjadi `datetime64`, kita dapat mengambil bagian waktu lebih spesifik seperti tahun, bulan, hari, dan jam.

**Date component (numeric)**

- `.dt.year` untuk komponen tahun
- `.dt.month` untuk komponen bulan (dalam angka)
- `.dt.day` untuk komponen tanggal (dalam angka)
- `.dt.dayofweek` untuk ekstrak index hari (0: Senin - 6: Minggu)

**Date component (string)**

- `.dt.month_name()` untuk komponen nama bulan
- `.dt.day_name()` untuk komponen nama hari

**Time component**

- `.dt.hour` untuk komponen jam
- `.dt.minute` untuk komponen menit
- `.dt.second` untuk komponen detik

> [Dokumentasi: datetime properties](https://pandas.pydata.org/pandas-docs/stable/reference/series.html#datetimelike-properties)

### Datetime Transformation

Selain digunakan untuk melakukan partisi, kita juga dapat melakukan transformasi object `datetime64` ke dalam format periode menggunakan method `.to_period()`.

- `.dt.to_period('D')` untuk mengubah ke format **D**aily (tanggal lengkap)
- `.dt.to_period('W')` untuk mengubah ke format **W**eekly (awal dan akhir minggu)
- `.dt.to_period('M')` untuk mengubah ke format **M**onthly (year-month)
- `.dt.to_period('Q')` untuk mengubah ke format **Q**uarterly (year-quarter)

## Category Data Type

Tipe data `category` digunakan untuk menyimpan data dengan nilai yang berulang, dengan kata lain jumlah uniknya sedikit. Ketika kita belum mengetahui kolom mana yang dapat diubah ke dalam tipe data ini, kita dapat menggunakan method `.nunique()` untuk melihat banyaknya nilai yang unik dari sebuah kolom atau dataframe.

Alasan sebuah kolom perlu diubah ke tipe data `category`, yaitu:

- Memory efficient
- Memiliki accessor nya sendiri, yaitu `.cat`, yang digunakan untuk mengakses method dan attribute yang khusus dimiliki dan hanya bisa digunakan oleh tipe data ini.

> [Dokumentasi: categorical accessor](https://pandas.pydata.org/pandas-docs/stable/reference/series.html#categorical-accessor)

## Contingency/Frequency Table

Tabel kontingensi digunakan untuk menghitung nilai frekuensi/kemunculan data.

- Method `.value_counts()` untuk menghitung jumlah baris pada setiap category dalam **1 kolom**, dan secara default diurutkan descending. Parameter yang dapat diatur:
    + `sort=False`: mencegah nilai pengurutan apa pun, urutkan berdasarkan indeks sebagai gantinya (default: `sort=True`)
    + `ascending=True`: urutkan nilai dalam urutan menaik (default: `ascending=False`)
- Method `pd.crosstab()` digunakan ketika ingin membuat sebuah frequency table dari lebih dari 1 kolom. Parameter yang dapat diatur:
    + `index`: kolom yang akan dijadikan pengelompokkan pada baris (axis 0)
    + `columns`: kolom yang akan dijadikan pengelompokkan pada kolom (axis 1)
    + `margins=True`: menambahkan baris atau kolom margins yang menampung nilai subtotal
    + `normalize`: membagi keseluruhan nilai hasil crosstab dengan jumlah nilai. Dapat diisikan dengan beberapa nilai:
        - `normalize='all'` atau `normalize=True` untuk melakukan normalisasi untuk keseluruhan nilai
        - `normalize='index'` melakukan normalisasi pada setiap baris
        - `normalize='columns'` melakukan normalisasi pada setiap kolom
        
## Aggregation Table

- Method `pd.crosstab()` selain bisa digunakan untuk membuat tabel frekuensi, bisa juga digunakan untuk membuat tabel agregasi dari suatu nilai. Parameter yang perlu ditambahkan untuk membuat tabel agregasi:
    + `values`: diisi dengan kolom yang ingin dihitung menggunakan fungsi agregasi
    + `aggfunc`: diisi dengan fungsi agregasi yang ingin digunakan, misal: *mean*, *median*, *count* (tabel frekuensi), *sum* dll.
- Method `pd.pivot_table()` merupakan alternatif dari method crosstab. Perbedaannya adalah kita tidak perlu menuliskan dataframe yang digunakan berkali-kali. Terdapat 2 jenis penulisan untuk method ini:
    1. Menambahkan parameter 'data' untuk mendefinisikan dataframe yang digunakan: `pd.pivot_table(data=..., index=..., columns=..., values=..., aggfunc=...)`
    2. Menggunakan pivot_table sebagai method dari dataframe: `data.pivot_table(index=..., columns=..., values=..., aggfunc=...)`