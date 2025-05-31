# Laporan Proyek Machine Learning - Adam Firmansyah
## Project Overview
Di era digital, pembaca memiliki akses ke jutaan novel dengan berbagai genre, penulis, dan penerbit. Dengan begitu banyak pilihan, seringkali pengguna kesulitan menemukan novel yang sesuai dengan selera dan preferensi mereka. Hal ini menimbulkan kebutuhan akan sistem rekomendasi yang mampu memberikan saran novel yang relevan dan personal berdasarkan karakteristik buku dan preferensi pembaca.

## Business Understanding
### Problem Statements
Rumusan masalah dari masalah latar belakang diatas adalah:
1. Novel apa saja yang paling populer dan disukai berdasarkan genre dan metadata buku?
2. Fitur atau atribut apa saja dari novel (misalnya genre, deskripsi, tahun terbit) yang berpengaruh terhadap kesukaan pembaca?
3. Bagaimana cara membangun sistem rekomendasi novel yang memberikan saran sesuai dengan preferensi pengguna, khususnya berdasarkan genre dan metadata buku?

### Goals
Berdasarkan problem statements, berikut tujuan yang ingin dicapai pada proyek ini.
1. engidentifikasi novel-novel yang populer dan banyak diminati berdasarkan genre dan metadata lainnya.
2. Menentukan atribut buku yang berperan penting dalam menentukan rekomendasi yang sesuai.
3. Membangun sistem rekomendasi berbasis Content-Based Filtering yang menggunakan fitur genre, deskripsi, dan metadata lain untuk memberikan rekomendasi novel yang personal dan relevan.

### Solution Approach
Untuk mencapai goals tersebut, solution statements yang diusulkan adalah:
1. Data Exploration & Preprocessing
- Memahami distribusi genre, deskripsi, dan atribut lain dalam dataset.
- Membersihkan data dan menyiapkan fitur untuk sistem rekomendasi.

2. Feature Extraction
- Mengolah data teks seperti genre dan deskripsi menggunakan teknik seperti TF-IDF untuk representasi fitur buku.

3. Content-Based Filtering
- Menggunakan algoritma cosine similarity untuk menghitung kemiripan antar buku berdasarkan fitur teks.
- Membangun fungsi rekomendasi yang memberikan daftar novel serupa sesuai input pengguna.


## Data Understanding
Data yang digunakan untuk membuat sistem rekomendasi musik diambil dari platform open source Kaggle dan dipublikasikan oleh chhavidhankhar11, (https://www.kaggle.com/datasets/chhavidhankhar11/amazon-books-dataset/)

Dataset ini terdiri atas 3 dataframe yaitu Books_df, Genre_df, dan Sub_Genre_df.

Deskripsi variabel Books_df

| Nama Kolom            | Tipe Data | Deskripsi Singkat                                              |
| --------------------- | --------- | -------------------------------------------------------------- |
| `Unnamed: 0`          | int64     | Index otomatis dari dataset   |
| `Title`               | object    | Judul buku                                                     |
| `Author`              | object    | Nama penulis buku                     |
| `Main Genre`          | object    | Genre utama buku (kategori genre utama)                        |
| `Sub Genre`           | object    | Sub-genre buku (kategori lebih spesifik dari genre utama)      |
| `Type`                | object    | Tipe buku (misal paperback, ebook, hardcover, dll.)            |
| `Price`               | object    | Harga buku (perlu diperhatikan formatnya, bisa perlu konversi) |
| `Rating`              | float64   | Rating rata-rata buku (biasanya dari 0 sampai 5)               |
| `No. of People rated` | float64   | Jumlah orang yang memberikan rating                            |
| `URLs`                | object    | Link atau URL ke halaman buku (biasanya untuk referensi)       |

Insight :
Dataset Books_df terdiri dari 7928 baris dan 10 kolom


Deskripsi variabel Genre_df

| Nama Kolom             | Tipe Data | Deskripsi Singkat                                      |
| ---------------------- | --------- | ------------------------------------------------------ |
| `Title`                | object    | Nama sub-genre                                         |
| `Number of Sub-genres` | int64     | Jumlah sub-genre yang termasuk dalam kategori tersebut |
| `URL`                  | object    | Link atau URL referensi untuk sub-genre tersebut       |

Insight :
Dataset Genre_df terdiri dari 35 baris dan 3 kolom


Deskripsi variabel Sub_Genre_df

| Nama Kolom     | Tipe Data | Deskripsi Singkat                                                               |
| -------------- | --------- | ------------------------------------------------------------------------------- |
| `Title`        | object    | Nama genre utama                                                                |
| `Main Genre`   | object    | Kategori genre utama, bisa jadi nama yang sama dengan `Title` atau variasi lain |
| `No. of Books` | float64   | Jumlah buku yang termasuk dalam genre tersebut                                  |
| `URLs`         | object    | Link atau URL referensi untuk genre tersebut                                    |

Insight :
Dataset Sub_Genre_df terdiri dari 329 baris dan 4 kolom

### Exploratory Data Analysis
#### 1. Distribusi Genre — Banyak Buku per Genre Utama
<p align="center">
  <img src="https://github.com/user-attachments/assets/ed8416bb-26fa-49d7-b04d-6ad975d201d9" width="600"/>
</p>

**Insight:**
 Grafik ini menunjukkan genre buku mana yang paling banyak jumlahnya:
 
- Buku Anak-anak paling banyak: Genre "Children's Books" memiliki jumlah buku terbanyak, yaitu lebih dari 1400 buku.

- Romantis dan Olahraga populer berikutnya: Genre "Romance" berada di urutan kedua, diikuti oleh "Sports". Keduanya juga memiliki jumlah buku yang signifikan.

- Banyak genre dengan jumlah lebih sedikit: Setelah beberapa genre teratas, ada banyak genre lain dengan jumlah buku yang lebih kecil, dan daftarnya cukup panjang. Ini menunjukkan variasi genre yang luas, meskipun beberapa genre jauh lebih dominan daripada yang lain.

#### 2. Distribusi Rating Buku
<p align="center">
  <img src="https://github.com/user-attachments/assets/61e720ac-7b63-42e2-8b8d-6060cc8949ea" width="1000"/>
</p>

**Insight:**
Grafik ini menunjukkan sebaran rating yang diterima buku-buku:

- Mayoritas buku dapat rating sangat tinggi: Sebagian besar buku mendapatkan rating antara 4.0 hingga 5.0, dengan puncak tertinggi ada di sekitar rating 4.5. Ini berarti banyak buku yang dinilai sangat baik.

- Rating rendah jarang ditemui: Sangat sedikit buku yang memiliki rating antara 1.0 hingga 3.5.

- Ada kelompok buku dengan rating nol: Terdapat sejumlah buku yang memiliki rating 0. Ini bisa berarti buku tersebut belum 
diberi rating, atau memang mendapat nilai nol.

- Cenderung ke nilai bagus: Secara keseluruhan, distribusi rating ini "miring ke kiri", yang artinya kebanyakan buku cenderung mendapatkan nilai di sisi kanan (nilai tinggi) skala rating.

#### 3. Distribusi Jumlah Orang yang Memberi Rating
<p align="center">
  <img src="https://github.com/user-attachments/assets/5979a354-5f32-4131-9649-63c7ecf680d2" width="1000"/>
</p>

**Insight :**
Grafik ini menunjukkan seberapa banyak orang yang memberikan rating untuk tiap buku:

- Kebanyakan buku dirating oleh sedikit orang: Sebagian besar buku (lebih dari 7000 buku) hanya mendapatkan rating dari sejumlah kecil orang. Ini ditunjukkan oleh batang yang sangat tinggi di sisi kiri grafik.
- Sedikit buku yang dirating oleh banyak orang: Hanya sejumlah kecil buku yang mendapatkan rating dari puluhan ribu hingga ratusan ribu orang. Jumlah buku ini menurun drastis seiring bertambahnya jumlah orang yang memberi rating.
- Distribusi "berat di kiri": Artinya, sangat umum buku hanya diulas oleh sedikit orang, dan sangat jarang ada buku yang diulas oleh kerumunan besar.

Intinya: Mayoritas buku hanya menerima ulasan dari sejumlah kecil pembaca, sementara hanya segelintir buku "super populer" yang berhasil menarik perhatian dan ulasan dari banyak sekali orang.

#### 4. Genre Paling Populer (Main Genre)
<p align="center">
  <img src="https://github.com/user-attachments/assets/056a234f-032b-4dd1-ab27-e050e389fdb4" width="1000"/>
</p>

**Insight :**

Grafik ini menunjukkan 10 genre buku yang paling banyak jumlahnya:

- Buku Anak-anak juaranya: Genre "Children's Books" adalah yang paling populer dengan jumlah buku terbanyak, mencapai lebih dari 1400 buku.
- Romantis dan Olahraga menyusul: Di posisi kedua ada "Romance" dengan sekitar 1100-an buku, diikuti oleh "Sports" di posisi ketiga dengan sekitar 850-an buku.
- Penurunan jumlah yang jelas: Setelah tiga besar, jumlah buku untuk genre-genre berikutnya menurun secara bertahap. Misalnya, "Literature & Fiction" dan "Teen & Young Adult" memiliki sekitar 350-400 buku.
- Variasi di 10 besar: Genre seperti "Computing", "Crafts, Home & Lifestyle", "Religion", "Medicine & Health Sciences", hingga "Reference" melengkapi daftar 10 besar, meskipun dengan jumlah buku yang lebih sedikit dibandingkan tiga teratas.

#### 5. Sub Genre Paling Populer
<p align="center">
  <img src="https://github.com/user-attachments/assets/fcd8412f-325d-4e00-98cc-019b64210d03" width="1000"/>
</p>

**Insight :**

Grafik ini menampilkan 10 sub-genre buku yang paling banyak ditemukan:

- Fantasi paling unggul: Sub-genre "Fantasy" menempati posisi teratas dengan jumlah buku sekitar 150.

- Beberapa sub-genre bersaing ketat: Di bawah fantasi, ada sekelompok sub-genre seperti "Education & Reference", "Biographies & Autobiographies", "Computer Science", "Reference", "Humour", "History", "Mathematics", dan "Biology & Life Sciences" yang jumlahnya hampir sama, yaitu sekitar 100 buku masing-masing.

- "Analysis & Strategy" melengkapi 10 besar: Sub-genre "Analysis & Strategy" berada di urutan kesepuluh dengan jumlah buku sekitar 50.

#### 6. Korelasi antara Rating dan Jumlah Orang yang Memberi Rating
<p align="center">
  <img src="https://github.com/user-attachments/assets/c65206cd-ec9e-41cc-b3c8-558b07884d7a" width="1000"/>
</p>

**Insight :**

Grafik ini menunjukkan hubungan antara nilai rating sebuah buku dengan seberapa banyak orang yang memberikan rating tersebut:

- Rating tinggi, bisa banyak pengulas: Buku-buku dengan rating tinggi (terutama antara 4.0 hingga 5.0) memiliki jumlah pengulas yang bervariasi, mulai dari sedikit hingga sangat banyak (ratusan ribu). Titik-titik paling atas (jumlah pengulas terbanyak) semuanya ada di rentang rating ini.

- Rating rendah, cenderung sedikit pengulas: Buku-buku dengan rating rendah (di bawah 3.5, terutama yang sangat rendah seperti 0-2) hampir selalu memiliki jumlah orang yang memberi rating sangat sedikit.

- Pola "corong" ke kanan atas: Secara umum, semakin tinggi rating sebuah buku (bergerak ke kanan), semakin besar kemungkinan buku tersebut mendapatkan banyak ulasan. Buku yang paling banyak diulas orang juga cenderung memiliki rating yang bagus.


### Data Quality Verification
#### Cek Data Books_df
<p align="center">
  <img src="https://github.com/user-attachments/assets/5370b5a1-0021-42d9-88ad-595bec0d119f" width="600"/>
</p>

**Insight :**
- Tidak ditemukan baris duplikat.
- Terdapat missing values sebanyak 21 baris pada kolom Author
- Nilai unik dalam Rating cukup bervariasi, dan mencakup rentang penuh dari 0.0 hingga 5.0.
- Semua kolom memiliki tipe data yang sesuai kecuali Price, yang masih berupa object.

#### Cek Data Genre_df
<p align="center">
  <img src="https://github.com/user-attachments/assets/d294a107-faba-4868-a48c-ea5a19c3df48" width="600"/>
</p>

**Insight :**
- Tidak ditemukan duplikat
- Tidak ada missing values

#### Cek Sub_Data Genre_df
<p align="center">
  <img src="https://github.com/user-attachments/assets/31b8d573-de17-4517-bc9e-112ebe066be2" width="600"/>
</p>

**Insight :**
- Tidak ditemukan duplikat
- Tidak ada missing values
- Kolom No. of Books bertipe float64, namun seharusnya secara logis merupakan bilangan bulat (jumlah buku).


## Data Preparation
### Data Cleaning
#### Data Cleaning — Books_df
1. Hapus kolom Unnamed: 0
Saya menghapus kolom Unnamed: 0 karna kolom fitur tersebut tidak diperlukan.
<p align="center">
  <img src="https://github.com/user-attachments/assets/bce41dce-23f7-45ce-95f6-1eb0ac5d2975" width="1000"/>
</p>

2. Tangani nilai missing pada kolom Author
Menghapus nilai missing pada kolom Author karna hanya sedikit.
<p align="center">
  <img src="https://github.com/user-attachments/assets/d9e3229d-2dba-4669-beaa-ccab31de37e1" width="1000"/>
</p>

3. Bersihkan dan konversi kolom Price
Mengganti tipe data pada kolom Price karna masih bertipe object.
<p align="center">
  <img src="https://github.com/user-attachments/assets/35af911e-68a7-432e-8c9f-252c9428e428" width="600"/>
</p>

#### Data Cleaning — Genre_df dan Sub_Genre_df
1. Konversi No. of Books ke int
Mengkonversi kolom No. of Books dan Number of Sub-genres kedalam tipe data integer.
<p align="center">
  <img src="https://github.com/user-attachments/assets/5d0f77bd-af28-4142-ad27-9c83ddd01b0d" width="600"/>
</p>

#### Menggabungkan Kolom
Menggabungkan kolom 'Main Genre', 'Sub Genre', dan 'Author' menjadi satu string per baris yang nanti nya akan digunakan sebagai representasi teks untuk menghitung kemiripan antar buku
<p align="center">
  <img src="https://github.com/user-attachments/assets/bd410fdd-163f-43af-aabd-c82b64500bad" width="600"/>
</p>

#### Encoding (TF-IDF Vectorization)
TF-IDF Vectorization
Untuk mengubah informasi teks seperti genre, sub-genre, dan penulis menjadi format numerik yang bisa diproses oleh sistem rekomendasi, digunakan metode TF-IDF (Term Frequency-Inverse Document Frequency).

TF-IDF merupakan teknik untuk merepresentasikan dokumen teks berdasarkan pentingnya setiap kata di dalam dokumen tersebut dibandingkan dengan seluruh dokumen lainnya. Kata-kata yang sering muncul di satu buku namun jarang muncul di buku lain akan memiliki bobot lebih tinggi.

Dengan TF-IDF, setiap buku direpresentasikan dalam bentuk vektor angka berdasarkan metadata-nya. Hasilnya adalah matriks TF-IDF, yang akan digunakan pada tahap modeling untuk menghitung kemiripan antar buku.
<p align="center">
  <img src="https://github.com/user-attachments/assets/6de39de8-a57b-4934-a894-218895fce438" width="600"/>
</p>

## Modeling
### Cosine Similarity
Model rekomendasi ini menggunakan pendekatan Content-Based Filtering yang mengandalkan kesamaan konten antar buku. Sistem memanfaatkan metadata seperti genre, sub-genre, dan penulis, yang telah diproses menjadi representasi numerik menggunakan TF-IDF pada tahap Data Preparation.

Kemudian, digunakan cosine similarity untuk mengukur tingkat kemiripan antar buku berdasarkan representasi tersebut. Semakin tinggi nilai cosine similarity, semakin mirip dua buku tersebut, dan sistem akan merekomendasikan buku-buku dengan kemiripan tertinggi.

<p align="center">
  <img src="https://github.com/user-attachments/assets/9239aa07-efab-4919-8e75-dd82642479d9" width="1000"/>
</p>

Setelah mendapatkan nilai cosine similarity antar buku menggunakan fungsi cosine_similarity, hasilnya disimpan dalam sebuah matriks bernama cosine_sim. Matriks ini berisi nilai kemiripan antar semua buku berdasarkan atribut kontennya, yang telah diproses menggunakan TF-IDF Vectorizer pada kolom gabungan content_features—yang mencakup Main Genre, Sub Genre, dan Author.

<p align="center">
  <img src="https://github.com/user-attachments/assets/11898b36-b0b2-4ccf-845d-40d8ce831f3d" width="1000"/>
</p>

Model merekomendasikan buku-buku yang memiliki kemiripan konten dengan buku "Harry Potter and the Philosopher's Stone", berdasarkan atribut Main Genre, Sub Genre, dan Author. Lima buku yang direkomendasikan adalah:

| Judul Buku                              | Penulis             | Rating |
| --------------------------------------- | ------------------- | ------ |
| Black Holes (L): The Reith Lectures     | Stephen Hawking     | 4.5    |
| The Kite Runner                         | Khaled Hosseini     | 4.6    |
| Greenlights                             | Matthew McConaughey | 4.6    |
| The Science of Storytelling             | Will Storr          | 4.5    |
| Fantastic Beasts and Where to Find Them | J.K. Rowling        | 4.5    |

Interpretasi

Kesesuaian berdasarkan penulis:
Buku "Fantastic Beasts and Where to Find Them" oleh J.K. Rowling sangat relevan karena berasal dari penulis yang sama. Ini menunjukkan bahwa sistem berhasil mengenali kesamaan konten berdasarkan penulis, yang menjadi bagian penting dari content_features.

Ragam genre dan subgenre:
Beberapa rekomendasi lain seperti "The Kite Runner" dan "Greenlights" mungkin tidak memiliki genre yang identik dengan Harry Potter, namun memiliki kesamaan pada kombinasi subgenre atau segmentasi penulis populer.

Kemiripan umum dalam struktur konten:
Buku "The Science of Storytelling" dan "Black Holes" menunjukkan bahwa meskipun berbeda tema, kemungkinan besar mereka memiliki fitur konten yang sering muncul bersama dengan Harry Potter—misalnya subgenre edukatif, populer, atau naratif kuat yang ditangkap oleh model TF-IDF.


### Kelebihan dan Kekurangan Pendekatan Content-Based Filtering (CBF)
#### Kelebihan Pendekatan Content-Based Filtering
a. Rekomendasi Berdasarkan Fitur Buku
Content-Based Filtering (CBF) merekomendasikan buku berdasarkan atribut kontennya, seperti genre utama, sub-genre, dan penulis. Sistem akan menyarankan buku-buku yang memiliki kemiripan dengan buku yang disukai pengguna.

Contoh: Jika seorang pengguna menyukai buku bergenre Fantasy karya J.K. Rowling, maka sistem akan merekomendasikan buku lain dari genre yang sama atau karya dari penulis serupa.

b. Tidak Bergantung pada Preferensi Pengguna Lain
CBF tidak membutuhkan data dari pengguna lain. Sistem tetap dapat merekomendasikan buku meskipun hanya satu pengguna yang membaca dan menilai buku tertentu.

Cocok untuk pengguna baru yang belum memiliki banyak interaksi atau ketika sistem belum memiliki cukup data komunitas pembaca.

c. Efektif untuk Buku Baru atau Kurang Populer
CBF dapat memberikan rekomendasi untuk buku-buku yang belum memiliki banyak pembaca atau rating, selama metadata (genre, sub-genre, penulis) tersedia.

Bermanfaat untuk memperkenalkan buku baru, buku indie, atau buku lokal yang belum banyak dikenal namun memiliki konten mirip dengan buku populer.

d. Rekomendasi Lebih Personal dan Dapat Dijelaskan
CBF memungkinkan sistem menjelaskan mengapa buku tertentu direkomendasikan, seperti:

“Buku ini ditulis oleh penulis yang sama dan memiliki genre Fantasy seperti buku yang Anda sukai sebelumnya.”

e. Mengurangi Bias Terhadap Buku Populer
Rekomendasi tidak hanya terfokus pada buku best-seller, tapi juga bisa mengangkat buku-buku berkualitas yang sesuai dengan minat personal pengguna.

Hal ini membantu pengguna menemukan hidden gem yang mungkin terlewat dalam sistem berbasis popularitas.

#### Kekurangan Pendekatan Content-Based Filtering
a. Terbatas pada Informasi Konten
CBF hanya menggunakan informasi dari atribut buku seperti genre dan penulis. Jika metadata tidak lengkap atau kurang informatif, sistem akan kesulitan memberikan rekomendasi yang akurat.

Misalnya, jika buku tidak memiliki genre atau penulis yang jelas, maka sistem tidak bisa menghitung kemiripan dengan baik.

b. Kurang Variatif (Over-Specialization)
CBF cenderung merekomendasikan buku-buku yang terlalu mirip dengan yang sudah dibaca pengguna sebelumnya, sehingga rekomendasi menjadi monoton.

Pengguna yang suka Fantasy mungkin hanya akan terus-menerus direkomendasikan buku Fantasy, padahal bisa jadi mereka juga tertarik pada Science Fiction atau Historical Fiction.

c. Tidak Menangkap Preferensi Sosial
Karena tidak mempertimbangkan preferensi atau rating dari pengguna lain, sistem ini tidak bisa merekomendasikan buku berdasarkan tren komunitas atau pengaruh sosial.

Berbeda dengan collaborative filtering, CBF tidak bisa menangkap pola seperti "buku ini disukai oleh pengguna yang juga menyukai buku X".

d. Sulit untuk Menemukan Hubungan Tak Terduga
CBF sulit memberikan rekomendasi lintas genre atau lintas penulis yang mungkin menarik. Hubungan semacam ini biasanya hanya bisa ditangkap oleh sistem yang mempelajari pola dari banyak pengguna (collaborative filtering).

Contoh: pengguna yang suka Horror ternyata juga suka Thriller Psikologis, tapi CBF tidak bisa menyarankan genre tersebut jika belum ada keterkaitan kontennya secara eksplisit.

e. Membutuhkan Metadata Berkualitas
Keakuratan rekomendasi sangat tergantung pada kualitas informasi konten. Jika informasi seperti genre, sub-genre, atau nama penulis tidak konsisten atau salah penulisan, maka hasil rekomendasi bisa menjadi tidak relevan.


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
Penerapan metrik dilakukan dengan membuat ground truth, lalu membuat fungsi evaluate_single_recommendation dengan parameter title, recommend_func, ground_truth, k=5.
<p align="center">
  <img src="https://github.com/user-attachments/assets/2e260944-dde0-489d-8db5-f53dfb7b8cfe" width="600"/>
</p>

Selanjutnya, fungsi tersebut dipanggil sehingga menampilkan output di bawah ini.
<p align="center">
  <img src="https://github.com/user-attachments/assets/ce20dafe-7e23-4277-84b3-13b75a258e2b" width="600"/>
</p>

**Insight:**
- Precision@5 = 1.0
Semua 5 buku yang direkomendasikan benar-benar relevan dengan buku yang dijadikan acuan ("Harry Potter and the Philosopher's Stone"). Tidak ada rekomendasi yang kurang tepat atau tidak sesuai.

- Recall@5 = 1.0
Dari semua buku relevan yang sebenarnya ada di data ground truth untuk buku tersebut, sistem berhasil menemukan semuanya dalam rekomendasi top 5. Ini berarti sistem tidak melewatkan buku relevan sama sekali dalam rekomendasi teratas.

- F1-Score@5 = 1.0
Kombinasi precision dan recall sempurna, menunjukkan performa sistem rekomendasi sangat baik pada contoh ini.

- Relevant Found
Daftar buku yang direkomendasikan dan dianggap relevan oleh sistem, seperti:
1. The Kite Runner
2. Black Holes (L) : The Reith Lectures
3. Fantastic Beasts and Where to Find Them
4. The Science of Storytelling
5. Greenlights

## Kesimpulan
### 1. Menemukan Novel Paling Populer dan Disukai
<p align="center">
  <img src="https://github.com/user-attachments/assets/aca5bb43-e25c-4001-9a38-af3ac7513299" width="800"/>
</p>

**Insight:**

1. Semua Buku Sangat Disukai Pembaca
Semua buku memiliki rating sangat tinggi, yaitu 4.9 hingga 5.0.

Ini menunjukkan bahwa buku-buku ini sangat memenuhi ekspektasi pembaca.

2. Genre Beragam dan Tidak Terbatas pada Fiksi. Bukan hanya novel atau fiksi yang populer, tapi juga:

- Bisnis & Keuangan: Spreading Joy (5.0, 1274 rating)

- Travel Guide: Bucket List Family Travel (5.0, 239 rating)

- Komik/Manga: One Piece Vol. 104 (4.9, 227 rating)

- Health/Science: Vax-Unvax, Environmental Studies

- Religi: The Holy Quran (4.9, 788 rating)

Ini menunjukkan bahwa preferensi pembaca sangat beragam, mencakup edukasi, hiburan, religi, dan inspirasi pribadi.

3. Penulis Terkenal dan Pengaruh Sosial
Penulis seperti Eiichiro Oda (One Piece) dan Allah (The Holy Quran) menunjukkan:

- Kekuatan nama besar dan pengaruh dalam menarik rating tinggi.

- Buku dari figur publik atau influencer juga menonjol, seperti:

- Trent Shelton – Seorang motivator dengan buku “Straight Up” (4.9, 1070 rating)


### 2. Mengetahui Fitur yang Mempengaruhi Kesukaan Pembaca
<p align="center">
  <img src="https://github.com/user-attachments/assets/86d692c5-cca2-4d13-9208-f3ba3c1c218f" width="800"/>
</p>

**Insight:**

Heatmap ini menunjukkan rata-rata rating buku berdasarkan kombinasi genre utama dan penulis populer:

- Rating Sempurna (5.00): Beberapa penulis mencapai rating sempurna 5.00 di genre tertentu:
"Oswaal Editorial Board" untuk genre "Reference".
"Maple Press" untuk genre "Teen & Young Adult".

- Penulis dengan Rating Tinggi Konsisten:
1. J.K. Rowling mendapatkan rating sangat tinggi (sekitar 4.7) untuk genre "Children's Books", "Fantasy, Horror & Science Fiction", dan "Literature & Fiction".
2. Sudha Murty secara konsisten mendapatkan rating tinggi (sekitar 4.50 - 4.63) di berbagai genre seperti "Biographies", "Children's Books", "History", "Literature & Fiction", "Society & Social Sciences", dan "Teen & Young Adult".
3. Rick Riordan juga sangat tinggi (4.71) di "Children's Books".

- Genre Populer dengan Rating Tinggi:
1. "Children's Books" secara umum mendapat rating tinggi dari banyak penulis populer yang tercantum.
2. "Literature & Fiction" juga menunjukkan rating tinggi dari beberapa penulis seperti J.K. Rowling dan Maple Press.

- Spesialisasi Penulis: Beberapa penulis menunjukkan keunggulan di genre spesifik, seperti "Jeff Kinney" di "Children's Books" (4.58) dan "Comics & Mangas" (4.53).

- Variasi Rating: Meskipun sebagian besar rating yang ditampilkan tinggi (di atas 4.3), ada variasi tergantung pada penulis dan genre. Warna yang lebih gelap menunjukkan rating yang lebih tinggi.

Intinya: Penulis-penulis populer dalam daftar ini umumnya memiliki rata-rata rating yang sangat baik di genre-genre utama mereka, dengan beberapa kombinasi penulis dan genre bahkan mencapai rating sempurna. Genre seperti "Children's Books" secara konsisten menerima rating tinggi dari berbagai penulis ternama.

### 3. Mengetahui cara membangun sistem rekomendasi novel yang memberikan saran sesuai dengan preferensi pengguna, khususnya berdasarkan genre dan metadata buku.
Untuk menjawab permasalahan ini, dibuatlah sebuah fungsi bernama recommend_books(). Fungsi ini merupakan implementasi dari sistem rekomendasi berbasis Content-Based Filtering yang dirancang untuk memberikan saran buku kepada pengguna berdasarkan buku yang mereka sukai.

Sistem ini bekerja dengan meminta input berupa judul buku, kemudian mencocokkannya dengan data dalam matriks cosine similarity antar buku. Jika judul buku ditemukan, sistem akan mencari dan menampilkan 5 buku teratas yang memiliki tingkat kemiripan tertinggi dengan buku input, berdasarkan atribut Main Genre, Sub Genre, dan Author yang telah dikombinasikan dan diubah menjadi representasi numerik menggunakan TF-IDF Vectorizer.

Nilai cosine similarity digunakan untuk mengukur kedekatan antar buku dalam ruang fitur. Semakin tinggi nilai similarity (mendekati 1), maka semakin mirip dua buku tersebut dari segi kontennya.

Hasil rekomendasi disajikan secara jelas kepada pengguna dalam bentuk tabel yang berisi judul buku, nama penulis, dan rating, sehingga pengguna dapat memahami alasan di balik rekomendasi yang diberikan. Dengan pendekatan ini, sistem menjadi relevan, personal, dan efektif dalam membantu pengguna menemukan buku baru yang sesuai dengan preferensinya.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7086cc2d-6b70-475b-b6a6-97177535b407" width="800"/>
</p>

Output yang dihasilkan adalah sebagai berikut.
<p align="center">
  <img src="https://github.com/user-attachments/assets/854cff01-9cdf-4cbb-bace-cc2c7535b224" width="800"/>
</p>

Hasil sistem rekomendasi buku berdasarkan input "Harry Potter and the Philosopher's Stone" menunjukkan performa yang baik dalam menyajikan buku-buku dengan karakteristik serupa. Hal ini terlihat dari munculnya beberapa buku lain dari penulis yang sama, J.K. Rowling, seperti "Fantastic Beasts and Where to Find Them" yang menempati posisi tinggi dalam daftar dengan nilai similarity yang cukup besar. Ini mengindikasikan bahwa sistem mampu mengenali konsistensi gaya penulisan dan tema cerita khas dari penulis tersebut.

Selain itu, rekomendasi juga mencakup buku-buku dari penulis lain dengan genre fantasi dan petualangan yang serupa, seperti karya-karya dari Stephen Hawking dan Will Storr yang memiliki narasi dan tema yang relevan dengan genre dan gaya Harry Potter. Buku-buku tersebut memiliki elemen cerita dan gaya penulisan yang mendekati, sehingga cocok untuk pembaca yang menyukai Harry Potter.

Nilai similarity yang konsisten tinggi menunjukkan bahwa sistem ini cukup stabil dan relevan dalam memberikan rekomendasi buku berdasarkan kesamaan genre, gaya penulisan, dan metadata seperti penulis dan tema cerita.

