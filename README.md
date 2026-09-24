# Pemodelan Rantai Markov untuk Pergerakan IHSG (Tugas 2)

Proyek repositori akademik untuk pengerjaan **Tugas 2** pada mata kuliah **Pengantar Proses Stokastik**, Program Studi Sarjana Matematika, Fakultas Matematika dan Ilmu Pengetahuan Alam (FMIPA), Universitas Sebelas Maret (UNS).

* **Dosen Pengampu:** Ade Susanti, S.Si., M.Si.
* **Kelompok:**
  * Karunia Febyayu Puspitaningtyas (M0124010)
  * Achika Vigo Azhyra (M0125001)
  * Ramadhan Imanur Rochim (M0124015)
  * Fawwaz Absyar Rifai (M0125044)
  * Fadhila Hardi Ningrum (M0124004)
* **Bentuk Luaran:** Makalah/Laporan Analisis Kuantitatif Tertulis (LaTeX & PDF)

---

## 1. Deskripsi Fenomena & Spesifikasi Model

Proyek ini memodelkan pergerakan harian Indeks Harga Saham Gabungan (IHSG) menggunakan pendekatan **Rantai Markov Waktu Diskret (DTMC) orde-1**. Nilai indeks penutupan harian (*daily close*) diubah menjadi *return* persentase harian:

$$R_t = \frac{P_t - P_{t-1}}{P_{t-1}} \times 100\%$$

Berdasarkan ambang batas volatilitas harian $\pm 0{,}5\%$, ruang keadaan diskret $S$ dipartisi menjadi 3 kondisi operasional:

$$S = \{-1, 0, +1\}$$

* **Keadaan $-1$ (Turun):** $R_t < -0{,}5\%$
* **Keadaan $0$ (Stabil):** $-0{,}5\% \le R_t \le +0{,}5\%$
* **Keadaan $+1$ (Naik):** $R_t > +0{,}5\%$

Model mengasumsikan sifat Markov (*memoryless*):

$$\Pr\{X_{t+1} = j \mid X_t = i, X_{t-1}, \dots\} = \Pr\{X_{t+1} = j \mid X_t = i\} = P_{ij}$$

---

## 2. Struktur Repositori

```text
Tugas 2 / markov-chain-ihsg/
├── .gitignore                      # Filter cache LaTeX (*.aux, *.log, *.toc) & Python (*.pyc)
├── README.md                       # Dokumentasi navigasi & ringkasan komputasi proyek
├── deskripsi/
│   ├── deskripsi_tugas.md          # Panduan penugasan resmi dari dosen pengampu
│   ├── struktur_tugas.md           # Sistematika penulisan naskah laporan 4 halaman
│   └── rule_agent.md               # Pedoman kerja analis kuantitatif & metodologi
├── code/
│   ├── requirements.txt            # Daftar dependensi Python (pandas, numpy, yfinance, dll.)
│   ├── data/
│   │   └── raw/
│   │       └── ihsg_daily.csv      # Data historis harian penutupan IHSG (Yahoo Finance ^JKSE)
│   └── notebooks/
│       ├── 01_data_collection.ipynb          # Akuisisi, eksplorasi, & rekayasa return IHSG
│       └── 02_markov_chain_modeling.ipynb    # Estimasi matriks transisi, P^2, P^5, & titik stasioner
├── report/
│   ├── logo_uns.png                # Logo resmi UNS untuk cover naskah
│   ├── laporan.tex                 # Naskah sumber LaTeX (format resmi Matematika FMIPA UNS)
│   └── laporan.pdf                 # Naskah laporan terkompilasi siap kumpul
└── arsip/
    ├── catatan_feby.md             # Catatan draf pembahasan awal anggota tim
    ├── raw1.md                     # Draf awal penurunan matriks
    ├── raw2.md                     # Draf awal interpretasi hasil
    └── simpulan.md                 # Draf awal simpulan & rekomendasi kebijakan
```

---

## 3. Temuan Kuantitatif & Implikasi Praktis

Berdasarkan analisis empiris 823 hari bursa perdagangan IHSG:

1. **Matriks Transisi Satu Langkah ($P$):**
   * Peluang pasar mengalami pembalikan (*rebound*) dari kondisi Turun menuju Stabil atau Naik mencapai $> 70\%$.
   * Volatilitas ekstrem cenderung diredam secara alami oleh mekanisme pasar dalam jangka pendek.
2. **Proyeksi Multi-Langkah ($P^2$ dan $P^5$):**
   * Dalam 5 hari perdagangan (satu pekan bursa), probabilitas transisi mulai berkonvergensi menuju distribusi batas (*limiting distribution*).
   * Nilai probabilitas batas stasioner:
     $$\pi \approx [24{,}7\% \text{ Turun}, \; 46{,}0\% \text{ Stabil}, \; 29{,}2\% \text{ Naik}]$$
3. **Implikasi bagi Pelaku Pasar:**
   * **Trader Ritel:** Menghindari *panic selling* berlebihan saat pasar anjlok sesaat, karena peluang stabil/rebound pada hari bursa berikutnya sangat dominan.
   * **Manajer Investasi:** Dominasi probabilitas stabil dan naik (akumulasi $75{,}2\%$) memberikan justifikasi empiris bagi strategi *buy and hold* atau alokasi aset berkala.
   * **Regulator (BEI & OJK):** Pola konvergensi dapat digunakan sebagai acuan dasar (*baseline*) untuk mengidentifikasi lonjakan anomali di luar batas stochastic normal.

---

## 4. Panduan Eksekusi & Reproduksi

### A. Persiapan Lingkungan Python

Pasang pustaka yang diperlukan:
```bash
pip install -r code/requirements.txt
```

### B. Menjalankan Analisis

Buka notebook di direktori `code/notebooks/`:
```bash
jupyter notebook code/notebooks/
```
1. Eksekusi `01_data_collection.ipynb` untuk mengunduh dan memvalidasi data IHSG.
2. Eksekusi `02_markov_chain_modeling.ipynb` untuk kalkulasi matriks transisi empiris, verifikasi sifat stokastik, dan perhitungan distribusi stasioner.

### C. Kompilasi Naskah Laporan LaTeX

Untuk mengompilasi naskah laporan menjadi PDF:
```bash
cd report
pdflatex -interaction=nonstopmode laporan.tex
pdflatex -interaction=nonstopmode laporan.tex
```
Dokumen hasil kompilasi akan otomatis tersimpan sebagai `report/laporan.pdf`.
