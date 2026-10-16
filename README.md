# Pemodelan Rantai Markov Waktu Diskret untuk Pergerakan Harian IHSG

[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Report: PDF](https://img.shields.io/badge/Report-PDF-red.svg)](report/laporan.pdf)

Penelitian ini memodelkan dinamika pergerakan harian Indeks Harga Saham Gabungan (IHSG, kode ticker `^JKSE`) pada Bursa Efek Indonesia menggunakan rantai Markov waktu diskret orde pertama. Model ini menaksir parameter peluang transisi langsung dari data historis bursa untuk mengukur kecenderungan pembalikan arah jangka pendek, kecepatan konvergensi multi-langkah, dan distribusi probabilitas batas jangka panjang.

---

## Temuan utama penelitian

Berdasarkan ambang batas volatilitas harian plus minus 0,5% yang membagi pasar ke dalam tiga keadaan operasional (Turun, Stabil, Naik), model menghasilkan sejumlah parameter stokastik empiris berikut:

| Parameter evaluasi | Nilai empiris | Interpretasi hasil |
| :--- | :---: | :--- |
| **Peluang Bertahan di Keadaan Stabil ($P_{00}$)** | **50,9%** | Keadaan stabil memiliki persistensi tertinggi dalam satu hari bursa berikutnya. |
| **Peluang Rebound dari Keadaan Turun ($P_{-1,0} + P_{-1,+1}$)** | **69,0%** | Penurunan tajam cenderung tertahan esok harinya (34,5% stabil, 34,5% naik). |
| **Peluang Koreksi dari Keadaan Naik ($P_{+1,0} + P_{+1,-1}$)** | **69,3%** | Lonjakan indeks umumnya disusul fase konsolidasi (48,1% stabil, 21,2% turun). |
| **Waktu Konvergensi ($P^5$)** | **5 hari bursa** | Baris matriks probabilitas transisi bernilai identik dalam satu pekan perdagangan. |
| **Distribusi Probabilitas Stasioner ($\pi$)** | **[24,7%, 46,0%, 29,2%]** | Komposisi probabilitas limit jangka panjang: 24,7% turun, 46,0% stabil, 29,2% naik. |
| **Ekspektasi Batas Arah Pasar ($E[X]$)** | **+0,045** | Ekspektasi arah pasar bernilai positif tipis pada jangka panjang. |

Matriks peluang transisi satu langkah empiris ($P$):

$$P = \begin{pmatrix} 0{,}310 & 0{,}345 & 0{,}345 \\ 0{,}236 & 0{,}509 & 0{,}255 \\ 0{,}212 & 0{,}481 & 0{,}307 \end{pmatrix}$$

Matriks ini memenuhi aksioma matriks stokastik: setiap entri bernilai non-negatif ($P_{ij} \ge 0$) dan jumlahan probabilitas pada setiap baris tepat bernilai 1,000:
- Baris Turun: $0{,}310 + 0{,}345 + 0{,}345 = 1{,}000$
- Baris Stabil: $0{,}236 + 0{,}509 + 0{,}255 = 1{,}000$
- Baris Naik: $0{,}212 + 0{,}481 + 0{,}307 = 1{,}000$

---

## Metodologi dan formulasi matematis

### 1. Sumber data dan perhitungan return harian
Data yang dianalisis mencakup harga penutupan harian IHSG (`^JKSE`) periode 3 Januari 2025 sampai 4 September 2026 yang diunduh melalui antarmuka Yahoo Finance. Persentase return harian dihitung dengan rumus:

$$R_t = \frac{P_t - P_{t-1}}{P_{t-1}} \times 100\%$$

dengan $P_t$ menyatakan harga penutupan pada hari bursa ke-$t$.

### 2. Partisi ruang keadaan
Variabel acak $X_t$ menyatakan keadaan pergerakan pasar pada hari ke-$t$. Ruang keadaan diskret $S = \{-1, 0, +1\}$ ditentukan berdasarkan volatilitas wajar harian pasar modal Indonesia:
- Keadaan $-1$ (Turun): $R_t < -0{,}5\%$
- Keadaan $0$ (Stabil): $-0{,}5\% \le R_t \le +0{,}5\%$
- Keadaan $+1$ (Naik): $R_t > +0{,}5\%$

### 3. Estimasi parameter transisi
Asumsi rantai Markov orde pertama menyatakan bahwa peluang pergerakan pasar esok hari hanya bergantung pada kondisi hari ini:

$$\Pr(X_{t+1} = j \mid X_t = i, X_{t-1} = i_{t-1}, \dots, X_0 = i_0) = \Pr(X_{t+1} = j \mid X_t = i) = P_{ij}$$

Estimator frekuensi empiris dihitung dengan membagi frekuensi transisi dari keadaan $i$ ke keadaan $j$ ($N_{ij}$) terhadap jumlah kemunculan keadaan asal ($N_i$):

$$\hat{P}_{ij} = \frac{N_{ij}}{\sum_{k} N_{ik}}$$

### 4. Proyeksi multi-langkah Chapman-Kolmogorov
Untuk memproyeksikan dinamika beberapa hari ke depan, persamaan Chapman-Kolmogorov dieksekusi melalui pemangkatan matriks transisi:

$$P^{(n)} = P^n$$

Pada proyeksi dua langkah ($n = 2$):

$$P^2 = \begin{pmatrix} 0{,}251 & 0{,}449 & 0{,}301 \\ 0{,}247 & 0{,}463 & 0{,}289 \\ 0{,}244 & 0{,}466 & 0{,}290 \end{pmatrix}$$

Pada proyeksi lima langkah ($n = 5$):

$$P^5 = \begin{pmatrix} 0{,}247 & 0{,}460 & 0{,}292 \\ 0{,}247 & 0{,}460 & 0{,}292 \\ 0{,}247 & 0{,}460 & 0{,}292 \end{pmatrix}$$

Seluruh baris pada $P^5$ telah berkonvergensi ke nilai yang identik hingga tiga angka desimal. Kondisi ini membuktikan bahwa pengaruh fluktuasi harga pada hari awal memudar dalam tempo lima hari bursa.

Vektor probabilitas stasioner $\pi = [\pi_{-1}, \pi_0, \pi_{+1}]$ yang memenuhi sistem persamaan $\pi P = \pi$ dan $\sum \pi_i = 1$ menghasilkan:

$$\pi = [0{,}247 \quad 0{,}460 \quad 0{,}292]$$

Ekspektasi nilai keadaan jangka panjang dihitung melalui:

$$E[X] = (-1)(0{,}247) + (0)(0{,}460) + (+1)(0{,}292) = +0{,}045$$

---

## Pembahasan hasil dan implikasi pasar

1. **Dominansi fase konsolidasi**: Peluang IHSG menetap pada rentang stabil mencapai 50,9%. Angka ini merupakan nilai diagonal utama tertinggi dalam matriks transisi.
2. **Ketiadaan momentum penurunan beruntun**: Peluang IHSG kembali turun setelah hari sebelumnya turun bernilai 31,0%. Probabilitas pasar berbalik arah atau stabil mencapai 69,0% (34,5% stabil dan 34,5% naik). Karakteristik ini menunjukkan pasar memiliki daya tahan terhadap pelemahan harga berkelanjutan dalam jangka pendek.
3. **Peredaman pasca kenaikan**: Ketika IHSG mengalami kenaikan tajam ($+1$), peluang kenaikan berlanjut esok harinya adalah 30,7%. Mayoritas pergerakan beralih ke fase stabil (48,1%) atau koreksi turun (21,2%), dengan total peluang konsolidasi sebesar 69,3%.
4. **Waktu retensi memori pasar yang singkat**: Transisi dua hari ($P^2$) sudah memperlihatkan pemerataan probabilitas antarkeadaan, dan pada hari kelima ($P^5$) matriks mencapai kesetimbangan stasioner. Pengaruh keadaan hari ini memudar dalam waktu satu pekan kalender bursa.

---

## Batasan asumsi dan rekomendasi model

Model rantai Markov diskret menyajikan estimasi yang cepat dan terukur, tetapi memiliki asumsi penyederhanaan yang perlu dicatat:

1. **Sifat nir-memori (memoryless)**: Model mengabaikan riwayat pergerakan harga lebih dari satu hari ke belakang, sehingga tidak memperhitungkan fenomena pengelompokan volatilitas (volatility clustering).
2. **Homogenitas waktu parameter**: Peluang transisi diasumsikan bernilai konstan selama rentang observasi, padahal sentimen makroekonomi dan perubahan suku bunga dapat menggeser rezim volatilitas bursa.
3. **Arah pengembangan**: Analisis berikutnya dapat mengintegrasikan model Markov switching untuk menangkap pergantian rezim pasar, atau menggabungkan model heteroskedastisitas bersyarat (GARCH).

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
- `01_data_collection.ipynb`: Akuisisi data penutupan IHSG melalui yfinance dan eksplorasi return harian.
- `02_markov_chain_modeling.ipynb`: Estimasi matriks transisi, verifikasi aksioma stokastik, proyeksi Chapman-Kolmogorov, dan perhitungan vektor stasioner.

### 4. Kompilasi laporan ilmiah LaTeX
Untuk mengompilasi naskah laporan PDF dari sumber TeX:
```bash
cd report
pdflatex -interaction=nonstopmode laporan.tex
pdflatex -interaction=nonstopmode laporan.tex
```
Berkas keluaran tersimpan pada `report/laporan.pdf`.

---

## Spesifikasi teknologi

- **Bahasa**: Python 3.10+
- **Manipulasi data**: Pandas, NumPy
- **Komputasi dan analisis bursa**: yfinance, Statsmodels
- **Visualisasi data**: Matplotlib, Seaborn
- **Laporan ilmiah**: LaTeX (TeX Live)

---

## Tim peneliti dan konteks akademik

Penelitian ini disusun sebagai pemenuhan tugas terstruktur mata kuliah Pengantar Proses Stokastik pada Program Studi S1 Matematika, Fakultas Matematika dan Ilmu Pengetahuan Alam (FMIPA), Universitas Sebelas Maret (UNS).

**Kelompok 1:**
- Achika Vigo Azhyra (M0125001)
- Fadhila Hardi Ningrum (M0124004)
- Fawwaz Absyar Rifai (M0125044)
- Karunia Febyayu Puspitaningtyas (M0124010)
- Ramadhan Imanur Rochim (M0124015)

**Dosen Pengampu:** Ade Susanti, S.Si., M.Si.
