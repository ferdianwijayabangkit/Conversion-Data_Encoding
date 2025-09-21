📊 Modul Pembelajaran: Metode Konversi Data dalam Data Science
================
true
September 21, 2025

- [📊 Modul Pembelajaran: Metode Konversi Data dalam Data
  Science](#bar_chart-modul-pembelajaran-metode-konversi-data-dalam-data-science)
  - [🎯 **Tujuan Pembelajaran**](#dart-tujuan-pembelajaran)
  - [📚 **Materi Konversi Data**](#books-materi-konversi-data)
    - [🗂️ **1.
      Binning/Diskretisasi**](#card_index_dividers-1-binningdiskretisasi)
    - [🏷️ **2. Label Encoding**](#label-2-label-encoding)
    - [🔥 **3. One-Hot Encoding**](#fire-3-one-hot-encoding)
  - [⚖️ **Perbandingan Metode**](#balance_scale-perbandingan-metode)
  - [🎓 **Kasus Studi**](#mortar_board-kasus-studi)
  - [🔧 1. Install dan Load
    Libraries](#wrench-1-install-dan-load-libraries)
  - [📊 2. Load Dataset dan Eksplorasi
    Data](#bar_chart-2-load-dataset-dan-eksplorasi-data)
  - [🎯 3. Pemilihan Kolom untuk
    Konversi](#dart-3-pemilihan-kolom-untuk-konversi)
  - [🗂️ 4. Implementasi
    Binning/Diskretisasi](#card_index_dividers-4-implementasi-binningdiskretisasi)
  - [🏷️ 5. Implementasi Label
    Encoding](#label-5-implementasi-label-encoding)
  - [🔥 6. Implementasi One-Hot
    Encoding](#fire-6-implementasi-one-hot-encoding)
  - [📊 7. Evaluasi dan Gabungan
    Hasil](#bar_chart-7-evaluasi-dan-gabungan-hasil)

# 📊 Modul Pembelajaran: Metode Konversi Data dalam Data Science

## 🎯 **Tujuan Pembelajaran**

Memahami dan mengimplementasikan tiga metode utama konversi data yang
sering digunakan dalam preprocessing data untuk machine learning dan
data analysis.

## 📚 **Materi Konversi Data**

### 🗂️ **1. Binning/Diskretisasi**

**Definisi**: Proses mengubah data kontinyu menjadi data kategorial
dengan membagi nilai ke dalam interval-interval tertentu.

**Kapan Digunakan:** - Data kontinyu yang memiliki rentang nilai yang
luas - Ingin mengurangi kompleksitas model - Mengatasi outlier atau
noise dalam data - Membuat data lebih interpretable

**Contoh**: Usia (17-90 tahun) → Kelompok usia (Muda, Dewasa, Senior)

### 🏷️ **2. Label Encoding**

**Definisi**: Mengubah data kategori ordinal menjadi nilai numerik
berurutan yang mempertahankan hierarki/urutan.

**Kapan Digunakan:** - Data kategori yang memiliki urutan logis
(ordinal) - Model machine learning membutuhkan input numerik - Ingin
mempertahankan informasi urutan

**Contoh**: Pendidikan (SD → SMP → SMA → S1 → S2 → S3) menjadi (0, 1, 2,
3, 4, 5)

### 🔥 **3. One-Hot Encoding**

**Definisi**: Mengubah data kategori nominal menjadi beberapa kolom
binary (0 dan 1), dimana setiap kategori direpresentasikan oleh satu
kolom.

**Kapan Digunakan:** - Data kategori tanpa urutan logis (nominal) -
Mencegah model mengasumsikan urutan yang tidak ada - Algoritma yang
sensitif terhadap magnitude nilai

**Contoh**: Jenis Kelamin (Male, Female) → sex_Male (0/1), sex_Female
(0/1)

## ⚖️ **Perbandingan Metode**

| Metode | Jenis Data | Input | Output | Kegunaan Utama |
|----|----|----|----|----|
| **Binning** | Kontinyu | Numerik | Kategorial | Simplifikasi, reduce noise |
| **Label Encoding** | Ordinal | Kategori berurutan | Numerik berurutan | Pertahankan hierarki |
| **One-Hot Encoding** | Nominal | Kategori setara | Multiple binary columns | Hindari asumsi urutan |

## 🎓 **Kasus Studi**

Dalam notebook ini, kita akan menggunakan **Adult Census Income
Dataset** untuk mempraktikkan ketiga metode konversi data: - **Age**
(kontinyu) → Binning menjadi kelompok usia - **Education** (ordinal) →
Label encoding berdasarkan tingkat pendidikan  
- **Sex** (nominal) → One-hot encoding untuk jenis kelamin

------------------------------------------------------------------------

## 🔧 1. Install dan Load Libraries

``` r
knitr::opts_chunk$set(
  echo = TRUE,
  warning = FALSE,
  message = FALSE,
  fig.width = 12,
  fig.height = 8
)
```

``` r
# Install libraries jika belum ada
install_if_missing <- function(package_name) {
  if (!require(package_name, character.only = TRUE)) {
    install.packages(package_name, dependencies = TRUE)
    library(package_name, character.only = TRUE)
    cat("✅", package_name, "berhasil diinstall dan dimuat\n")
  } else {
    cat("✅", package_name, "sudah terinstall\n")
  }
}

# Install required packages
packages <- c('dplyr', 'readr', 'fastDummies', 'knitr', 'kableExtra')
for (package in packages) {
  install_if_missing(package)
}
```

    ## ✅ dplyr sudah terinstall

    ## ✅ readr sudah terinstall

    ## ✅ fastDummies sudah terinstall

    ## ✅ knitr sudah terinstall

    ## ✅ kableExtra sudah terinstall

``` r
# Load additional libraries
library(dplyr)
library(readr)
library(fastDummies)
library(knitr)
library(kableExtra)

cat("\n🎉 Semua libraries berhasil diimport!\n")
```

    ## 
    ## 🎉 Semua libraries berhasil diimport!

## 📊 2. Load Dataset dan Eksplorasi Data

Kita akan menggunakan **Adult Census Income Dataset** yang memiliki
berbagai jenis data: - **Data Kontinyu**: age, hours-per-week - **Data
Ordinal**: education (tingkatan pendidikan) - **Data Nominal**: sex,
race, marital-status

``` r
# Load Adult Census Income Dataset
# Karena tidak ada akses langsung ke OpenML di R base, kita buat dataset sintetis
cat("📁 Membuat dataset sintetis Adult Census Income...\n")
```

    ## 📁 Membuat dataset sintetis Adult Census Income...

``` r
set.seed(42)
n_samples <- 1000

df <- data.frame(
  age = sample(17:80, n_samples, replace = TRUE),
  sex = sample(c('Male', 'Female'), n_samples, replace = TRUE),
  education = sample(c('Preschool', '1st-4th', '5th-6th', '7th-8th', '9th', 
                      '10th', '11th', '12th', 'HS-grad', 'Some-college', 
                      'Assoc-voc', 'Assoc-acdm', 'Bachelors', 'Masters', 
                      'Prof-school', 'Doctorate'), n_samples, replace = TRUE),
  hours_per_week = sample(1:80, n_samples, replace = TRUE),
  race = sample(c('White', 'Black', 'Asian-Pac-Islander'), n_samples, replace = TRUE),
  marital_status = sample(c('Married-civ-spouse', 'Never-married', 'Divorced'), 
                         n_samples, replace = TRUE),
  stringsAsFactors = FALSE
)

cat(sprintf("\n📏 Dataset shape: %d rows, %d columns\n", nrow(df), ncol(df)))
```

    ## 
    ## 📏 Dataset shape: 1000 rows, 6 columns

``` r
cat("📋 Kolom tersedia:", paste(names(df), collapse = ", "), "\n")
```

    ## 📋 Kolom tersedia: age, sex, education, hours_per_week, race, marital_status

``` r
# Tampilkan sample data
cat("\n📄 Sample Data (5 baris pertama):\n")
```

    ## 
    ## 📄 Sample Data (5 baris pertama):

``` r
kable(head(df, 5), format = "html") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"))
```

<table class="table table-striped table-hover table-condensed" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:right;">

age
</th>

<th style="text-align:left;">

sex
</th>

<th style="text-align:left;">

education
</th>

<th style="text-align:right;">

hours_per_week
</th>

<th style="text-align:left;">

race
</th>

<th style="text-align:left;">

marital_status
</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

65
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:left;">

Prof-school
</td>

<td style="text-align:right;">

24
</td>

<td style="text-align:left;">

White
</td>

<td style="text-align:left;">

Married-civ-spouse
</td>

</tr>

<tr>

<td style="text-align:right;">

53
</td>

<td style="text-align:left;">

Male
</td>

<td style="text-align:left;">

1st-4th
</td>

<td style="text-align:right;">

55
</td>

<td style="text-align:left;">

Asian-Pac-Islander
</td>

<td style="text-align:left;">

Married-civ-spouse
</td>

</tr>

<tr>

<td style="text-align:right;">

17
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:left;">

Bachelors
</td>

<td style="text-align:right;">

44
</td>

<td style="text-align:left;">

Black
</td>

<td style="text-align:left;">

Divorced
</td>

</tr>

<tr>

<td style="text-align:right;">

41
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:left;">

Assoc-voc
</td>

<td style="text-align:right;">

51
</td>

<td style="text-align:left;">

Black
</td>

<td style="text-align:left;">

Divorced
</td>

</tr>

<tr>

<td style="text-align:right;">

26
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

27
</td>

<td style="text-align:left;">

Black
</td>

<td style="text-align:left;">

Divorced
</td>

</tr>

</tbody>

</table>

``` r
# Info dataset
cat("\n📊 Info Dataset:\n")
```

    ## 
    ## 📊 Info Dataset:

``` r
str(df)
```

    ## 'data.frame':    1000 obs. of  6 variables:
    ##  $ age           : int  65 53 17 41 26 52 34 74 65 80 ...
    ##  $ sex           : chr  "Female" "Male" "Female" "Female" ...
    ##  $ education     : chr  "Prof-school" "1st-4th" "Bachelors" "Assoc-voc" ...
    ##  $ hours_per_week: int  24 55 44 51 27 10 27 32 29 53 ...
    ##  $ race          : chr  "White" "Asian-Pac-Islander" "Black" "Black" ...
    ##  $ marital_status: chr  "Married-civ-spouse" "Married-civ-spouse" "Divorced" "Divorced" ...

## 🎯 3. Pemilihan Kolom untuk Konversi

Berdasarkan jenis data, kita akan memilih 3 kolom: 1. **Data Kontinyu**:
`age` - untuk Binning/Diskretisasi 2. **Data Ordinal**: `education` -
untuk Label Encoding 3. **Data Nominal**: `sex` - untuk One-Hot Encoding

Mari kita pastikan tidak ada missing data pada kolom-kolom ini.

``` r
# Pilih kolom untuk konversi
selected_columns <- list(
  kontinyu = 'age',      # Untuk Binning
  ordinal = 'education', # Untuk Label Encoding  
  nominal = 'sex'        # Untuk One-Hot Encoding
)

cat("🔍 Analisis Kolom Terpilih:\n")
```

    ## 🔍 Analisis Kolom Terpilih:

``` r
cat("========================================\n")
```

    ## ========================================

``` r
for (jenis in names(selected_columns)) {
  kolom <- selected_columns[[jenis]]
  cat(sprintf("\n%s - %s:\n", toupper(jenis), kolom))
  cat(sprintf("  Missing values: %d\n", sum(is.na(df[[kolom]]))))
  cat(sprintf("  Unique values: %d\n", length(unique(df[[kolom]]))))
  cat(sprintf("  Sample values: %s\n", paste(head(unique(df[[kolom]]), 5), collapse = ", ")))
}
```

    ## 
    ## KONTINYU - age:
    ##   Missing values: 0
    ##   Unique values: 64
    ##   Sample values: 65, 53, 17, 41, 26
    ## 
    ## ORDINAL - education:
    ##   Missing values: 0
    ##   Unique values: 16
    ##   Sample values: Prof-school, 1st-4th, Bachelors, Assoc-voc, Assoc-acdm
    ## 
    ## NOMINAL - sex:
    ##   Missing values: 0
    ##   Unique values: 2
    ##   Sample values: Female, Male

``` r
# Buat dataframe kerja dengan kolom terpilih
df_work <- df[, unlist(selected_columns)]

# Pastikan tidak ada missing data
cat(sprintf("\n✅ Dataset kerja shape: %d rows, %d columns\n", nrow(df_work), ncol(df_work)))
```

    ## 
    ## ✅ Dataset kerja shape: 1000 rows, 3 columns

``` r
cat("📊 Missing data check:\n")
```

    ## 📊 Missing data check:

``` r
print(sapply(df_work, function(x) sum(is.na(x))))
```

    ##       age education       sex 
    ##         0         0         0

``` r
if (sum(is.na(df_work)) > 0) {
  cat("⚠️ Ada missing data, akan dihapus...\n")
  df_work <- na.omit(df_work)
  cat(sprintf("📏 Shape setelah hapus missing: %d rows, %d columns\n", nrow(df_work), ncol(df_work)))
}

cat("\n📋 Dataset Kerja:\n")
```

    ## 
    ## 📋 Dataset Kerja:

``` r
kable(head(df_work), format = "html") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"))
```

<table class="table table-striped table-hover table-condensed" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:right;">

age
</th>

<th style="text-align:left;">

education
</th>

<th style="text-align:left;">

sex
</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

65
</td>

<td style="text-align:left;">

Prof-school
</td>

<td style="text-align:left;">

Female
</td>

</tr>

<tr>

<td style="text-align:right;">

53
</td>

<td style="text-align:left;">

1st-4th
</td>

<td style="text-align:left;">

Male
</td>

</tr>

<tr>

<td style="text-align:right;">

17
</td>

<td style="text-align:left;">

Bachelors
</td>

<td style="text-align:left;">

Female
</td>

</tr>

<tr>

<td style="text-align:right;">

41
</td>

<td style="text-align:left;">

Assoc-voc
</td>

<td style="text-align:left;">

Female
</td>

</tr>

<tr>

<td style="text-align:right;">

26
</td>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:left;">

Female
</td>

</tr>

<tr>

<td style="text-align:right;">

52
</td>

<td style="text-align:left;">

7th-8th
</td>

<td style="text-align:left;">

Male
</td>

</tr>

</tbody>

</table>

## 🗂️ 4. Implementasi Binning/Diskretisasi

**Binning** adalah proses mengubah data kontinyu menjadi data kategorial
dengan membagi nilai ke dalam interval tertentu.

**Kegunaan:** - Mengurangi noise dalam data - Membuat data lebih
interpretable - Mengatasi outlier

Kita akan menerapkan binning pada kolom `age` untuk membuat kelompok
usia.

``` r
# Implementasi Binning pada kolom 'age'
cat("🗂️ BINNING/DISKRETISASI - Kolom 'age'\n")
```

    ## 🗂️ BINNING/DISKRETISASI - Kolom 'age'

``` r
cat("========================================\n")
```

    ## ========================================

``` r
# Tampilkan statistik age sebelum binning
cat("📊 Statistik Age:\n")
```

    ## 📊 Statistik Age:

``` r
cat(sprintf("  Min: %d\n", min(df_work$age)))
```

    ##   Min: 17

``` r
cat(sprintf("  Max: %d\n", max(df_work$age)))
```

    ##   Max: 80

``` r
cat(sprintf("  Mean: %.1f\n", mean(df_work$age)))
```

    ##   Mean: 49.3

``` r
# Buat bins berdasarkan tahap kehidupan
age_bins <- c(0, 25, 35, 50, 65, 100)
age_labels <- c('Muda (17-25)', 'Dewasa Muda (26-35)', 'Dewasa (36-50)', 
                'Paruh Baya (51-65)', 'Senior (65+)')

# Implementasi binning
df_work$age_binned <- cut(df_work$age, 
                         breaks = age_bins, 
                         labels = age_labels, 
                         include.lowest = TRUE)

cat("\n✅ Binning selesai!\n")
```

    ## 
    ## ✅ Binning selesai!

``` r
cat("📋 Distribusi Age setelah Binning:\n")
```

    ## 📋 Distribusi Age setelah Binning:

``` r
print(table(df_work$age_binned))
```

    ## 
    ##        Muda (17-25) Dewasa Muda (26-35)      Dewasa (36-50)  Paruh Baya (51-65) 
    ##                 118                 139                 264                 254 
    ##        Senior (65+) 
    ##                 225

``` r
# Tampilkan contoh transformasi
cat("\n📄 Contoh Transformasi:\n")
```

    ## 
    ## 📄 Contoh Transformasi:

``` r
sample_data <- df_work[1:10, c('age', 'age_binned')]
kable(sample_data, format = "html") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"))
```

<table class="table table-striped table-hover table-condensed" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:right;">

age
</th>

<th style="text-align:left;">

age_binned
</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

65
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

</tr>

<tr>

<td style="text-align:right;">

53
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

</tr>

<tr>

<td style="text-align:right;">

17
</td>

<td style="text-align:left;">

Muda (17-25)
</td>

</tr>

<tr>

<td style="text-align:right;">

41
</td>

<td style="text-align:left;">

Dewasa (36-50)
</td>

</tr>

<tr>

<td style="text-align:right;">

26
</td>

<td style="text-align:left;">

Dewasa Muda (26-35)
</td>

</tr>

<tr>

<td style="text-align:right;">

52
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

</tr>

<tr>

<td style="text-align:right;">

34
</td>

<td style="text-align:left;">

Dewasa Muda (26-35)
</td>

</tr>

<tr>

<td style="text-align:right;">

74
</td>

<td style="text-align:left;">

Senior (65+)
</td>

</tr>

<tr>

<td style="text-align:right;">

65
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

</tr>

<tr>

<td style="text-align:right;">

80
</td>

<td style="text-align:left;">

Senior (65+)
</td>

</tr>

</tbody>

</table>

## 🏷️ 5. Implementasi Label Encoding

**Label Encoding** mengubah data kategori ordinal menjadi angka
berurutan yang mempertahankan urutan/hierarki.

**Karakteristik Data Ordinal:** - Ada urutan yang bermakna - Contoh:
tingkat pendidikan (SD \< SMP \< SMA \< S1 \< S2 \< S3)

**Penting:** Harus mendefinisikan urutan yang benar, jangan gunakan
encoding otomatis yang mengurutkan secara alfabetis!

``` r
# Implementasi Label Encoding pada kolom 'education'
cat("🏷️ LABEL ENCODING - Kolom 'education'\n")
```

    ## 🏷️ LABEL ENCODING - Kolom 'education'

``` r
cat("========================================\n")
```

    ## ========================================

``` r
# Tampilkan kategori education yang ada
cat("📋 Kategori Education yang ada:\n")
```

    ## 📋 Kategori Education yang ada:

``` r
education_counts <- table(df_work$education)
print(education_counts)
```

    ## 
    ##         10th         11th         12th      1st-4th      5th-6th      7th-8th 
    ##           51           67           44           72           70           62 
    ##          9th   Assoc-acdm    Assoc-voc    Bachelors    Doctorate      HS-grad 
    ##           55           82           58           71           52           63 
    ##      Masters    Preschool  Prof-school Some-college 
    ##           66           59           72           56

``` r
# Definisikan urutan education yang benar (dari rendah ke tinggi)
education_order <- c('Preschool', '1st-4th', '5th-6th', '7th-8th', '9th', 
                    '10th', '11th', '12th', 'HS-grad', 'Some-college', 
                    'Assoc-voc', 'Assoc-acdm', 'Bachelors', 'Masters', 
                    'Prof-school', 'Doctorate')

# Filter hanya education yang ada di data
available_education <- education_order[education_order %in% unique(df_work$education)]

cat("\n📚 Urutan Education yang tersedia:\n")
```

    ## 
    ## 📚 Urutan Education yang tersedia:

``` r
for (i in seq_along(available_education)) {
  cat(sprintf("  %d: %s\n", i-1, available_education[i]))
}
```

    ##   0: Preschool
    ##   1: 1st-4th
    ##   2: 5th-6th
    ##   3: 7th-8th
    ##   4: 9th
    ##   5: 10th
    ##   6: 11th
    ##   7: 12th
    ##   8: HS-grad
    ##   9: Some-college
    ##   10: Assoc-voc
    ##   11: Assoc-acdm
    ##   12: Bachelors
    ##   13: Masters
    ##   14: Prof-school
    ##   15: Doctorate

``` r
# Manual mapping dengan urutan yang benar
education_mapping <- setNames(0:(length(available_education)-1), available_education)

# Implementasi Label Encoding
df_work$education_encoded <- education_mapping[df_work$education]

cat("\n✅ Label Encoding selesai!\n")
```

    ## 
    ## ✅ Label Encoding selesai!

``` r
cat("📋 Mapping Education:\n")
```

    ## 📋 Mapping Education:

``` r
for (edu in names(education_mapping)) {
  code <- education_mapping[edu]
  count <- sum(df_work$education == edu, na.rm = TRUE)
  cat(sprintf("  %d: %s (%d data)\n", code, edu, count))
}
```

    ##   0: Preschool (59 data)
    ##   1: 1st-4th (72 data)
    ##   2: 5th-6th (70 data)
    ##   3: 7th-8th (62 data)
    ##   4: 9th (55 data)
    ##   5: 10th (51 data)
    ##   6: 11th (67 data)
    ##   7: 12th (44 data)
    ##   8: HS-grad (63 data)
    ##   9: Some-college (56 data)
    ##   10: Assoc-voc (58 data)
    ##   11: Assoc-acdm (82 data)
    ##   12: Bachelors (71 data)
    ##   13: Masters (66 data)
    ##   14: Prof-school (72 data)
    ##   15: Doctorate (52 data)

``` r
# Tampilkan contoh transformasi
cat("\n📄 Contoh Transformasi:\n")
```

    ## 
    ## 📄 Contoh Transformasi:

``` r
sample_data <- df_work[1:10, c('education', 'education_encoded')]
kable(sample_data, format = "html") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"))
```

<table class="table table-striped table-hover table-condensed" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

education
</th>

<th style="text-align:right;">

education_encoded
</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Prof-school
</td>

<td style="text-align:right;">

14
</td>

</tr>

<tr>

<td style="text-align:left;">

1st-4th
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:left;">

Bachelors
</td>

<td style="text-align:right;">

12
</td>

</tr>

<tr>

<td style="text-align:left;">

Assoc-voc
</td>

<td style="text-align:right;">

10
</td>

</tr>

<tr>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

11
</td>

</tr>

<tr>

<td style="text-align:left;">

7th-8th
</td>

<td style="text-align:right;">

3
</td>

</tr>

<tr>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

11
</td>

</tr>

<tr>

<td style="text-align:left;">

11th
</td>

<td style="text-align:right;">

6
</td>

</tr>

<tr>

<td style="text-align:left;">

Prof-school
</td>

<td style="text-align:right;">

14
</td>

</tr>

<tr>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

11
</td>

</tr>

</tbody>

</table>

## 🔥 6. Implementasi One-Hot Encoding

**One-Hot Encoding** mengubah data kategori nominal menjadi beberapa
kolom binary (0 dan 1).

**Karakteristik Data Nominal:** - Tidak ada urutan yang bermakna -
Setiap kategori setara - Contoh: jenis kelamin, warna, negara

**Keuntungan:** Tidak ada asumsi urutan, setiap kategori diperlakukan
adil  
**Kerugian:** Meningkatkan dimensi data (1 kolom → n kolom)

``` r
# Implementasi One-Hot Encoding pada kolom 'sex'
cat("🔥 ONE-HOT ENCODING - Kolom 'sex'\n")
```

    ## 🔥 ONE-HOT ENCODING - Kolom 'sex'

``` r
cat("========================================\n")
```

    ## ========================================

``` r
# Tampilkan kategori sex yang ada
cat("📋 Kategori Sex yang ada:\n")
```

    ## 📋 Kategori Sex yang ada:

``` r
sex_counts <- table(df_work$sex)
print(sex_counts)
```

    ## 
    ## Female   Male 
    ##    482    518

``` r
# Implementasi One-Hot Encoding menggunakan fastDummies
sex_onehot <- dummy_cols(df_work['sex'], select_columns = 'sex', remove_first_dummy = FALSE)
sex_onehot <- sex_onehot[, !names(sex_onehot) %in% 'sex']  # Hapus kolom asli

cat("\n✅ One-Hot Encoding selesai!\n")
```

    ## 
    ## ✅ One-Hot Encoding selesai!

``` r
cat("📊 Hasil transformasi:\n")
```

    ## 📊 Hasil transformasi:

``` r
cat(sprintf("  Kolom asli: 1 kolom ('sex')\n"))
```

    ##   Kolom asli: 1 kolom ('sex')

``` r
cat(sprintf("  Kolom baru: %d kolom\n", ncol(sex_onehot)))
```

    ##   Kolom baru: 2 kolom

``` r
cat(sprintf("  Kolom yang dibuat: %s\n", paste(names(sex_onehot), collapse = ", ")))
```

    ##   Kolom yang dibuat: sex_Female, sex_Male

``` r
# Gabungkan dengan dataframe kerja
df_work <- cbind(df_work, sex_onehot)

cat("\n📄 Contoh Transformasi:\n")
```

    ## 
    ## 📄 Contoh Transformasi:

``` r
sample_data <- df_work[1:10, c('sex', names(sex_onehot))]
kable(sample_data, format = "html") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"))
```

<table class="table table-striped table-hover table-condensed" style="margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:left;">

sex
</th>

<th style="text-align:right;">

sex_Female
</th>

<th style="text-align:right;">

sex_Male
</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

</tbody>

</table>

``` r
# Verifikasi: setiap baris harus memiliki tepat satu nilai 1
cat("\n🔍 Verifikasi One-Hot Encoding:\n")
```

    ## 
    ## 🔍 Verifikasi One-Hot Encoding:

``` r
cat("Setiap baris harus sum = 1:\n")
```

    ## Setiap baris harus sum = 1:

``` r
row_sums <- rowSums(sex_onehot)
cat(sprintf("Min sum: %d, Max sum: %d\n", min(row_sums), max(row_sums)))
```

    ## Min sum: 1, Max sum: 1

``` r
if (all(row_sums == 1)) {
  cat("✅ Verifikasi berhasil!\n")
} else {
  cat("❌ Ada masalah!\n")
}
```

    ## ✅ Verifikasi berhasil!

## 📊 7. Evaluasi dan Gabungan Hasil

Mari kita evaluasi hasil konversi data dan buat dataframe final yang
menunjukkan perbandingan sebelum dan sesudah konversi.

**Ringkasan Metode:** - **Binning**: age (kontinyu) → age_binned
(kategorial) - **Label Encoding**: education (ordinal) →
education_encoded (numerik) - **One-Hot Encoding**: sex (nominal) →
sex_Female, sex_Male (binary)

``` r
# Evaluasi dan Gabungan Hasil Konversi Data
cat("EVALUASI HASIL KONVERSI DATA\n")
```

    ## EVALUASI HASIL KONVERSI DATA

``` r
cat("==================================================\n")
```

    ## ==================================================

``` r
# Buat dataframe final dengan kolom sebelum dan sesudah konversi berurutan
df_final <- data.frame(
  age_original = df_work$age,
  age_binned = as.character(df_work$age_binned),
  education_original = df_work$education,
  education_encoded = as.integer(df_work$education_encoded),
  sex_original = df_work$sex,
  stringsAsFactors = FALSE
)

# Tambahkan kolom one-hot encoding
onehot_cols <- names(sex_onehot)
for (col in onehot_cols) {
  df_final[[col]] <- as.integer(df_work[[col]])
}

cat("Dataset final berhasil dibuat!\n")
```

    ## Dataset final berhasil dibuat!

``` r
cat(sprintf("Shape dataset final: %d rows, %d columns\n", nrow(df_final), ncol(df_final)))
```

    ## Shape dataset final: 1000 rows, 7 columns

``` r
cat("Kolom:", paste(names(df_final), collapse = ", "), "\n")
```

    ## Kolom: age_original, age_binned, education_original, education_encoded, sex_original, sex_Female, sex_Male

``` r
# Verifikasi urutan education
cat("\nVERIFIKASI URUTAN EDUCATION:\n")
```

    ## 
    ## VERIFIKASI URUTAN EDUCATION:

``` r
cat("----------------------------------------\n")
```

    ## ----------------------------------------

``` r
cat("Urutan yang didefinisikan:\n")
```

    ## Urutan yang didefinisikan:

``` r
for (i in seq_along(available_education)) {
  edu <- available_education[i]
  count <- sum(df_final$education_original == edu, na.rm = TRUE)
  if (count > 0) {
    cat(sprintf("  %d: %s (%d data)\n", i-1, edu, count))
  }
}
```

    ##   0: Preschool (59 data)
    ##   1: 1st-4th (72 data)
    ##   2: 5th-6th (70 data)
    ##   3: 7th-8th (62 data)
    ##   4: 9th (55 data)
    ##   5: 10th (51 data)
    ##   6: 11th (67 data)
    ##   7: 12th (44 data)
    ##   8: HS-grad (63 data)
    ##   9: Some-college (56 data)
    ##   10: Assoc-voc (58 data)
    ##   11: Assoc-acdm (82 data)
    ##   12: Bachelors (71 data)
    ##   13: Masters (66 data)
    ##   14: Prof-school (72 data)
    ##   15: Doctorate (52 data)

``` r
education_unique <- unique(df_final$education_original)
cat(sprintf("\nEducation yang ada di data: %d kategori\n", length(education_unique)))
```

    ## 
    ## Education yang ada di data: 16 kategori

``` r
cat("Urutan berdasarkan tingkat pendidikan (rendah ke tinggi):\n")
```

    ## Urutan berdasarkan tingkat pendidikan (rendah ke tinggi):

``` r
cat("Urutan sudah benar dari Preschool sampai Doctorate\n")
```

    ## Urutan sudah benar dari Preschool sampai Doctorate

``` r
# Tampilkan ringkasan konversi
cat("\nRINGKASAN KONVERSI:\n")
```

    ## 
    ## RINGKASAN KONVERSI:

``` r
cat("------------------------------\n")
```

    ## ------------------------------

``` r
cat("1. BINNING:\n")
```

    ## 1. BINNING:

``` r
cat(sprintf("   age_original (kontinyu) -> age_binned (kategorial)\n"))
```

    ##    age_original (kontinyu) -> age_binned (kategorial)

``` r
cat(sprintf("   Range: %d-%d -> %d kategori\n", 
           min(df_final$age_original), max(df_final$age_original), 
           length(unique(df_final$age_binned))))
```

    ##    Range: 17-80 -> 5 kategori

``` r
cat("\n2. LABEL ENCODING:\n")
```

    ## 
    ## 2. LABEL ENCODING:

``` r
cat(sprintf("   education_original (ordinal) -> education_encoded (numerik)\n"))
```

    ##    education_original (ordinal) -> education_encoded (numerik)

``` r
cat(sprintf("   Kategori: %d -> Range 0-%d\n", 
           length(unique(df_final$education_original)), 
           max(df_final$education_encoded, na.rm = TRUE)))
```

    ##    Kategori: 16 -> Range 0-15

``` r
cat("\n3. ONE-HOT ENCODING:\n")
```

    ## 
    ## 3. ONE-HOT ENCODING:

``` r
cat(sprintf("   sex_original (nominal) -> %d kolom binary\n", length(onehot_cols)))
```

    ##    sex_original (nominal) -> 2 kolom binary

``` r
cat(sprintf("   Kategori: %d -> %d kolom (0/1)\n", 
           length(unique(df_final$sex_original)), length(onehot_cols)))
```

    ##    Kategori: 2 -> 2 kolom (0/1)

``` r
# Tampilkan dataframe dalam format tabel yang rapi
cat("\nTABEL HASIL KONVERSI DATA (20 baris pertama)\n")
```

    ## 
    ## TABEL HASIL KONVERSI DATA (20 baris pertama)

``` r
cat("================================================================================\n")
```

    ## ================================================================================

``` r
cat("PERBANDINGAN SEBELUM DAN SESUDAH KONVERSI:\n")
```

    ## PERBANDINGAN SEBELUM DAN SESUDAH KONVERSI:

``` r
cat("--------------------------------------------------------------------------------\n")
```

    ## --------------------------------------------------------------------------------

``` r
# Tampilkan 20 baris dalam format tabel
display_df <- head(df_final, 20)
kable(display_df, format = "html") %>%
  kable_styling(bootstrap_options = c("striped", "hover", "condensed"),
                full_width = FALSE) %>%
  scroll_box(width = "100%", height = "400px")
```

<div style="border: 1px solid #ddd; padding: 0px; overflow-y: scroll; height:400px; overflow-x: scroll; width:100%; ">

<table class="table table-striped table-hover table-condensed" style="width: auto !important; margin-left: auto; margin-right: auto;">

<thead>

<tr>

<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">

age_original
</th>

<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">

age_binned
</th>

<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">

education_original
</th>

<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">

education_encoded
</th>

<th style="text-align:left;position: sticky; top:0; background-color: #FFFFFF;">

sex_original
</th>

<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">

sex_Female
</th>

<th style="text-align:right;position: sticky; top:0; background-color: #FFFFFF;">

sex_Male
</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:right;">

65
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

Prof-school
</td>

<td style="text-align:right;">

14
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

53
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

1st-4th
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:right;">

17
</td>

<td style="text-align:left;">

Muda (17-25)
</td>

<td style="text-align:left;">

Bachelors
</td>

<td style="text-align:right;">

12
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

41
</td>

<td style="text-align:left;">

Dewasa (36-50)
</td>

<td style="text-align:left;">

Assoc-voc
</td>

<td style="text-align:right;">

10
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

26
</td>

<td style="text-align:left;">

Dewasa Muda (26-35)
</td>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

11
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

52
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

7th-8th
</td>

<td style="text-align:right;">

3
</td>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:right;">

34
</td>

<td style="text-align:left;">

Dewasa Muda (26-35)
</td>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

11
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

74
</td>

<td style="text-align:left;">

Senior (65+)
</td>

<td style="text-align:left;">

11th
</td>

<td style="text-align:right;">

6
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

65
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

Prof-school
</td>

<td style="text-align:right;">

14
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

80
</td>

<td style="text-align:left;">

Senior (65+)
</td>

<td style="text-align:left;">

Assoc-acdm
</td>

<td style="text-align:right;">

11
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

63
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

Bachelors
</td>

<td style="text-align:right;">

12
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

40
</td>

<td style="text-align:left;">

Dewasa (36-50)
</td>

<td style="text-align:left;">

5th-6th
</td>

<td style="text-align:right;">

2
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

23
</td>

<td style="text-align:left;">

Muda (17-25)
</td>

<td style="text-align:left;">

Bachelors
</td>

<td style="text-align:right;">

12
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

52
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

Some-college
</td>

<td style="text-align:right;">

9
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

41
</td>

<td style="text-align:left;">

Dewasa (36-50)
</td>

<td style="text-align:left;">

Doctorate
</td>

<td style="text-align:right;">

15
</td>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:right;">

53
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

Some-college
</td>

<td style="text-align:right;">

9
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

62
</td>

<td style="text-align:left;">

Paruh Baya (51-65)
</td>

<td style="text-align:left;">

Preschool
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

36
</td>

<td style="text-align:left;">

Dewasa (36-50)
</td>

<td style="text-align:left;">

Doctorate
</td>

<td style="text-align:right;">

15
</td>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

<tr>

<td style="text-align:right;">

42
</td>

<td style="text-align:left;">

Dewasa (36-50)
</td>

<td style="text-align:left;">

HS-grad
</td>

<td style="text-align:right;">

8
</td>

<td style="text-align:left;">

Female
</td>

<td style="text-align:right;">

1
</td>

<td style="text-align:right;">

0
</td>

</tr>

<tr>

<td style="text-align:right;">

66
</td>

<td style="text-align:left;">

Senior (65+)
</td>

<td style="text-align:left;">

5th-6th
</td>

<td style="text-align:right;">

2
</td>

<td style="text-align:left;">

Male
</td>

<td style="text-align:right;">

0
</td>

<td style="text-align:right;">

1
</td>

</tr>

</tbody>

</table>

</div>

------------------------------------------------------------------------

**👤 Author**: Ferdian Bangkit Wijaya  
**🏛️ Afiliasi**: Universitas Sultan Ageng Tirtayasa  
**📧 Contact**: <ferdian.bangkit@untirta.ac.id>  
**📅 Last Updated**: September 2025
