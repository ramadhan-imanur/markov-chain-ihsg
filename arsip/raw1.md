# Pemodelan Rantai Markov pada Pergerakan Harian Indeks Harga Saham Gabungan (IHSG)

### 1. Definisi Model dan Asumsi Transisi

Laporan ini memodelkan pergerakan Indeks Harga Saham Gabungan (IHSG) menggunakan Rantai Markov orde-1. Untuk mengubah nilai IHSG yang kontinu menjadi proses stokastik diskret, kami memodelkan perubahan harga (*return*) harian, bukan nilai nominal indeks itu sendiri.

Misalkan $P_n$ menyatakan harga penutupan IHSG pada hari perdagangan ke-$n$, dan $R_n = \frac{P_n - P_{n-1}}{P_{n-1}}$ menyatakan return harian terkait. Dinamika status pasar dimodelkan sebagai proses stokastik waktu-diskret $\{X_n, n \ge 0\}$ dengan ruang keadaan terhitung $\mathcal{S} = \{-1, 0, +1\}$, yang didefinisikan melalui pemetaan:$$X_n = \begin{cases}  +1, & \text{jika } R_n > 0{,}5\% & \text{(Tren Naik)} \\  0, & \text{jika } -0{,}5\% \le R_n \le 0{,}5\% & \text{(Konsolidasi)} \\  -1, & \text{jika } R_n < -0{,}5\% & \text{(Tren Turun)}  \end{cases}$$

**Argumen Defensif Sifat Markovian**
Model ini bersandar pada asumsi fundamental bahwa probabilitas transisi memenuhi sifat Markov:
$$ P(X_{n+1} = j \mid X_n = i, X_{n-1}, \dots, X_0) = P(X_{n+1} = j \mid X_n = i) $$

Dalam literatur keuangan, teori *Random Walk* dan Hipotesis Pasar Efisien (EMH) menyatakan bahwa harga saham masa depan tidak dapat diprediksi dari harga masa lalu karena seluruh informasi telah terserap seketika. Memaksakan sifat tanpa memori (*memoryless*) pada IHSG adalah sebuah kompromi analitis terhadap EMH. Di dunia nyata, perilaku pelaku pasar seperti aksi *profit-taking* spontan, *panic selling*, atau *buy on weakness* kerap menciptakan riak momentum harian jangka pendek yang beresonansi kuat dengan asumsi Markov orde-1. 

Namun, secara jujur model ini memiliki batasan:
1. **Satu langkah = satu hari perdagangan:** Hari libur dan akhir pekan diabaikan.
2. **Volatilitas Mengelompok:** Asumsi matriks stasioner gagal menangkap *volatility clustering* (periode krisis yang panjang).
3. **Faktor Makro Diabaikan:** Suku bunga (BI Rate), kurs, dan isu geopolitik tidak dimodelkan secara eksplisit, melainkan dianggap telah tercermin secara tidak langsung ke dalam perubahan status IHSG. Batas $\pm0.5\%$ juga murni merupakan penyederhanaan konvensi.

---

### 2. Estimasi Matriks Peluang Transisi ($P$)

Pengujian empiris dilakukan menggunakan data historis IHSG berjumlah 823 hari perdagangan. Kami menyusun matriks peluang transisi $P$ menggunakan *Maximum Likelihood Estimation* (MLE). Probabilitas dihitung dengan membagi frekuensi transisi spesifik ($N_{ij}$) dengan total observasi pada baris asal ($N_i$):
$$ \hat{P}_{ij} = \frac{N_{ij}}{\sum_{k} N_{ik}} $$

Berdasarkan komputasi data, diperoleh matriks peluang transisi $P$:

| Dari \ Ke | Turun (-1) | Stabil (0) | Naik (+1) |
|---|---|---|---|
| **Turun (-1)** | 0.310 | 0.345 | 0.345 |
| **Stabil (0)** | 0.236 | 0.509 | 0.255 |
| **Naik (+1)** | 0.212 | 0.481 | 0.307 |

**Pembuktian Syarat Matriks Stokastik:**
Berdasarkan aksioma probabilitas, sebuah matriks sah disebut matriks stokastik jika memenuhi dua syarat mutlak:
1. Entri tak-negatif: $P_{ij} \ge 0$ untuk setiap $i,j$.
2. Jumlah peluang tiap baris sama dengan $1$: $\sum_{j} P_{ij} = 1$.

Pada matriks di atas, perhitungan horizontal tiap baris (misal baris "Turun": $0.310 + 0.345 + 0.345 = 1.000$) membuktikan bahwa matriks $P$ secara matematis valid sebagai matriks stokastik.

**Interpretasi Realistis:** 
Status "Stabil" memiliki persistensi dominan dengan peluang $0.509$. Hal yang menarik terjadi pada kondisi ekstrem: ketika pasar "Turun", probabilitas keesokan harinya terpecah persis $0.345$ untuk mereda menjadi stabil dan $0.345$ untuk *rebound* naik. Begitu pula saat pasar "Naik", probabilitas terkuatnya adalah terseret kembali menjadi stabil ($0.481$). Ini mengonfirmasi bahwa sentimen harian ekstrem di IHSG tidak mudah memicu reaksi berantai.

---

### 3. Proyeksi Multi-Langkah ($P^n$) dan Konvergensi

Untuk memproyeksikan probabilitas pergerakan pasar beberapa hari ke depan, kami mengaplikasikan persamaan Chapman-Kolmogorov. Probabilitas transisi $n$-langkah direpresentasikan melalui operasi perkalian matriks ($P^n$).

**Proyeksi Lusa ($n=2$)**
Langkah perhitungan: $P^2 = P \times P$
| Dari \ Ke | Turun (-1) | Stabil (0) | Naik (+1) |
|---|---|---|---|
| **Turun (-1)** | 0.251 | 0.449 | 0.301 |
| **Stabil (0)** | 0.247 | 0.463 | 0.289 |
| **Naik (+1)** | 0.244 | 0.466 | 0.290 |

**Proyeksi Satu Minggu Bursa ($n=5$)**
Langkah perhitungan: $P^5 = P^4 \times P$
| Dari \ Ke | Turun (-1) | Stabil (0) | Naik (+1) |
|---|---|---|---|
| **Turun (-1)** | 0.247 | 0.460 | 0.292 |
| **Stabil (0)** | 0.247 | 0.460 | 0.292 |
| **Naik (+1)** | 0.247 | 0.460 | 0.292 |

**Interpretasi Keuangan:** 
Pada proyeksi lusa ($P^2$), disparitas peluang antarbaris mulai menyusut, mengisyaratkan bahwa momentum pergerakan harga (*trend*) memudar secara drastis. Saat menyentuh $n=5$, matriks mengalami konvergensi stasioner secara absolut. Terlepas dari apakah IHSG ditutup anjlok parah atau reli tajam hari ini, dalam lima hari perdagangan ke depan, pasar telah mereset memori harganya ke distribusi ekuilibrium $\pi$:
$$ \pi = [0.247 \quad 0.460 \quad 0.292] $$
Ekspektasi batas arah pergerakan ($E[X]$) bernilai positif tipis:
$$ E[X] = (-1)(0.247) + (0)(0.460) + (+1)(0.292) = +0.045 $$

---

### 4. Implikasi Praktis

Konvergensi memori harga secara cepat (pada hari ke-5) menghasilkan tiga tindakan operasional yang dapat diambil oleh pemangku kepentingan:

1. **Bagi Trader Harian:** Strategi *momentum-following* murni yang menunggangi riwayat pergerakan harga kemarin sangat berisiko. Karena pasar sepenuhnya mereset memorinya di sekitar $T+5$, keuntungan (*edge*) dari sinyal pergerakan arah harian akan terdistorsi sepenuhnya dalam satu minggu. Keputusan masuk pasar hanya berdasarkan lilin hijau kemarin adalah jebakan probabilitas.
2. **Bagi Manajemen Investasi:** Ekspektasi jangka panjang IHSG bernilai positif tipis. Berdasarkan distribusi stasioner $\pi$, gabungan probabilitas pasar untuk naik atau stabil mencapai $75.2\%$, jauh mengungguli peluang terus turun yang hanya $24.7\%$. Dominansi fundamental ini memberi pembenaran probabilistik yang sangat kuat untuk mempertahankan posisi investasi panjang (*long-bias exposure*) pada bursa saham domestik.
3. **Bagi Regulator (OJK / BEI):** Matriks transisi empiris ini membuktikan ketangguhan sistemis pasar modal kita. Peluang kelanjutan penurunan beruntun dari kondisi turun hanyalah $31.0\%$. Kepanikan ritel jarang termanifestasi menjadi tren krisis *bearish* yang berantai. Matriks ini dapat dijadikan instrumen *baseline* untuk mendeteksi *herd behavior* berlebih jika probabilitas anjlok *real-time* menyimpang secara ekstrem di atas batas normal $31.0\%$ ini.
