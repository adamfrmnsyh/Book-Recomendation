# Laporan Proyek Machine Learning - Nabila Alawiyah
## Project Overview
Musik digital kini menjadi media utama konsumsi hiburan global melalui platform streaming seperti Spotify, yang mendominasi distribusi dan akses musik. Popularitas lagu di platform ini sangat menentukan kesuksesan komersial artis dan industri musik secara keseluruhan.

Faktor yang memengaruhi popularitas tidak hanya berasal dari preferensi pendengar, tetapi juga terkait dengan karakteristik audio lagu seperti tempo, loudness, dan valence, yang memengaruhi respon emosional pendengar (Celma & Herrera, 2008). Selain itu, algoritma rekomendasi yang diterapkan platform berperan besar dalam meningkatkan visibilitas lagu (Schedl, 2017).

Dataset Spotify yang berisi fitur akustik dan data popularitas lagu dari berbagai negara memberikan kesempatan untuk menganalisis hubungan antara atribut lagu dan tingkat kepopulerannya, sehingga membantu strategi produksi dan pemasaran musik yang lebih efektif.

## Business Understanding
### Problem Statements
Rumusan masalah dari masalah latar belakang diatas adalah:
1. Siapa saja top 10 penyanyi di Indonesia?
2. Faktor apa saja yang memengaruhi popularitas lagu?
2. Bagaimana cara membuat sistem rekomendasi terbaik yang dapat diimplementasikan?

### Goals
Berdasarkan problem statements, berikut tujuan yang ingin dicapai pada proyek ini.
1. Mengetahui siapa saja top 10 penyanyi di Indonesia?
2. Mengetahui faktor apa saja yang memengaruhi popularitas lagu
3. Mengetahui cara membuat sistem rekomendasi terbaik yang dapat diimplementasikan.

### Solution Approach
Untuk mencapai goals tersebut, solution statements yang diusulkan adalah:
1. Melakukan analisis dengan memfilter data berdasarkan negara Indonesia. Artis diekstrak dari kolom artists dan dipisah per individu, kemudian dihitung rata-rata popularitas serta jumlah lagu Top 50 untuk masing-masing artis. Selanjutnya, dibuat skor gabungan dari kedua metrik tersebut, dan diambil 10 artis dengan skor tertinggi.
2. Menganalisis hubungan antar fitur untuk mengukur kekuatan kontribusi tiap faktor terhadap popularitas lagu, sehingga faktor-faktor utama yang paling berpengaruh dapat diidentifikasi.
3. Menggunakan algoritma cosine similarity untuk membangun sistem rekomendasi, kemudian mengevaluasi performanya guna menjamin keakuratan sistem rekomendasi tersebut.

## Data Understanding
Data yang digunakan untuk membuat sistem rekomendasi musik diambil dari platform open source Kaggle dan dipublikasikan oleh asaniczka, [Top Spotify Songs in 73 Countries (Daily Updated)](https://www.kaggle.com/datasets/asaniczka/top-spotify-songs-in-73-countries-daily-updated)

Dataset ini terdiri atas 2.059.916 baris dan 25 kolom. Terdiri atas 7 kolom yang bertipe data object, 8 kolom bertipe data int64, 9 kolom bertipe data float64, dan 1 kolom bertipe data boolean. Setiap kolom memiliki arti sebagai berikut.

| Variabel           | Tipe Data | Keterangan                                                                 |
|--------------------|-----------|---------------------------------------------------------------------------|
| `spotify_id`       | object    | ID unik untuk lagu dalam database Spotify.                                |
| `name`             | object    | Judul lagu.                                                               |
| `artists`          | object    | Nama artis yang membawakan lagu. |
| `daily_rank`       | integer   | Peringkat harian lagu di daftar Top 50.                                  |
| `daily_movement`   | integer   | Perubahan peringkat dibandingkan dengan hari sebelumnya.                 |
| `weekly_movement`  | integer   | Perubahan peringkat dibandingkan dengan minggu sebelumnya.               |
| `country`          | object    | Kode ISO negara asal playlist Top 50. Jika bernilai Null, berarti berasal dari playlist Global Top 50. |
| `snapshot_date`    | object    | Tanggal pengambilan data dari API Spotify.                               |
| `popularity`       | integer   | Skor popularitas lagu di Spotify saat ini (0–100).                       |
| `is_explicit`      | bool      | Menunjukkan apakah lagu mengandung lirik eksplisit (TRUE or FALSE).  |
| `duration_ms`      | integer   | Durasi lagu dalam satuan milidetik.                                      |
| `album_name`       | object    | Nama album tempat lagu tersebut dirilis.                                 |
| `album_release_date` | object  | Tanggal rilis album.                                                     |
| `danceability`     | float     | Seberapa cocok lagu untuk menari (0.0 – 1.0).                            |
| `energy`           | float     | Tingkat intensitas dan aktivitas lagu (0.0 – 1.0).                       |
| `key`              | integer   | Kunci musikal lagu, direpresentasikan sebagai angka 0–11.                |
| `loudness`         | float     | Tingkat volume rata-rata lagu dalam desibel (bernilai negatif).|
| `mode`             | integer   | Modus lagu: 1 untuk mayor, 0 untuk minor.                                |
| `speechiness`      | float     | Indikator seberapa banyak kata-kata yang terdapat dalam lagu.           |
| `acousticness`     | float     | Kemungkinan bahwa lagu bersifat akustik (0.0 – 1.0).                     |
| `instrumentalness` | float     | Perkiraan seberapa besar lagu bersifat instrumental (tanpa vokal).      |
| `liveness`         | float     | Estimasi apakah lagu direkam secara live (0.0 – 1.0).                    |
| `valence`          | float     | Menggambarkan mood lagu: bahagia (1.0) hingga sedih (0.0).               |
| `tempo`            | float     | Kecepatan lagu dalam satuan BPM (beats per minute).                      |
| `time_signature`   | integer   | Birama lagu, misalnya 4 untuk 4/4.                                       |
### Exploratory Data Analysis
#### 1. Analysis Popularitas Berdasarkan Variable is_explicit
<p align="center">
  <img src="https://github.com/user-attachments/assets/e812c45a-8eef-4491-8216-cc5e3d15d171" width="600"/>
</p>

  > **Insight:**
  > Grafik di atas membandingkan rata-rata popularitas lagu eksplisit dan non-eksplisit. Lagu eksplisit sedikit lebih populer, tapi perbedaannya tidak signifikan, menunjukkan label eksplisit tidak berpengaruh besar pada popularitas lagu.

#### 2. Analysis Popularitas Berdasarkan Negara asal playlist Top 50
<p align="center">
  <img src="https://github.com/user-attachments/assets/493ce204-e4f8-4f95-b6fa-619da81c2bff" width="1000"/>
</p>

  > **Insight:**
  > * Top tiga negara dengan popularitas tertinggi adalah Australia (AU), New Zealand (NZ), dan Canada (CA)
  >  * Top tiga negara dengan popilaritas terendah adalah Islandia (IS), Egypt (EG), dan Bulgaria (BG)

#### 3. Analysis Hubungan Popularitas Berdasarkan Fitur Lagu
<p align="center">
  <img src="https://github.com/user-attachments/assets/be4fabec-2c93-4847-8914-ccd08b2e6519" width="1000"/>
</p>

  > **Insight:**
  > 1. Hasil korelasi menunjukkan bahwa tidak ada hubungan yang kuat antara popularitas dan fitur-fitur tersebut, karena nilai korelasinya sangat rendah.
  > 2. Terdapat korelasi positif yang cukup kuat (0.43) antara danceability dan valence, yang berarti bahwa lagu yang enak untuk berdansa biasanya juga terasa lebih ceria atau menyenangkan. Sebaliknya, fitur acousticness berkorelasi negatif dengan danceability (-0.25) dan valence (-0.16), menunjukkan bahwa lagu yang lebih akustik cenderung terdengar lebih tenang dan kurang cocok untuk berdansa.
  >
  > **Kesimpulan:** Fitur-fitur seperti danceability, acousticness, dan instrumentalness memang saling berhubungan, tetapi tidak berpengaruh besar terhadap popularitas lagu.

#### 4. Top 10 Artist di Indonesia
<p align="center">
  <img src="https://github.com/user-attachments/assets/e83c3c80-8608-45f4-84a8-2bba3a8f3f51" width="1000"/>
</p>

  > **Insight:**
  > * Bernadya jadi artis paling populer di Spotify Indonesia (skor: 521.5) berkat kombinasi popularitas tinggi dan jumlah lagu terbanyak (1556 lagu).
  > * Juicy Luicy dan Hindia menyusul di posisi 2 dan 3, menunjukkan konsistensi rilisan dan basis pendengar yang kuat.
  > * Lyodra dan Feby Putri punya popularitas rata-rata tertinggi (79.17 & 78.51) meski jumlah lagu lebih sedikit. Menandakan kualitas lagu yang tinggi.
  > * Tulus tetap masuk top 5 meskipun produktivitasnya lebih rendah dari yang lain. Menunjukkan lagu-lagunya tahan lama dan terus diputar.

### Data Quality Verification
#### Memeriksa Kesesuaian tipe data dan Menentukan prioritas kolom
<p align="center">
  <img src="https://github.com/user-attachments/assets/aa76bef6-49bf-4f5c-9dcb-5812f8a8d2ce" width="600"/>
</p>

> **Insight:**
> * Untuk melakukan content-based filtering, dapat dilakukan penghapusan beberapa kolom yaitu: daily_rank, daily_movement, weekly_movement, country, snapshot_date, album_name, album_release_date, key, mode, time_signature.
> * Melakukan konversi pada kolom duration_ms ke menit.
> * Kolom loudness bernilai negatif, sehingga perlu dilakukan standarisasi
> * Kolom is_explicit diubah ke dalam numerik (TRUE : 1, dan FALSE : 0)
> * Perlu dilakukan standarisasi pada numerical feature

#### Memeriksa Data duplikat
<p align="center">
  <img src="https://github.com/user-attachments/assets/b966327e-1515-4f7e-a99c-4abe8550ce2d" width="400"/>
</p>

> **Insight:**
> 1. Tidak ada data yang duplikat

#### Memeriksa Data Missing Value
<p align="center">
  <img src="https://github.com/user-attachments/assets/c074d2ae-618e-49fe-adf7-76fad622c3b1" width="400"/>
</p>

> **Insight:**
>
> * Kolom country memiliki banyak nilai null (28.058), yang berarti sebagian besar data berasal dari playlist Global Top 50.
> * Kolom name dan artists memiliki sedikit nilai null (sekitar 29-30), yang harus diperhatikan karena penting untuk analisis lagu dan artis.
> * Kolom album_name dan album_release_date juga memiliki cukup banyak nilai null (822 dan 659), bisa jadi karena beberapa lagu belum ada data album lengkapnya.
> * Sebagian besar kolom fitur audio dan metadata lainnya tidak memiliki nilai null, jadi data cukup lengkap di aspek teknis.

#### Memeriksa Outliers dengan IQR Method
##### 1. Menampilkan analisis statistik
<p align="center">
  <img src="https://github.com/user-attachments/assets/80627d43-7d0e-428e-946d-517bc24d2676" width="1000"/>
</p>

  > Fungsi `describe()` memberikan informasi statistik pada masing-masing kolom, antara lain:
  > - `Count` adalah jumlah sampel pada data.
  > - `Mean` adalah nilai rata-rata.
  > - `Std` adalah standar deviasi.
  > - `Min` yaitu nilai minimum setiap kolom.
  > - `25%` adalah kuartil pertama. Kuartil adalah nilai yang menandai batas interval dalam empat bagian sebaran yang sama.
  > - `50%` adalah kuartil kedua, atau biasa juga disebut median (nilai tengah).
  > -` 75%` adalah kuartil ketiga.
  > - `Max` adalah nilai maksimum.

##### 2. Mengecek data outlier dan memvisualisasikannya
<p align="center">
  <img src="https://github.com/user-attachments/assets/6ec16bdc-d915-4a57-9ffe-1dfc8d8b4784" width="1000"/>
</p>

  > Program di atas akan menampilkan hasil visualisasi sebagai berikut:

<p align="center">
  <img src="https://github.com/user-attachments/assets/350ea572-4cfd-4c34-8d9e-31586eabd778" width="1000"/>
</p>

> **Interpretasi Boxplot:**
> 1. Daily Rank & Popularity.
   Sebagian besar lagu memiliki ranking harian dan tingkat popularitas yang cukup merata. Tidak terdapat outlier yang signifikan pada kedua kolom ini.
> 2. Daily Movement & Weekly Movement.
   Nilai perpindahan peringkat harian dan mingguan sebagian besar berada di sekitar nol, namun terdapat beberapa outlier yang menandakan lagu dengan perubahan peringkat yang sangat drastis.
> 3. Duration (duration\_ms). 
   Durasi lagu umumnya berada antara 150.000 hingga 250.000 milidetik (2,5–4 menit), dengan beberapa outlier berdurasi sangat pendek maupun panjang.
> 4. Danceability, Energy, & Valence. 
   Sebagian besar lagu memiliki nilai danceability, energy, dan valence yang tinggi (di atas 0.5), menandakan lagu-lagu cenderung mudah dinikmati, enerjik, dan ceria.
> 5. Key & Mode. 
   Penyebaran nada dasar (`key`) cukup merata dari 0 sampai 11. Sementara itu, `mode` cenderung seimbang antara mayor dan minor.
> 6. Speechiness. 
   Terdapat banyak outlier pada fitur ini, menandakan adanya sejumlah lagu dengan unsur lirik berbicara yang tinggi, seperti rap atau spoken word.
> 7. Acousticness. 
   Mayoritas lagu memiliki tingkat akustik yang rendah hingga sedang. Terdapat outlier untuk lagu yang sangat akustik.
> 8. Instrumentalness. 
   Sebagian besar lagu memiliki nilai instrumentalness sangat rendah, menunjukkan dominasi lagu dengan vokal. Beberapa outlier mengindikasikan lagu instrumental penuh.
> 9. Liveness 
   Mayoritas lagu direkam di studio dengan sedikit unsur live, meskipun ada beberapa outlier yang menunjukkan kemungkinan lagu live performance.
> 10. Loudness & Tempo  
    Nilai loudness menunjukkan lagu umumnya cukup keras, dengan rentang antara -8 hingga -4 dB. Tempo lagu bervariasi, umumnya berada antara 90–150 BPM, dengan beberapa outlier bertempo sangat tinggi.
>
> **Kesimpulan:**
> Sebagian besar fitur numerik memiliki distribusi data yang wajar dan tidak ekstrem. Beberapa fitur seperti `speechiness`, `instrumentalness`, dan `tempo` mengandung outlier, namun hal ini bisa mencerminkan keragaman jenis lagu (misalnya lagu instrumental, rap, atau lagu tempo cepat). Oleh karena itu, outlier tidak akan dihapus karena tetap relevan dan mencerminkan variasi alami dalam musik.

## Data Preparation
### Data Cleaning
#### Menentukan df_sample yang digunakan
Karena jumlah data asli terlalu banyak, jadi saya hanya menggunakan 5k data dengan country berfokus di Indonesia saja.
<p align="center">
  <img src="https://github.com/user-attachments/assets/22f16c5a-4c1f-407a-aa9c-268b0f437f00" width="1000"/>
</p>

#### Menghapus kolom yang tidak relevan
Menghapus kolom `daily_rank`, `daily_movement`, `weekly_movement`, `country`, `snapshot_date`, `album_name`, `album_release_date`, `key`, `mode`, `time_signature` karena tidak relevan dengan tujuan. 
<p align="center">
  <img src="https://github.com/user-attachments/assets/bcd4306b-c17e-406d-b458-f6ce1786beff" width="1000"/>
</p>

#### Melakukan konversi duration_ms ke dalam menit 
Program di bawah ini digunakan untuk melakukan konversi dari detik ke menit, lalu hasilnya akan menggunakan format 3 angka di belakang koma, contohnya 9.999. 
<p align="center">
  <img src="https://github.com/user-attachments/assets/856d7f1a-a7d8-4a5b-a886-0e9b800efb58" width="600"/>
</p>

#### Menangani Kolom name dan Artist
Program di bawah ini digunakan untuk menggabungkan kolom name dan artis menjadi unique_song_key.
<p align="center">
  <img src="https://github.com/user-attachments/assets/200f31e7-c121-49f3-bd44-5fc1962a7971" width="600"/>
</p>

#### Encoding Categorical
Program di bawah ini digunakan untuk mengubah nilai dari kolom is_explicit menjadi 1 untuk True, dan 0 untuk False
<p align="center">
  <img src="https://github.com/user-attachments/assets/e4267b30-c550-4b63-8671-dd20803cde3d" width="600"/>
</p>

#### Standarisasi
Proses standarisasi membantu untuk membuat fitur data menjadi bentuk yang lebih mudah diolah oleh algoritma dan menyeragamkan karena memiliki satuan yang berbeda pada tiap fitur.
<p align="center">
  <img src="https://github.com/user-attachments/assets/20d64fc2-e752-4b25-af91-acd15e813aa2" width="600"/>
</p>

#### Dataset hasil data cleaning, encoding, dan standarisasi
<p align="center">
  <img src="https://github.com/user-attachments/assets/e01bd0cc-0dbe-41f0-ba7c-5aae87d718bb" width="1000"/>
</p>

## Modeling and Result
### Cosine Similarity
Model rekomendasi ini dibangun menggunakan pendekatan content-based filtering, yang berfokus pada kesamaan atribut antar lagu untuk menghasilkan rekomendasi. Sistem ini menggunakan metrik cosine similarity untuk mengukur tingkat kemiripan antar lagu berdasarkan fitur-fitur numerik yang telah dinormalisasi, seperti danceability, energy, loudness, speechiness_log, acousticness, instrumentalness_log, liveness, valence, tempo, dan duration_ms_log. Beberapa fitur mengalami transformasi logaritmik (_log) untuk mengurangi skewness dan membuat distribusi lebih normal sebelum proses perhitungan similarity dilakukan.

Cosine similarity sendiri bekerja dengan menghitung sudut antara dua vektor dalam ruang fitur multidimensi. Nilai cosine similarity berkisar dari -1 (sangat berbeda) hingga 1 (sangat mirip). Dalam konteks sistem rekomendasi ini, nilai yang mendekati 1 menunjukkan bahwa dua lagu memiliki karakteristik musikal yang serupa, sedangkan nilai mendekati -1 menunjukkan perbedaan signifikan antar lagu.

Pemilihan cosine similarity didasarkan pada kemampuannya untuk mengukur kesamaan arah antar vektor tanpa terpengaruh oleh perbedaan skala nilai fitur. Ini penting karena fitur-fitur seperti tempo dan loudness memiliki rentang nilai yang berbeda, namun arah dan pola hubungan antar fitur lebih penting dibandingkan besarannya.

Berikut contoh penerapan untuk mengetahui cosine similarity dengan dataset hasil data cleaning, encoding, dan standarisasi
<p align="center">
  <img src="https://github.com/user-attachments/assets/a93474cd-5341-458e-a52d-97a1ef66b1d6" width="1000"/>
</p>

Setelah mendapatkan nilai cosine similarity antar lagu menggunakan fungsi cosine_similarity, hasilnya dikonversi ke dalam sebuah DataFrame bernama cosine_sim_df, dengan indeks dan kolom berupa unique_song_key, yaitu kombinasi unik antara nama artis dan judul lagu. Matriks ini digunakan untuk membandingkan seluruh pasangan lagu berdasarkan kemiripan fitur audio mereka, seperti danceability, energy, acousticness, dan lainnya.

<p align="center">
  <img src="https://github.com/user-attachments/assets/45117a96-d967-430b-80d0-6f0b975540f8" width="1000"/>
</p>

Output print('Shape:', cosine_sim_df.shape) menunjukkan bahwa terdapat 210 lagu yang dibandingkan, sehingga terbentuk matriks berukuran 210x210. Setiap elemen dalam matriks tersebut mewakili skor kemiripan antara dua lagu. Misalnya, skor similarity sebesar 0.896 antara lagu "James Arthur, Anne-Marie - Rewrite The Stars" dan "Katy Perry - Wide Awake" menunjukkan kemiripan tinggi, sedangkan skor -0.617 antara "Feby Putri, Fiersa Besari - Runtuh" dan "Nidji - Hapus Aku" menunjukkan perbedaan yang besar. Matriks ini menjadi fondasi utama sistem rekomendasi untuk menemukan lagu-lagu yang mirip berdasarkan kontennya.


### Inference
Untuk melakukan inference berdasarkan lagu tertentu, digunakan fungsi recommend_by_identifier(). Fungsi ini memiliki dua parameter: song_identifier (judul lagu yang dijadikan acuan) dan top_n (jumlah rekomendasi yang diinginkan). Fungsi akan mengambil nilai kemiripan lagu tersebut terhadap semua lagu lain dalam cosine_sim_df, mengurutkannya secara menurun, lalu mengembalikan top_n lagu dengan skor similarity tertinggi—tentunya tanpa menyertakan lagu itu sendiri.

<p align="center">
  <img src="https://github.com/user-attachments/assets/c84b6eab-d55c-4942-a3bb-b35969c56159" width="1000"/>
</p>

Contoh penggunaan function recommend_by_identifier, pada lagu NIKI - Take A Chance With Me dengan top_n = 10.  Sistem akan merekomendasikan 10 lagu yang memiliki karakteristik audio paling mirip dengan lagu tersebut. Beberapa lagu yang muncul dalam daftar rekomendasi antara lain "Devano - Surat Hati" dan "Aziz Hedra - Somebody's Pleasure", yang menunjukkan bahwa model mampu mengidentifikasi lagu dengan nuansa dan warna musik yang serupa.
Melakukan inference dengan membuat dan memanggil function recommend_by_identifier seperti pada program di bawah ini.
<p align="center">
  <img src="https://github.com/user-attachments/assets/4c928743-6c23-4345-a506-4bdbc58da7ce" width="1000"/>
</p>

> Sebagai contoh hasil implementasi, sistem merekomendasikan 10 lagu paling mirip terhadap lagu input "NIKI - Take A Chance With Me", di antaranya: "Devano - Surat Hati", "Aziz Hedra - Somebody's Pleasure", dan lainnya, yang secara fitur memiliki karakteristik mirip. Ini menunjukkan model mampu mengidentifikasi lagu-lagu dengan warna dan nuansa serupa.


Contoh penggunaan function recommend_by_identifier, pada Juicy Luicy - Tampar dengan top_n = 5. 
<p align="center">
  <img src="https://github.com/user-attachments/assets/ab301cae-805f-4ec9-86f8-95d776050aed" width="600"/>
</p>

> Begitu pula saat fungsi dipanggil dengan recommend_by_identifier("Juicy Luicy - Tampar", top_n=5), sistem mengembalikan lima lagu yang paling mirip secara musikal dengan lagu tersebut. Ini membuktikan bahwa pendekatan content-based filtering berbasis cosine similarity dapat memberikan rekomendasi yang relevan berdasarkan kemiripan fitur-fitur audio.

### Kelebihan dan kekurangan pendekatan Content-Based Filtering
**a. Rekomendasi Berdasarkan Fitur Lagu**
CBF memanfaatkan fitur seperti danceability,	energy,	loudness, dan lain-lain. Sistem dapat merekomendasikan lagu yang mirip dengan lagu yang sering didengarkan pengguna, meskipun lagu tersebut tidak populer secara global.
> Contoh: Jika seorang pengguna sering mendengarkan lagu dengan tempo cepat dan genre Latin, CBF akan menyarankan lagu-lagu lain dengan karakteristik serupa.

**b. Tidak Perlu Data dari Pengguna Lain**
Sistem tetap bisa bekerja meski hanya ada sedikit data pengguna, karena rekomendasi dibangun berdasarkan histori interaksi user itu sendiri dan fitur konten lagu.
> Cocok untuk pengguna baru di negara tertentu yang belum banyak memiliki data komunitas Spotify di wilayahnya.

**c. Efektif untuk Lagu Baru atau Niche**
CBF dapat merekomendasikan lagu-lagu yang belum banyak didengar orang (belum populer), tapi punya karakteristik yang mirip dengan lagu yang disukai pengguna.
> Berguna untuk mengenalkan lagu-lagu lokal di masing-masing negara dalam dataset (73 negara), meski belum viral secara global.

**d. Lebih Personal dan Bisa Dijelaskan**
CBF memungkinkan sistem menjelaskan alasan suatu lagu direkomendasikan, seperti:
> "Lagu ini memiliki energi tinggi dan genre pop, mirip dengan lagu-lagu yang sering Anda dengarkan."

**e. Mengurangi Risiko Bias Popularitas**
Sistem tidak hanya merekomendasikan lagu yang sedang trending, tetapi juga lagu yang sesuai dengan selera unik pengguna, meskipun kurang dikenal secara umum.


## Evaluation
### Penjelasan Metrik
Metrik yang digunakan untuk mengevaluasi seberapa baik model *content-based filtering* dalam memberikan rekomendasi adalah **Precision\@k**, **Recall\@k**, dan **F1-Score\@k**.

Metrik-metrik ini merupakan bagian dari evaluasi berbasis relevansi, yang biasa digunakan dalam sistem rekomendasi dan *information retrieval*.

1. **Precision\@k**
   Precision\@k digunakan untuk mengukur proporsi item yang relevan dari total item yang direkomendasikan sebanyak *k*.
   Artinya, metrik ini menunjukkan seberapa **tepat** sistem dalam memberikan rekomendasi.
   Formula:

   $$\text{Precision@k} = \frac{\text{Jumlah item relevan dalam rekomendasi}}{k}$$

2. **Recall\@k**
   Recall\@k digunakan untuk mengukur proporsi item relevan yang berhasil ditemukan oleh sistem dari seluruh item relevan yang tersedia.
   Metrik ini menunjukkan seberapa **lengkap** sistem dalam menangkap item relevan.
   Formula:
   
   $$\text{Recall@k} = \frac{\text{Jumlah item relevan dalam rekomendasi}}{\text{Jumlah total item relevan}}$$

4. **F1-Score\@k**
   F1-Score\@k merupakan rata-rata harmonik dari Precision dan Recall. Metrik ini digunakan untuk memberikan keseimbangan antara ketepatan dan kelengkapan rekomendasi.
   Formula:

   $$\text{F1-Score@k} = \frac{2 \times \text{Precision@k} \times \text{Recall@k}}{\text{Precision@k} + \text{Recall@k}}$$
   
### Contoh Penerapan Metrik
Penerapan metrik dilakukan dengan membuat ground truth, lalu membuat fungsi evaluate_single_recommendation dengan parameter song_id, recommend_func, ground_truth, k=10.
<p align="center">
  <img src="https://github.com/user-attachments/assets/7c35cb78-6a91-4c53-8789-c71dbf4221ce" width="600"/>
</p>

Selanjutnya, fungsi tersebut dipanggil sehingga menampilkan output di bawah ini.
<p align="center">
  <img src="https://github.com/user-attachments/assets/728f6d1e-2204-47db-b84e-72ab9b983ca1" width="600"/>
</p>

> **Insight:**
> * Precision@5 = 0.6, artinya dari 5 lagu yang direkomendasikan, rata-rata 60% adalah lagu yang relevan. Ini menunjukkan bahwa sebagian besar rekomendasi cukup tepat sasaran, meskipun masih ada ruang untuk perbaikan agar hasil rekomendasi semakin relevan.
> * Recall@5 = 0.75, artinya dari seluruh lagu relevan yang tersedia, 75% berhasil ditemukan oleh sistem dalam top-5. Ini menandakan bahwa sistem mampu menjangkau sebagian besar lagu yang seharusnya direkomendasikan.
> * F1-Score@5 = 0.6667 menunjukkan keseimbangan yang cukup baik antara Precision dan Recall, yang berarti sistem memiliki performa yang solid dalam memberikan rekomendasi yang relevan secara proporsional.
> * Relevant Found seperti “yung kai - blue”, “Nadin Amizah - Bertaut”, dan “Tulus - Hati-Hati di Jalan” memperkuat bahwa sistem berhasil menyarankan lagu-lagu yang mirip secara musikal atau emosional dengan lagu acuan.

## Kesimpulan
### 1. Mengetahui siapa saja top 10 penyanyi di Indonesia
Untuk mengetahui siapa saja top 10 penyanyi di Indonesia, pertama-tama adalah melakukan filter data hanya untuk country dengan kode "ID", lalu melakukan perhitungan popularitasnya, dan menampilkan visualisasi ke dalam bar chart. Menggunakan program di bawah ini:
<p align="center">
  <img src="https://github.com/user-attachments/assets/e66f7bf5-ce25-4c12-8617-168d0ce3b19a" width="800"/>
</p>

Akan menghasilkan visualisasi sebagai berikut.
<p align="center">
  <img src="https://github.com/user-attachments/assets/a4ef6dd3-5287-4be5-9582-1642548b783f" width="800"/>
</p>

> **Insight:**
> * Bernadya jadi artis paling populer di Spotify Indonesia (skor: 522.4) berkat kombinasi popularitas tinggi dan jumlah lagu terbanyak (1556 lagu).
> * Juicy Luicy dan Hindia menyusul di posisi 2 dan 3, menunjukkan konsistensi rilisan dan basis pendengar yang kuat.
> * Lyodra dan Feby Putri punya popularitas rata-rata tertinggi meski jumlah lagu lebih sedikit. Menandakan kualitas lagu yang tinggi.
> * Tulus tetap masuk top 5 meskipun produktivitasnya lebih rendah dari yang lain. Menunjukkan lagu-lagunya tahan lama dan terus diputar.

### 2. Mengetahui faktor apa saja yang memengaruhi popularitas lagu
<p align="center">
  <img src="https://github.com/user-attachments/assets/ae55a290-c790-401f-99f7-8817728f4304" width="800"/>
</p>

**Insight:**
1. Fitur yang Paling Positif Berkorelasi dengan Popularitas:
   * Tidak ada fitur yang memiliki korelasi kuat dengan popularitas. Korelasi tertinggi masih sangat lemah.
   * Valence (0.04), energy (-0.02), dan danceability (-0.05) hanya menunjukkan korelasi sangat lemah terhadap popularitas. Nilai-nilai ini terlalu kecil untuk dianggap signifikan secara praktis.
2. Fitur yang Berkorelasi Negatif dengan Popularitas:
   * Speechiness (-0.15): Memiliki korelasi negatif paling besar terhadap popularitas, artinya lagu yang terdengar seperti berbicara (seperti spoken word atau rap yang kering) cenderung kurang populer.
   * Acousticness (-0.07) dan tempo (-0.01) juga menunjukkan korelasi negatif, tapi sangat kecil — artinya dampaknya terhadap popularitas sangat terbatas.

3. Fitur Lain yang Kurang Berpengaruh:

   * Liveness (-0.04), instrumentalness (0.05), loudness (0.00), dan tempo (-0.01) menunjukkan hampir tidak ada hubungan dengan popularitas. Mereka dapat dianggap sebagai fitur yang tidak terlalu berpengaruh dalam dataset ini.

**Kesimpulan:**

Fitur-fitur seperti **valence**, **danceability**, dan **energy** memiliki kontribusi paling besar terhadap popularitas lagu. Artinya, lagu yang upbeat, enerjik, dan ceria lebih cenderung disukai oleh pendengar global. Sebaliknya, lagu yang terlalu akustik atau banyak mengandung narasi tidak terlalu populer.

### 3. Mengetahui cara membuat sistem rekomendasi terbaik yang dapat diimplementasikan.
Untuk menjawab permasalahan ini, dibuatlah sebuah fungsi bernama `run_recommendation()`. Fungsi ini merupakan implementasi sistem rekomendasi berbasis content-based filtering yang dirancang untuk memberikan saran lagu kepada pengguna berdasarkan lagu yang mereka sukai. Sistem ini bekerja dengan meminta input berupa nama lagu, kemudian mencocokkannya dengan data yang tersedia dalam matriks kemiripan (cosine similarity) antar lagu. Jika lagu ditemukan, sistem akan menggunakan fungsi `recommend_by_identifier()` untuk menampilkan 10 lagu teratas yang memiliki kemiripan tertinggi dengan lagu input berdasarkan fitur-fitur seperti danceability, energy, valence, dan lainnya. Nilai cosine similarity digunakan untuk mengukur kedekatan antar lagu dalam ruang fitur, di mana semakin tinggi nilainya, semakin mirip kedua lagu tersebut. Hasil rekomendasi ditampilkan secara interaktif kepada pengguna lengkap dengan skor kemiripan, sehingga pengguna dapat memahami alasan di balik rekomendasi yang diberikan. Dengan pendekatan ini, sistem menjadi intuitif, relevan, dan mudah diimplementasikan dalam aplikasi nyata untuk meningkatkan pengalaman mendengarkan musik pengguna.

<p align="center">
  <img src="https://github.com/user-attachments/assets/f17b31d1-6518-4259-a9ce-0895e1c9139a" width="800"/>
</p>

Output yang dihasilkan adalah sebagai berikut.
<p align="center">
  <img src="https://github.com/user-attachments/assets/717ed426-0fe0-4f72-95b3-fae1fdaace08" width="800"/>
</p>

Hasil sistem rekomendasi lagu berdasarkan input "Bernadya - Satu Bulan" menunjukkan performa yang cukup baik dalam menyajikan lagu-lagu dengan karakteristik serupa. Hal ini terlihat dari munculnya beberapa lagu lain dari Bernadya sendiri, seperti "Kata Mereka Ini Berlebihan", "Masa Sepi", dan "Lama-Lama", yang menempati posisi tinggi dalam daftar dengan nilai similarity yang cukup besar. Ini mengindikasikan bahwa sistem mampu mengenali konsistensi gaya musik dan nuansa emosional khas dari artis tersebut. Selain itu, rekomendasi juga mencakup lagu-lagu dari musisi dengan genre pop balada yang serupa, seperti Tiara Andini, Arsy Widianto, dan Fabio Asher. Lagu-lagu seperti "Usai", "Cintanya Aku", dan "Rumah Singgah" memiliki sentuhan melankolis yang sejalan dengan nuansa lagu Satu Bulan. Sistem juga menampilkan lagu dari artis internasional seperti SZA dan Bruno Mars, yang meskipun berasal dari ranah global, tetap dinilai memiliki kemiripan dari sisi mood dan aransemen musik. Nilai similarity yang konsisten di atas 0.79 menunjukkan bahwa sistem ini cukup stabil dan relevan dalam memberikan rekomendasi lagu berdasarkan kesamaan emosional, gaya vokal, dan karakter musik.

## Referensi
1. Celma, Ò., & Herrera, P. (2008, October). A new approach to evaluating novel recommendations. In Proceedings of the 2008 ACM conference on Recommender systems (pp. 179-186).
2. Schedl, M., Knees, P., & Gouyon, F. (2017, August). New paths in music recommender systems research. In Proceedings of the Eleventh ACM Conference on Recommender Systems (pp. 392-393).
