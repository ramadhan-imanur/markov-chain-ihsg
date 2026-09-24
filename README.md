# Pemodelan Rantai Markov Waktu Diskret untuk Pergerakan Harian IHSG

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Report: PDF](https://img.shields.io/badge/Report-PDF-red.svg)](report/laporan.pdf)

Studi kasus ini memodelkan dinamika pergerakan harian Indeks Harga Saham Gabungan (IHSG) menggunakan rantai Markov waktu diskret orde pertama. Parameter peluang transisi diestimasi dari data historis bursa untuk mengukur kecenderungan pembalikan arah jangka pendek, kecepatan konvergensi multi-langkah, dan distribusi probabilitas jangka panjang.

---

## Ringkasan eksekutif dan temuan kunci

Dengan membagi persentase return harian ke dalam tiga keadaan operasional (Turun, Stabil, Naik) menggunakan ambang batas volatilitas ±0,5%, model menghasilkan metrik berikut:

| Parameter evaluasi | Nilai model | Keterangan |
| :--- | :---: | :--- |
| **Peluang Bertahan di Keadaan Stabil ($P_{00}$)** | **50,9%** | Keadaan stabil memiliki persistensi tertinggi dalam satu hari bursa. |
| **Peluang Rebound dari Keadaan Turun ($P_{-1,0} + P_{-1,+1}$)** | **69,0%** | Pasar cenderung menahan penurunan lanjutan pada hari berikutnya (34,5% stabil, 34,5% naik). |
| **Peluang Koreksi dari Keadaan Naik ($P_{+1,0} + P_{+1,-1}$)** | **69,3%** | Kenaikan umumnya disusul konsolidasi (48,1% stabil, 21,2% turun). |
| **Waktu Konvergensi ($P^5$)** | **5 hari bursa** | Baris matriks transisi mendekati nilai identik, menandakan memori pasar memudar dalam sepekan. |
| **Distribusi Stasioner Jangka Panjang ($\pi$)** | **[24,7%, 46,0%, 29,2%]** | Komposisi batas: 24,7% turun, 46,0% stabil, dan 29,2% naik. |
| **Ekspektasi Batas Arah Pasar ($E[X]$)** | **+0,045** | Memberikan nilai harapan positif tipis pada orientasi tren jangka panjang. |

Matriks peluang transisi satu langkah empiris ($P$):

$$P = \begin{pmatrix} 0{,}310 & 0{,}345 & 0{,}345 \\ 0{,}236 & 0{,}509 & 0{,}255 \\ 0{,}212 & 0{,}481 & 0{,}307 \end{pmatrix}$$

Seluruh baris memenuhi aksioma matriks stokastik dengan entri non-negatif dan jumlahan probabilitas tiap baris tepat bernilai 1,000.

---

## Metodologi

Alur kerja analisis data terbagi ke dalam empat tahap:

1. **Pengumpulan data dan perhitungan return**: Mengambil data penutupan harian IHSG (`^JKSE`) dan menghitung persentase return harian:
   $$R_t = \frac{P_t - P_{t-1}}{P_{t-1}} \times 100\%$$
2. **Partisi ruang keadaan**: Menentukan ruang keadaan diskret $S = \{-1, 0, +1\}$ berdasarkan batas toleransi pergerakan normal bursa:
   - Keadaan $-1$ (Turun): $R_t < -0{,}5\%$
   - Keadaan $0$ (Stabil): $-0{,}5\% \le R_t \le +0{,}5\%$
   - Keadaan $+1$ (Naik): $R_t > +0{,}5\%$
3. **Estimasi matriks transisi**: Menghitung frekuensi transisi empiris ($N_{ij}$) dan membaginya dengan total kemunculan keadaan asal ($N_i$) untuk menyusun matriks $P$.
4. **Proyeksi multi-langkah dan distribusi stasioner**: Menggunakan persamaan Chapman-Kolmogorov untuk menghitung proyeksi 2 hari ($P^2$) dan 5 hari ($P^5$), serta menghitung vektor probabilitas stasioner $\pi$ yang memenuhi $\pi P = \pi$.

---

## Evaluasi dan catatan asumsi Markov

Pendekatan rantai Markov memberikan estimasi arah probabilitas yang ringkas, tetapi memiliki beberapa batasan:

- **Sifat nir-memori (*memoryless*)**: Model mengasumsikan arah pergerakan esok hari hanya dipengaruhi oleh kondisi hari ini, mengabaikan efek volatilitas beruntun (*volatility clustering*).
- **Homogenitas waktu**: Parameter peluang transisi diasumsikan konstan sepanjang periode observasi, padahal dinamika pasar dapat bergeser saat sentimen makroekonomi berubah drastis.
- **Rekomendasi pengembangan**: Analisis lanjutan dapat menerapkan model Markov beralih rezim (*Markov switching*) atau model volatilitas bersyarat (GARCH) untuk menangkap perubahan rezim pasar.

---

## Struktur repositori

```text
markov-chain-ihsg/
├── .gitignore                      # Filter cache LaTeX dan Python
├── LICENSE                         # Lisensi sumber terbuka MIT
├── README.md                       # Dokumentasi repositori proyek
├── requirements.txt                # Dependensi pustaka Python di root
├── code/
│   ├── requirements.txt            # Salinan dependensi modul komputasi
│   ├── data/
│   │   └── raw/
│   │       └── ihsg_daily.csv      # Data historis harian penutupan IHSG
│   └── notebooks/
│       ├── 01_data_collection.ipynb       # Akuisisi data dan eksplorasi return
│       └── 02_markov_chain_modeling.ipynb # Estimasi matriks P, P^2, P^5, dan stasioner
├── deskripsi/
│   ├── deskripsi_tugas.md          # Petunjuk tugas perkuliahan
│   ├── rule_agent.md               # Pedoman pengerjaan teknis
│   └── struktur_tugas.md           # Sistematika penulisan laporan
├── report/
│   ├── logo_uns.png                # Aset logo universitas untuk sampul
│   ├── laporan.tex                 # Sumber naskah laporan format LaTeX
│   └── laporan.pdf                 # Naskah laporan akhir siap baca
└── arsip/
    ├── catatan_feby.md             # Catatan draf pembahasan awal tim
    ├── raw1.md                     # Draf awal penurunan matriks
    ├── raw2.md                     # Draf interpretasi hasil
    └── simpulan.md                 # Draf simpulan
```

---

## Cara menjalankan proyek

### 1. Kloning repositori
```bash
git clone git@github.com:ramadhan-imanur/markov-chain-ihsg.git
cd markov-chain-ihsg
```

### 2. Instalasi dependensi
Gunakan Python 3.10 atau versi yang lebih baru:
```bash
pip install -r requirements.txt
```

### 3. Menjalankan Jupyter Notebook
Jalankan notebook secara berurutan untuk mereproduksi pengolahan data dan kalkulasi matriks:
```bash
jupyter notebook code/notebooks/
```
1. Buka `01_data_collection.ipynb` untuk mengambil dan memvalidasi data historis bursa.
2. Buka `02_markov_chain_modeling.ipynb` untuk menghitung matriks transisi, verifikasi sifat stokastik, dan proyeksi $P^n$.

### 4. Kompilasi laporan LaTeX (opsional)
Untuk mengompilasi naskah laporan menjadi dokumen PDF:
```bash
cd report
pdflatex -interaction=nonstopmode laporan.tex
pdflatex -interaction=nonstopmode laporan.tex
```
Dokumen hasil kompilasi tersimpan pada berkas `report/laporan.pdf`.

---

## Tech stack dan pustaka

- **Bahasa**: Python 3.10+
- **Manipulasi Data**: Pandas, NumPy
- **Analisis & Data Bursa**: yfinance, Statsmodels
- **Visualisasi**: Matplotlib, Seaborn
- **Laporan Ilmiah**: LaTeX (TeX Live)

---

## Tim pengembang dan konteks

Proyek ini disusun sebagai bagian dari studi proses stokastik terapan pada Program Studi S1 Matematika, Fakultas MIPA, Universitas Sebelas Maret (UNS).

**Kelompok 1:**
- Achika Vigo Azhyra (M0125001)
- Fadhila Hardi Ningrum (M0124004)
- Fawwaz Absyar Rifai (M0125044)
- Karunia Febyayu Puspitaningtyas (M0124010)
- Ramadhan Imanur Rochim (M0124015)

**Dosen Pengampu:** Ade Susanti, S.Si., M.Si.
