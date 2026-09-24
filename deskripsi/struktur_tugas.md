# Struktur Makalah: Pemodelan Rantai Markov pada Pergerakan IHSG

Struktur di bawah ini disusun berdasarkan instruksi tugas pada `deskripsi_tugas.md`. Sistematika ini dirancang untuk menghasilkan makalah analisis kuantitatif yang profesional, terstruktur, padat, dan langsung menjawab inti permasalahan (target maksimal 4 halaman).

## 1. Pendahuluan dan Definisi Model
*   **Konteks Studi:** Pengantar ringkas (1-2 paragraf) tentang urgensi pemodelan arah pergerakan Indeks Harga Saham Gabungan (IHSG) menggunakan proses stokastik Rantai Markov.
*   **Definisi Variabel dan Ruang Keadaan ($S$):** 
    *   Penetapan variabel acak $X_n$ yang merepresentasikan arah *return* IHSG pada hari ke-$n$.
    *   Pendefinisian batas interval untuk ruang keadaan diskret $S$ (misalnya: $\{-1, 0, +1\}$ untuk keadaan Turun, Stabil, dan Naik).
*   **Evaluasi Sifat Markov:** 
    *   **Argumen Rasionalitas:** Penjelasan mengapa arah penutupan pasar esok hari dipengaruhi oleh kondisi psikologis pasar hari ini (seperti momentum atau aksi *profit-taking*).
    *   **Asumsi Defensif:** Pengakuan jujur secara akademis mengenai batasan model ini ketika dihadapkan pada Teori Pasar Efisien (*Efficient Market Hypothesis* / *Random Walk*) dan fluktuasi makroekonomi.

## 2. Pengumpulan Data dan Matriks Transisi ($P$)
*   **Deskripsi Data Historis:** Menyebutkan rentang observasi data penutupan IHSG riil yang digunakan untuk estimasi sampel.
*   **Tabel Frekuensi Transisi:** Menampilkan rekapitulasi mentah jumlah perpindahan dari satu status ke status lain.
*   **Pembentukan Matriks Peluang ($P$):** 
    *   Menyajikan hasil konversi frekuensi ke probabilitas empiris (menggunakan metode estimasi kemungkinan maksimum) dalam format blok matriks LaTeX.
*   **Verifikasi Sifat Matriks Stokastik:** 
    *   Pembuktian logis dan matematis bahwa tidak ada nilai probabilitas negatif ($P_{ij} \ge 0$).
    *   Pembuktian matematis bahwa total keseluruhan peluang pada tiap baris adalah persis $1$ ($\sum P_{ij} = 1$).

## 3. Analisis Transisi Multi-Langkah ($P^n$) dan Interpretasi Pasar
*   **Kalkulasi Peluang 2 Hari Mendatang ($P^2$):** 
    *   Demonstrasi langkah perkalian linier $P \times P$.
    *   **Interpretasi Praktis:** Mengartikan lonjakan atau penurunan angka probabilitas tertentu (contoh: apakah momentum tren 'Turun' membesar atau mengecil pada lusa?).
*   **Kalkulasi Peluang 5 Hari Mendatang ($P^5$):** 
    *   Hasil dari perkalian $P^4 \times P$ (mewakili proyeksi satu pekan perdagangan aktif).
    *   **Interpretasi Praktis:** Menganalisis fenomena konvergensi baris matriks yang membuktikan semakin memudarnya "memori" status harga awal seiring bertambahnya waktu horison investasi.
*   **Proyeksi Keseimbangan (Opsional):** Penentuan Distribusi Stasioner ($\pi$) dan nilai Ekspektasi jangka panjang ($E[X]$) untuk memperkuat ketajaman makalah.

## 4. Kesimpulan dan Implikasi Praktis
*   **Sintesis Hasil Pemodelan:** Ringkasan temuan utama mengenai kecocokan antara dinamika IHSG jangka pendek dengan model Markov orde-1.
*   **Rekomendasi bagi Pemangku Kepentingan:**
    *   **Bagi Trader / Investor Ritel:** Validitas penerapan strategi *trend-following* versus *buy-and-hold* dalam horison sangat pendek (harian).
    *   **Bagi Manajer Investasi:** Pemanfaatan angka distribusi stasioner probabilistik sebagai masukan tambahan dalam manajemen risiko dan *asset allocation*.
    *   **Bagi Regulator (OJK/BEI):** Bagaimana pola probabilitas *rebound* dari matriks $P$ dapat diamati sebagai *baseline* untuk mendeteksi *panic selling* yang anomali.