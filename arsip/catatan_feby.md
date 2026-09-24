# Laporan Praktis: Pemodelan Rantai Markov pada Pergerakan Harian IHSG

### 1. Definisi Model dan Asumsi Transisi

Laporan ini memodelkan pergerakan harian Indeks Harga Saham Gabungan (IHSG) Bursa Efek Indonesia menggunakan Rantai Markov orde-1. Tujuannya adalah menguji secara empiris seberapa jauh arah pergerakan harga satu hari ke belakang dapat diandalkan sebagai prediktor pergerakan hari esok.

Kami mendefinisikan $X_n$ sebagai variabel acak diskret yang mewakili status *return* IHSG pada hari perdagangan ke-$n$. Ruang keadaan $S$ dibagi menjadi tiga kondisi operasional:
$$ S = \{-1, 0, +1\} $$
* **+1 (Naik):** *Return* harian $> +0.5\%$
* **0 (Stabil):** *Return* harian antara $-0.5\%$ hingga $+0.5\%$
* **-1 (Turun):** *Return* harian $< -0.5\%$

Model ini berasumsi bahwa rangkaian transisi memenuhi sifat Markov:
$$ \text{Pr}(X_{n+1} = j \mid X_n = i, X_{n-1}, \dots, X_0) = \text{Pr}(X_{n+1} = j \mid X_n = i) $$

**Validitas Asumsi Markovian pada Harga Saham**
Dalam praktiknya, memaksakan sifat *memoryless* pada IHSG adalah sebuah kompromi. Kondisi psikologis pasar (seperti aksi *profit-taking* spontan atau *buy on weakness*) sering kali menciptakan momentum harian yang selaras dengan Markov orde-1. Namun, asumsi stasioner ini memiliki kelemahan mendasar: pasar modal di dunia nyata rentan terhadap *volatility clustering* (periode fluktuasi tinggi yang berkelanjutan) dan anomali makro, yang tidak dapat ditangkap murni oleh matriks tiga-status yang kaku.

---

### 2. Estimasi Matriks Peluang Transisi ($P$)

Pengujian dilakukan menggunakan sampel 21 hari perdagangan (5 Agustus - 4 September 2026). Data persentase perubahan harian dikonversi menjadi status diskret $X_n$:

| Tanggal | *Return* (%) | $X_n$ | Tanggal | *Return* (%) | $X_n$ |
|---|---|---|---|---|---|
| 05 Agu 26 | +0.50% | 0 | 21 Agu 26 | +0.37% | 0 |
| 06 Agu 26 | -0.12% | 0 | 24 Agu 26 | -0.37% | 0 |
| 07 Agu 26 | +1.04% | +1 | 26 Agu 26 | -1.48% | -1 |
| 10 Agu 26 | -0.69% | -1 | 27 Agu 26 | +1.81% | +1 |
| 11 Agu 26 | -1.53% | -1 | 28 Agu 26 | -0.06% | 0 |
| 12 Agu 26 | +1.69% | +1 | 31 Agu 26 | +0.11% | 0 |
| 13 Agu 26 | -1.13% | -1 | 01 Sep 26 | +1.14% | +1 |
| 14 Agu 26 | +1.59% | +1 | 02 Sep 26 | -0.06% | 0 |
| 18 Agu 26 | +0.75% | +1 | 03 Sep 26 | +1.09% | +1 |
| 19 Agu 26 | -0.86% | -1 | 04 Sep 26 | -0.47% | 0 |
| 20 Agu 26 | +1.68% | +1 | | | |

Dari 20 pasang transisi yang terekam, probabilitas empiris dihitung menggunakan *Maximum Likelihood Estimation* (frekuensi transisi relatif terhadap total kejadian di baris asal):

| Dari \ Ke | Turun (-1) | Stabil (0) | Naik (+1) |
|---|---|---|---|
| **Turun (-1)** | 0.200 | 0.000 | 0.800 |
| **Stabil (0)** | 0.143 | 0.429 | 0.429 |
| **Naik (+1)** | 0.375 | 0.500 | 0.125 |

**Verifikasi Matriks Stokastik:** Seluruh entri bernilai tak-negatif ($P_{ij} \ge 0$), dan akumulasi probabilitas secara horizontal pada setiap baris menghasilkan nilai pasti $1$. Matriks $P$ di atas valid secara matematis.

*Catatan Temuan:* Berdasarkan sampel 21 hari perdagangan, terlihat pola *rebound* yang tidak biasa. Dari 5 kejadian pasar turun, 4 di antaranya langsung diikuti oleh penutupan naik pada hari berikutnya ($P_{-1,1} = 0.800$). Hal ini menunjukkan tingginya agresivitas *buy on weakness* di pasar selama bulan observasi.

---

### 3. Proyeksi Multi-Langkah ($P^n$) dan Eksekusi Konvergensi

Untuk melihat perilaku pasar pada horison yang sedikit lebih panjang, dilakukan operasi pemangkatan matriks ($P^2$ untuk lusa, dan $P^5$ untuk satu pekan perdagangan).

**Proyeksi 2 Hari ($P^2 = P \times P$)**
| Dari \ Ke | Turun (-1) | Stabil (0) | Naik (+1) |
|---|---|---|---|
| **Turun (-1)** | 0.340 | 0.400 | 0.260 |
| **Stabil (0)** | 0.251 | 0.399 | 0.352 |
| **Naik (+1)** | 0.193 | 0.277 | 0.530 |

*Analisis:* Matriks $P^2$ memunculkan dinamika yang tersembunyi pada $P$. Jika pasar turun hari ini, peluang untuk kembali turun di lusa hari ($P^2_{-1,-1}$) membesar menjadi $0.340$ (naik dari *base rate* $0.200$). Kenaikan probabilitas ini tidak didorong oleh tren turun beruntun, melainkan oleh jalur transitif memantul (Turun $\to$ Naik $\to$ Turun) yang memang mendominasi perilaku *choppy* di sampel ini. Di sisi lain, probabilitas untuk mengalami kenaikan lusa hari jika hari ini naik ($P^2_{1,1}$) melonjak ke $0.530$, menandakan *lag effect* dari momentum positif.

**Proyeksi 5 Hari ($P^5 = P^4 \times P$)**
| Dari \ Ke | Turun (-1) | Stabil (0) | Naik (+1) |
|---|---|---|---|
| **Turun (-1)** | 0.244 | 0.340 | 0.415 |
| **Stabil (0)** | 0.248 | 0.349 | 0.403 |
| **Naik (+1)** | 0.256 | 0.357 | 0.387 |

*Analisis Konvergensi:* Pada langkah $n=5$, variansi nilai antarbaris mulai tereduksi secara drastis. Apapun status pergerakan IHSG pada hari ke-$0$, probabilitas pasar akan berakhir di zona Naik pada lima hari ke depan ($P^5_{i,1}$) berkumpul di rentang sempit 38% hingga 41%. 

Eksekusi algoritma ekuilibrium jangka panjang ($\pi P = \pi$) menghasilkan distribusi stasioner:
$$ \pi = [0.250 \quad 0.350 \quad 0.400] $$
Ekspektasi batas arah pergerakan ($E[X]$) menjadi:
$$ E[X] = (-1)(0.250) + (0)(0.350) + (+1)(0.400) = +0.150 $$

---

### 4. Implikasi bagi Pemangku Kepentingan

Fenomena konvergensi probabilitas dan pola transisi di atas bermuara pada tiga kesimpulan operasional:

1. **Bagi Trader Harian:** Strategi *momentum-following* murni berbasis *return* *lag-1* terbukti memiliki *decay rate* (tingkat keusangan) yang terlalu cepat untuk digunakan di luar horison $T+1$ atau $T+2$. Pasar terlalu cepat mereset memori harganya (mendekati distribusi stasioner di $T+5$) sehingga prediktabilitas taktis dari rantai Markov ini sangat pendek umurnya.
2. **Bagi Manajemen Portofolio:** Keberadaan ekspektasi jangka panjang yang bernilai marginal positif ($E[X] = +0.150$) memberikan justifikasi probabilistik untuk tetap mempertahankan *long-bias exposure* pada alokasi aset bulanan. Model ini membuktikan bahwa probabilitas fundamental harian sedikit condong ke atas meskipun dihantam pergerakan *choppy*.
3. **Bagi Regulator (BEI/OJK):** Tingkat *rebound* absolut 80% ($P_{-1,1}$) adalah bukti absennya *panic selling* berantai pada periode observasi. Kepanikan tidak tereskalasi menjadi *trend* turun persisten, karena likuiditas di pasar langsung menyerap koreksi dengan aksi pembelian harian. Matriks $P$ ini bisa diarsipkan sebagai metrik *baseline* untuk mendeteksi anomali perilaku pasar di bulan-bulan kritis berikutnya.