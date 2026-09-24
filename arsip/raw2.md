<div style="font-family: 'Times New Roman', Times, serif; text-align: justify; line-height: 1.15; font-size: 11pt;">

<div style="text-align: center;">
    <span style="font-size: 16pt; font-weight: bold;">
        Pemodelan Rantai Markov pada pergerakan harian Indeks Harga Saham Gabungan
    </span>
    <br><br>
    <span style="font-weight: bold;">Karunia Febyayu Puspitaningtyas, Achika Vigo Azhyra, Ramadhan Imanur Rochim, Fawwaz Absyar Rifai, Fadhila Hardi Ningrum</span><br>
    Program Studi Matematika, Fakultas Matematika dan Ilmu Pengetahuan Alam, Universitas Sebelas Maret<br>
    e-mail: kelompoksaham@student.uns.ac.id
</div>

<br><br>

<hr style="border: 1px solid black; margin-bottom: 10px;">
<table style="width: 100%; border-collapse: collapse; font-family: 'Times New Roman', Times, serif; font-size: 10pt;">
  <tr>
    <td style="width: 25%; vertical-align: top; padding-right: 15px; border-right: none;">
      <p style="margin-top:0;"><b><i>Key Words:</i></b></p>
      <hr style="border: 0.5px solid black; margin-top: -5px; margin-bottom: 10px;">
      <p style="margin-top:0;">Rantai Markov; IHSG; probabilitas transisi; matriks stokastik; konvergensi</p>
    </td>
    <td style="width: 75%; vertical-align: top; padding-left: 15px; text-align: justify;">
      <p style="margin-top:0;"><b>Abstrak:</b> Penelitian ini memodelkan pergerakan Indeks Harga Saham Gabungan (IHSG) dengan Rantai Markov orde-1 untuk menguji akurasi pergerakan harga kemarin sebagai prediktor pergerakan hari ini. Menggunakan data historis 823 hari perdagangan, status return harian dibagi menjadi tiga keadaan: Naik, Stabil, dan Turun. Matriks peluang transisi diestimasi menggunakan <i>Maximum Likelihood Estimation</i>. Hasil menunjukkan bahwa kondisi pasar Stabil memiliki persistensi tertinggi (50,9%), sedangkan sentimen ekstrem (Naik/Turun) cenderung mendingin. Proyeksi matriks 5 langkah ke depan membuktikan terjadinya konvergensi penuh, yang mengindikasikan bahwa pasar mereset memori harganya dalam waktu lima hari perdagangan. Simpulan dari penelitian ini menunjukkan bahwa strategi <i>momentum trading</i> harian tidak efektif dalam jangka panjang, dan probabilitas fundamental secara natural mendukung untuk mempertahankan posisi <i>long-bias</i> di IHSG.</p>
    </td>
  </tr>
</table>
<hr style="border: 1px solid black; margin-top: 10px; margin-bottom: 20px;">

<h3 style="text-align: center; color: red; font-size: 12pt; font-weight: bold; text-transform: uppercase;">PENDAHULUAN</h3>

<p style="text-indent: 30px;">Laporan ini memodelkan pergerakan Indeks Harga Saham Gabungan (IHSG) Bursa Efek Indonesia dengan Rantai Markov orde-1 untuk melihat seberapa akurat pergerakan harga kemarin memprediksi pergerakan hari ini. Kami mendefinisikan <i>X<sub>n</sub></i> sebagai status <i>return</i> IHSG pada hari perdagangan ke-<i>n</i>. Model ini berasumsi bahwa transisi memenuhi sifat Markov, di mana probabilitas arah pergerakan esok hari hanya bergantung pada pergerakan pasar hari ini tanpa memedulikan rentetan riwayat masa lalu.</p>

<p style="text-indent: 30px;">Memaksakan sifat tanpa memori (<i>memoryless</i>) pada IHSG adalah sebuah kompromi analisis. Perilaku pasar nyata seperti aksi <i>profit-taking</i> spontan atau <i>buy on weakness</i> memang sering menciptakan momentum harian yang cocok dengan Markov orde-1. Namun, secara teoritis, asumsi stasioner ini memiliki kelemahan mendasar karena mengabaikan <i>volatility clustering</i> (periode fluktuasi tinggi yang berlanjut) dan anomali makroekonomi yang tidak bisa sepenuhnya ditangkap oleh matriks keadaan tiga status yang kaku.</p>


<h3 style="text-align: center; color: red; font-size: 12pt; font-weight: bold; text-transform: uppercase;">METODE</h3>

<p style="text-indent: 30px;">Pengujian empiris dilakukan menggunakan keseluruhan sampel 823 hari perdagangan dari bursa domestik. Ruang keadaan <i>S</i> dibagi secara ketat menjadi tiga kondisi operasional: keadaan +1 (Naik) didefinisikan ketika <i>return</i> harian melampaui +0.5%, keadaan 0 (Stabil) ketika <i>return</i> harian berada pada rentang -0.5% hingga +0.5%, dan keadaan -1 (Turun) ketika <i>return</i> harian jatuh di bawah -0.5%. Metode yang digunakan adalah <i>Maximum Likelihood Estimation</i>, di mana kami menghitung probabilitas empiris dari rekapan data historis dengan membandingkan frekuensi perpindahan transisi terhadap total kejadian di masing-masing baris asal untuk menyusun matriks peluang transisi harian <i>P</i>.</p>


<h3 style="text-align: center; color: red; font-size: 12pt; font-weight: bold; text-transform: uppercase;">HASIL</h3>

<p style="text-indent: 30px;">Berdasarkan perhitungan dari data harian IHSG, matriks peluang transisi empiris <i>P</i> berhasil diestimasi dengan rincian probabilitas sebagai berikut:</p>

<table style="width: 70%; margin: 0 auto; border-collapse: collapse; text-align: center; font-size: 11pt;" border="1">
    <tr>
        <th>Dari \ Ke</th>
        <th>Turun (-1)</th>
        <th>Stabil (0)</th>
        <th>Naik (+1)</th>
    </tr>
    <tr>
        <td><b>Turun (-1)</b></td>
        <td>0.310</td>
        <td>0.345</td>
        <td>0.345</td>
    </tr>
    <tr>
        <td><b>Stabil (0)</b></td>
        <td>0.236</td>
        <td>0.509</td>
        <td>0.255</td>
    </tr>
    <tr>
        <td><b>Naik (+1)</b></td>
        <td>0.212</td>
        <td>0.481</td>
        <td>0.307</td>
    </tr>
</table>
<br>

<p style="text-indent: 30px;">Verifikasi syarat stokastik menunjukkan bahwa semua entri bernilai tak-negatif (<i>P<sub>ij</sub></i> &ge; 0) dan jumlah akumulasi probabilitas pada setiap baris secara horizontal adalah mutlak 1, sehingga matriks <i>P</i> valid. Selanjutnya, untuk meninjau pola jangka menengah, dilakukan pemangkatan matriks untuk dua hari (<i>P</i><sup>2</sup>) dan lima hari (<i>P</i><sup>5</sup>) ke depan:</p>

<p style="text-align: center;"><b>Proyeksi 2 Hari (<i>P</i><sup>2</sup>)</b></p>
<table style="width: 50%; margin: 0 auto; border-collapse: collapse; text-align: center; font-size: 11pt;" border="1">
    <tr>
        <td>0.251</td>
        <td>0.449</td>
        <td>0.301</td>
    </tr>
    <tr>
        <td>0.247</td>
        <td>0.463</td>
        <td>0.289</td>
    </tr>
    <tr>
        <td>0.244</td>
        <td>0.466</td>
        <td>0.290</td>
    </tr>
</table>
<br>

<p style="text-align: center;"><b>Proyeksi 5 Hari (<i>P</i><sup>5</sup>)</b></p>
<table style="width: 50%; margin: 0 auto; border-collapse: collapse; text-align: center; font-size: 11pt;" border="1">
    <tr>
        <td>0.247</td>
        <td>0.460</td>
        <td>0.292</td>
    </tr>
    <tr>
        <td>0.247</td>
        <td>0.460</td>
        <td>0.292</td>
    </tr>
    <tr>
        <td>0.247</td>
        <td>0.460</td>
        <td>0.292</td>
    </tr>
</table>
<br>

<h3 style="text-align: center; color: red; font-size: 12pt; font-weight: bold; text-transform: uppercase;">PEMBAHASAN</h3>

<p style="text-indent: 30px;">Analisis mendalam terhadap matriks <i>P</i> mengungkap beberapa pola anomali di bursa. Terlihat bahwa kondisi pasar "Stabil" memiliki persistensi paling tinggi, yakni peluang sebesar 0.509 untuk tetap stabil keesokan harinya. Menariknya, saat pasar sedang "Naik", probabilitas terkuatnya justru memudar atau mendingin menuju zona stabil (0.481) alih-alih reli berlanjut. Sementara saat pasar "Turun", probabilitas keesokan harinya terpecah rata persis (0.345 dan 0.345) untuk <i>rebound</i> naik atau mereda menjadi stabil. Hal ini membuktikan bahwa sentimen ekstrem harian jarang memicu reaksi berantai yang berkepanjangan.</p>

<p style="text-indent: 30px;">Pada hasil matriks <i>P</i><sup>2</sup>, peluang di seluruh baris mulai terlihat saling mendekat dan merata, menandakan momentum pergerakan harian memudar dengan sangat tajam. Pada langkah <i>n</i> = 5, matriks secara sempurna telah mencapai konvergensi (nilai antarbaris identik). Artinya, dalam lima hari perdagangan (satu minggu bursa), semua memori arah pergerakan historis dari hari ke-0 telah sepenuhnya menguap, membuktikan efisiensi adaptasi pasar. Distribusi stasioner jangka panjang menetap di &pi; = [0.247 &emsp; 0.460 &emsp; 0.292], menghasilkan ekspektasi batas pergerakan bernilai positif tipis sebesar <i>E</i>[<i>X</i>] = +0.045.</p>


<h3 style="text-align: center; color: red; font-size: 12pt; font-weight: bold; text-transform: uppercase;">SIMPULAN</h3>

<p style="text-indent: 30px;">Konvergensi probabilitas ini mengonfirmasi sifat pasar modal Indonesia yang cukup tangguh, di mana kepanikan ekstrem atau <i>panic selling</i> jarang bereskalasi menjadi tren turun harian yang persisten. Bagi para <i>trader</i> harian, temuan empiris ini membuktikan bahwa strategi <i>momentum-following</i> murni yang hanya mengandalkan pergerakan hari sebelumnya rentan kedaluwarsa dengan cepat, karena bursa akan mereset harganya di ekuilibrium sekitar <i>T</i>+5. Sementara itu, bagi manajemen investasi, dominansi probabilitas jangka panjang yang condong untuk selalu naik atau stabil (akumulasi 75.2%) dibandingkan turun (24.7%) memberikan justifikasi rasional yang sangat kuat untuk senantiasa mempertahankan paparan investasi (<i>long-bias exposure</i>) pada fundamental bursa saham domestik.</p>
