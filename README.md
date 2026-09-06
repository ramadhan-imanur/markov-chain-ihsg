# Pemodelan Rantai Markov untuk Pergerakan IHSG

Proyek ini bertujuan untuk memodelkan perubahan Indeks Harga Saham Gabungan (IHSG) menggunakan pendekatan Rantai Markov (*Markov Chain*) diskrit dengan tiga keadaan: **Turun**, **Stabil**, dan **Naik**. Keadaan ini ditentukan berdasarkan persentase *return* harian dari IHSG.

## 📂 Struktur Direktori

```text
.
├── code/                              <- Berisi source code, dataset, dan eksperimen
│   ├── data/
│   │   └── raw/
│   │       └── ihsg_daily.csv         <- Data historis harian IHSG
│   └── notebooks/
│       ├── 01_data_collection.ipynb       <- Pengumpulan data (Yahoo Finance) & Preprocessing
│       └── 02_markov_chain_modeling.ipynb <- Pemodelan Rantai Markov & Analisis Probabilitas
├── report/                            <- Laporan penelitian 
│   └── laporan.pdf                    <- Laporan akhir penelitian
├── .gitignore                         <- File yang diabaikan oleh Git
└── README.md                          <- Dokumentasi proyek
```

*(Catatan: Folder `deskripsi`, `.raw`, serta file internal laporan seperti `laporan.tex` tidak diunggah ke repositori ini sesuai dengan kebijakan proyek.)*

## 📊 Metodologi

1. **Pengumpulan Data**: Data IHSG diunduh langsung dari Yahoo Finance.
2. **Preprocessing**: IHSG yang merupakan data kontinu diubah menjadi persentase return harian:
   $$ R_t = \frac{I_t - I_{t-1}}{I_{t-1}} \times 100\% $$
3. **Pemodelan Markov Chain**: 
   - Mengestimasi probabilitas transisi antar keadaan menggunakan data historis.
   - Mengasumsikan probabilitas keadaan di masa depan hanya bergantung pada keadaan saat ini (Sifat Markov).
4. **Prediksi Probabilitas**: Menghitung probabilitas pergerakan IHSG pada dua ($n=2$) dan lima ($n=5$) hari perdagangan berikutnya.

## 🚀 Cara Menjalankan Proyek

1. **Clone repository ini**
   ```bash
   git clone <url-repo-anda>
   cd <nama-folder-repo>
   ```

2. **Install library yang dibutuhkan**
   Proyek ini memerlukan library Python standar untuk analisis data:
   ```bash
   pip install pandas numpy matplotlib seaborn yfinance jupyter
   ```

3. **Jalankan Jupyter Notebook**
   Buka direktori `code/notebooks/` dan jalankan notebook secara berurutan:
   - Mulai dari `01_data_collection.ipynb` untuk mengambil dan memperbarui data IHSG terbaru.
   - Lanjutkan ke `02_markov_chain_modeling.ipynb` untuk melihat hasil analisis pemodelan Rantai Markov.

## 👥 Penulis
- Tugas 2 Mata Kuliah Proses Stokastik
