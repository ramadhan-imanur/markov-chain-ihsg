# Pemodelan Rantai Markov untuk Pergerakan IHSG

Proyek ini memodelkan pergerakan harian Indeks Harga Saham Gabungan (IHSG) menggunakan pendekatan Rantai Markov orde-1. Nilai indeks harian diubah menjadi *return* persentase, kemudian dikelompokkan ke dalam tiga kondisi operasional: Naik, Stabil, dan Turun (berdasarkan ambang batas $\pm0.5\%$). Model ini menghitung probabilitas transisi arah pasar berdasarkan frekuensi historis, dengan asumsi bahwa pergerakan harga hari esok diproyeksikan murni dari kondisi perdagangan hari ini.

## Hasil dan Implikasi

Berdasarkan olahan data historis 823 hari perdagangan, matriks peluang transisi menunjukkan bahwa volatilitas ekstrem di pasar modal domestik cenderung teredam dalam jangka pendek.

Peluang kejatuhan atau lonjakan pasar yang terjadi secara beruntun tertahan di angka sekitar 30\%. Secara statistik, pasar paling sering mengoreksi pergerakannya kembali ke kondisi stabil pada hari perdagangan berikutnya. Dalam waktu lima hari bursa, probabilitas pasar sudah mereset memorinya dan mencapai titik stasioner (46,0\% stabil, 29,2\% naik, 24,7\% turun). 

Distribusi empiris ini memberikan gambaran strategis bagi para pelaku pasar. Angka pembalikan arah yang tinggi menunjukkan bahwa strategi mengejar momentum (*chasing the market*) atau kepanikan (*panic selling*) harian berisiko tinggi. Di sisi lain, akumulasi peluang stabil dan naik yang dominan (75,2\%) dalam jangka menengah memberikan landasan empiris untuk mempertahankan portofolio investasi pasif jangka panjang (*buy and hold*).

## Cara Menjalankan

1. Klon repositori ini:
   ```bash
   git clone <url-repo-anda>
   cd <nama-folder-repo>
   ```

2. Pasang dependensi yang dibutuhkan:
   ```bash
   pip install pandas numpy matplotlib seaborn yfinance jupyter
   ```

3. Buka dan jalankan Jupyter Notebook di dalam folder `code/notebooks/` secara berurutan:
   - `01_data_collection.ipynb`: Mengunduh data historis IHSG terbaru dari Yahoo Finance dan melakukan standardisasi format.
   - `02_markov_chain_modeling.ipynb`: Menghitung matriks probabilitas transisi, memproyeksikan pergerakan jangka pendek ($P^2$ dan $P^5$), dan memvalidasi konvergensi.
