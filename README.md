# 📊 Analisis Konversi Data
### **Implementasi Tiga Metode Utama dalam Python dan R**

![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)
![R](https://img.shields.io/badge/R-4.0+-purple.svg)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)
![RStudio](https://img.shields.io/badge/RStudio-RMarkdown-purple.svg)
![Scikit--Learn](https://img.shields.io/badge/scikit--learn-Preprocessing-red.svg)
![Affiliation](https://img.shields.io/badge/Affiliation-UNTIRTA-orange.svg)

Repositori ini menyajikan modul pembelajaran yang memandu Anda melalui alur kerja komprehensif untuk mengubah format data dalam **Data Science**. Proyek ini mendemonstrasikan implementasi paralel di **Python** dan **R** untuk tiga metode konversi data yang krusial: **Binning**, **Label Encoding**, dan **One-Hot Encoding**.

---

## ✨ Fitur Utama

- **Analisis Dwi-Bahasa**: Menampilkan implementasi yang sama dari metode konversi data dalam dua ekosistem pemrograman terkemuka.
- **Materi Pembelajaran**: Menyajikan definisi, contoh, dan kasus penggunaan untuk setiap metode, menjadikannya sumber daya yang ideal untuk pemula.
- **Implementasi Tiga Metode**: Menerapkan tiga strategi konversi yang berbeda pada **Adult Census Income Dataset**:
    - **Binning**: Mengubah data `age` (kontinyu) menjadi kelompok usia.
    - **Label Encoding**: Mengubah data `education` (ordinal) menjadi nilai numerik berurutan.
    - **One-Hot Encoding**: Mengubah data `sex` (nominal) menjadi kolom binary.
- **Otomasi & Robustness**: Dilengkapi dengan skrip yang secara otomatis menginstal pustaka yang dibutuhkan, memastikan alur kerja dapat berjalan tanpa hambatan teknis.
- **Laporan Reproducibel**: Menyediakan file Jupyter Notebook (`.ipynb`) dan R Markdown (`.Rmd`) yang menghasilkan laporan analisis lengkap dan terstruktur.

---

## 📂 Struktur Proyek

- `Conversion Data.ipynb`: Notebook Python untuk implementasi metode konversi data.
- `Conversion Data.Rmd`: Dokumen R Markdown untuk implementasi metode konversi data.
- `Conversion Data.md`: Laporan analisis hasil *knit* dari file `.Rmd`.

*(Catatan: Proyek ini menggunakan dataset bawaan atau sintetis di Python dan R, sehingga tidak memerlukan file data eksternal.)*

---

## 🔧 Tumpukan Teknologi (Tech Stack)

- **Bahasa**: `Python 3.7+`, `R 4.0+`
- **Pustaka Python**: `pandas`, `numpy`, `scikit-learn`, `seaborn`.
- **Pustaka R**: `dplyr`, `readr`, `fastDummies`, `knitr`, `kableExtra`.
- **Lingkungan**: `Jupyter Notebook / Lab`, `RStudio`.

---

## 🚀 Cara Memulai

### Prasyarat

- Instalasi Python dan R.
- Pustaka yang diperlukan (akan diinstal secara otomatis oleh skrip).
- Jupyter Notebook/Lab atau RStudio.

### Instalasi & Penggunaan

1. **Unduh Repositori**: *Clone* atau unduh proyek ini ke komputer Anda.
2. **Jalankan Notebook/R Markdown**:
    - Untuk proyek **Python**, jalankan Jupyter Lab/Notebook dan buka file `Conversion Data.ipynb`.
    - Untuk proyek **R**, buka RStudio dan buka file `Conversion Data.Rmd` lalu klik "Knit" atau jalankan *chunk* kode.
3. **Pustaka Otomatis**: Semua skrip telah dirancang untuk secara otomatis menginstal pustaka yang diperlukan jika belum terpasang.

---

## 📊 Ringkasan Hasil Utama

| Masalah Konversi Data | Metode Utama | Hasil Utama |
|---|---|---|
| **Data Kontinyu** (`age`) | Binning | Berhasil mengubah data usia menjadi 5 kategori yang mudah diinterpretasikan. |
| **Data Ordinal** (`education`) | Label Encoding | Berhasil mengubah 16 tingkat pendidikan menjadi nilai numerik yang mempertahankan urutan logis (0-15). |
| **Data Nominal** (`sex`) | One-Hot Encoding | Berhasil mengubah 2 kategori jenis kelamin menjadi 2 kolom binary (0 dan 1). |

---

## ⚖️ Lisensi & Ketentuan Penggunaan

- **Hak Cipta**: © 2025 Ferdian Bangkit Wijaya - Seluruh Hak Dilindungi.
- **Penggunaan Akademik**: Diizinkan secara bebas untuk keperluan penelitian dan pendidikan non-komersial dengan atribusi.
- **Penggunaan Komersial**: Memerlukan izin tertulis dari pengembang.

---

## 👨‍💻 Author

- **Ferdian Bangkit Wijaya**
- Universitas Sultan Ageng Tirtayasa (UNTIRTA)
- Banten, Indonesia 🇮🇩

---

> Proyek ini berfungsi sebagai panduan praktis untuk pra-pemrosesan data, menunjukkan bagaimana pendekatan yang terstruktur dan penggunaan bahasa pemrograman yang fleksibel dapat menghasilkan dataset yang bersih dan siap untuk pemodelan machine learning.
