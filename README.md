# Sistem Rekomendasi Film - Gratia Yudika Morado Silalahi

## Latar Belakang  
Dalam era digital yang serba cepat saat ini, masyarakat dihadapkan pada banjir informasi dan pilihan, termasuk dalam hal hiburan seperti film. Platform penyedia layanan streaming seperti Netflix, Disney+, dan Amazon Prime menghadirkan ribuan judul film dan serial, yang membuat pengguna kerap kebingungan dalam memilih tontonan yang sesuai dengan preferensinya. Di sinilah pentingnya peran sistem rekomendasi yang cerdas dan personal.

Sistem rekomendasi film bertujuan untuk membantu pengguna menemukan film yang relevan dan menarik bagi mereka, sekaligus meningkatkan kepuasan pengguna dan durasi interaksi mereka dengan platform. Dua pendekatan utama dalam sistem rekomendasi adalah Content-Based Filtering dan Collaborative Filtering.

Content-Based Filtering merekomendasikan film berdasarkan kesamaan konten (seperti genre, sutradara, aktor, atau sinopsis) dengan film yang pernah disukai oleh pengguna. Dengan kata lain, sistem mengenali pola preferensi pengguna secara individual. Collaborative Filtering, di sisi lain, memanfaatkan perilaku dan penilaian (rating) dari banyak pengguna. Sistem ini merekomendasikan film berdasarkan kesamaan pola penilaian antar pengguna, bahkan jika konten film yang disarankan belum pernah dikonsumsi oleh pengguna tersebut sebelumnya.

Menggabungkan kedua pendekatan ini memungkinkan sistem rekomendasi menjadi lebih akurat dan adaptif. Pendekatan hybrid ini dapat mengatasi berbagai keterbatasan yang muncul jika hanya menggunakan salah satu pendekatan, seperti cold-start problem (pada collaborative filtering) dan keterbatasan generalisasi (pada content-based filtering).

## Business Understanding

### Problem statement
Dengan latar belakang tersebut, rumusan masalah yang ingin diolah pada proyek ini meliputi:
1. Bagaimana merancang sistem rekomendasi menggunakan algoritma content-based filtering yang memanfaatkan informasi genre film?
2. Bagaimana merancang sistem rekomendasi berbasis collaborative filtering dengan menggunakan data interaksi atau rating pengguna?
3. Sejauh mana performa algoritma content-based filtering dan collaborative filtering yang telah dibangun dalam memberikan rekomendasi yang relevan?

### Goals  
Dengan begitu, tujuan yang akan dicapai pada proses penyelesaian masalah ini adalah sebagai berikut
1. Mengetahui proses pembangunan sistem rekomendasi film berdasarkan genre dengan menggunakan algoritma content-based filtering.
2. Mengetahui proses pembangunan sistem rekomendasi film berdasarkan aktivitas rating pengguna dengan menggunakan algoritma collaborative filtering.
3. Mengetahui kualitas kinerja kedua algoritma yang digunakan dalam memberikan rekomendasi yang tepat bagi pengguna.

### Solution Statement  
Proses implementasi nyata dari goal/tujuan di atas meliputi:
1. **Implementasi Content-Based Filtering untuk Genre Film**
Mengembangkan sistem rekomendasi yang menganalisis karakteristik genre film melalui representasi vektor TF-IDF. Sistem akan menghitung tingkat kemiripan antar film menggunakan cosine similarity untuk menghasilkan rekomendasi berdasarkan preferensi genre yang telah dipilih pengguna sebelumnya.
2. **Implementasi Collaborative Filtering Berbasis Rating Pengguna**
Membangun sistem rekomendasi dengan memanfaatkan pola rating dan preferensi pengguna melalui pendekatan machine learning. Algoritma akan mengidentifikasi kesamaan perilaku rating antar pengguna untuk memprediksi preferensi film yang belum ditonton.
3. **Evaluasi Performa Sistem Rekomendasi**
Melakukan assessment kualitas kedua algoritma menggunakan metrik evaluasi yang sesuai: Precision untuk mengukur ketepatan rekomendasi content-based filtering, dan Root Mean Square Error (RMSE) untuk mengevaluasi akurasi prediksi rating pada collaborative filtering, guna menentukan efektivitas masing-masing pendekatan dalam memberikan rekomendasi yang relevan.


## Data Understanding  

### Deskripsi Dataset  
Dataset yang digunakan diambil dari repository kaggle dengan informasi berikut.
- Nama file: Movies & Ratings for Recommendation System
- Author: Nicoleta Cilibiu
- Tautan: https://www.kaggle.com/datasets/nicoletacilibiu/movies-and-ratings-for-recommendation-system

Dataset yang digunakan dalam proyek ini terdiri dari dua file utama yang saling terhubung melalui kolom movieId, yaitu Data Movies dan Data Ratings. Kedua dataset ini membentuk struktur data yang komprehensif untuk membangun sistem rekomendasi film dengan pendekatan content-based dan collaborative filtering.

#### Data Film.  
Berjumlah 9.742 baris data dengan 3 kolom sebagai representasi film.  
- **movieId**: Data ID unik film.  
- **title**: Judul film beserta tahun rilis.  
- **genres**: menyimpan data genre film.

Data Film berperan sebagai master data film yang berisi **9.742** film unik dengan 3 kolom informasi utama. Setiap film memiliki identifikasi unik melalui **movieId, informasi judul lengkap beserta tahun rilis(title), dan karakteristik genre(genres)** . Yang menarik dari kolom genres adalah kemampuannya menyimpan multiple genre dalam satu film yang dipisahkan dengan delimiter "|", memungkinkan representasi yang lebih akurat terhadap kompleksitas kategorisasi film modern.

#### Data Ratings  
Berjumlah 100.836 baris data dengan 4 kolom sebagai representasi rating film.  
- **userId**: Data ID unik pengguna.  
- **movieId**: Data ID unik film.  
- **rating**:  rating film oleh pengguna (0.5 – 5.0).  
- **timestamp**: Waktu saat rating diberikan, formatnya memakai UNIX timestamp.  

Data Ratings merupakan dataset interaksi pengguna yang jauh lebih besar dengan **100.836 baris data dan 4 kolom**. Dataset ini mencatat aktivitas rating pengguna terhadap film dengan struktur relasional yang jelas - userId sebagai identifikasi pengguna, movieId yang menghubungkan dengan data movies, rating dengan skala 0.5 hingga 5.0 yang memberikan granularitas penilaian yang baik, dan timestamp untuk melacak kronologi aktivitas rating.

Perbandingan ukuran kedua dataset menunjukkan karakteristik yang menarik: rasio 1:10 antara jumlah film dan rating mengindikasikan rata-rata setiap film mendapat sekitar 10 rating dari pengguna. Hal ini menunjukkan distribusi data yang cukup seimbang untuk membangun sistem rekomendasi, dimana terdapat cukup informasi konten (film) dan interaksi pengguna (ratings) untuk mengimplementasikan kedua algoritma yang direncanakan. Struktur relasional melalui movieId memungkinkan integrasi seamless antara analisis berbasis konten genre dan pola preferensi pengguna.  


## Exploratory Data Analysis

### Deskripsi variabel
#### Dataset Film
Dataset Film memiliki struktur data yang bersih tanpa missing values, dengan total memory usage 228.5+ KB untuk 9.742 entri film.
- movieId (int64): Identifier unik untuk setiap film dalam dataset. Nilai integer yang berfungsi sebagai primary key dan foreign key untuk relasi dengan dataset ratings.
- title (object): Judul lengkap film beserta tahun rilis. Format teks yang umumnya berisi "Judul Film (Tahun)", memberikan informasi identitas film yang mudah dibaca.
- genres (object): Kategori genre film. String dengan multiple values dipisahkan delimiter "|", memungkinkan satu film memiliki beberapa genre sekaligus.

#### Dataset Ratings
Dataset Ratings memiliki volume data yang signifikan lebih besar dengan 100.836 entri dan memory usage 3.1 MB, juga tanpa missing values.
- userId (int64): Identifier unik untuk setiap pengguna. Nilai integer yang mengidentifikasi pengguna unik dalam sistem, memungkinkan tracking preferensi individual.
- movieId (int64): Identifier unik film yang diberi rating. Foreign key yang menghubungkan dengan dataset movies, memungkinkan join operations untuk analisis komprehensif.
- rating (float64): Nilai rating yang diberikan pengguna. Skala numerik 0.5-5.0 dengan presisi desimal, memberikan granularitas penilaian yang detail untuk collaborative filtering.
- timestamp (int64): Waktu pemberian rating. Format UNIX timestamp (detik sejak 1 Januari 1970), memungkinkan analisis temporal dan tracking evolusi preferensi.

Kedua dataset menunjukkan kualitas data yang excellent dengan 100% data completeness (tidak ada missing values). Tipe data sudah optimal untuk computational efficiency - integer untuk identifiers, float untuk ratings yang memerlukan presisi, dan object untuk data tekstual. Konsistensi tipe data movieId sebagai int64 di kedua dataset memastikan integritas relasional yang kuat untuk operasi join dan analisis cross-dataset.

### Penggambaran dataframe (describe)

#### Dataset Film
- ID film dimulai dari 1 hingga 193.609, menunjukkan bahwa movieId tidak berurutan dan kemungkinan besar mengandung celah (gap) antar ID.
- Nilai tengah (median) movieId adalah 7.300, tetapi nilai maksimum jauh lebih tinggi, menandakan distribusi miring ke kanan (right-skewed) — sebagian besar film memiliki movieId lebih kecil dibanding outlier di ujung atas.

#### Dataset Rating
- Jumlah pengguna: antara userId 1 hingga 610 — menandakan ada 610 pengguna unik.
- Nilai rating berada pada rentang 0.5 hingga 5.0, dengan rata-rata ~3.5, menunjukkan bahwa pengguna cenderung memberi rating netral hingga positif.
- Variasi rating (std = 1.04) cukup rendah → mayoritas rating terpusat di sekitar nilai tengah.
- Timestamp menunjukkan rentang waktu yang luas: dari sekitar 1996 hingga 2018, menunjukkan data historis yang panjang, cocok untuk analisis temporal (misalnya perubahan preferensi film dari waktu ke waktu).

### Missing Value, Data Duplikat, Outlier  
Terbukti bahwa tidak ada missing value dan outlier pada dataset. Untuk judul dan ID, ditemukan beberapa duplikasi di mana 1 judul memiliki lebih dari 1 ID. Namun tidak terlalu berpengaruh pada pengolahan, sehingga tidak perlu untuk dihapus.
![Gambar Outlier](<./Dokumentasi rekomendasi/Outlier.png>)



## Analisis data tunggal - Univariate  
Dilakukan sebagai bentuk analisis pemahaman bentuk distribusi variabel secara individual. Tujuannya jelas, untuk memberikan bentuk paparan informasi sebelum proses modelling dimulai.

### Analisis Distribusi Genre Film  
![Grafik Barplot Genre Film](<./Dokumentasi rekomendasi/Distribusi genre.png>)
Berdasarkan eksplorasi awal terhadap kolom genres, ditemukan bahwa dataset ini memuat sebanyak 20 genre unik. Genre Drama menjadi yang paling dominan dalam koleksi film yang tersedia, disusul oleh Comedy dan Thriller sebagai genre terbanyak berikutnya. Di sisi lain, terdapat beberapa genre yang jarang muncul, seperti Film-Noir dan Western, yang hanya diwakili oleh sedikit film. Menariknya, terdapat 34 film yang tidak memiliki informasi genre sama sekali. Karena proyek ini akan membangun sistem rekomendasi berbasis konten (content-based filtering) yang bergantung pada atribut genre sebagai fitur utama, maka film-film tanpa genre ini sebaiknya dikeluarkan dari analisis untuk menjaga konsistensi dan keakuratan hasil model.


### Analisis Distribusi Rating Pengguna  
 ![Grafik Univariate Numeric Data](<./Dokumentasi rekomendasi/Distribusi rating.png>)
 Mayoritas pengguna cenderung memberikan penilaian di kisaran **moderat**, yakni antara **3.0 hingga 4.0**, dengan **rating 4.0** menjadi yang paling sering diberikan. Sementara itu, **rating sempurna (5.0)** juga cukup banyak muncul, menunjukkan bahwa pengguna relatif sering memberikan apresiasi tinggi, terutama jika dibandingkan dengan frekuensi rating rendah yang lebih jarang diberikan.
 

### Distribusi Film Dengan Akumulasi Rating - Top 20 rating tertinggi  
 ![Grafik Univariate Numeric Data](<./Dokumentasi rekomendasi/Top 20.png>)
 Tahun 1990-an menjadi tahun terbanyak dari 20 film terbaik yang ada. Faktor bahwa di zaman itu hiburan belum terlalu bervariasi menjadi salah 1 faktor terkuat dari analisis tersebut
  


## Data Preparation  
Langkah-langkah preparation data untuk proses selanjutnya mencakup penghapusan entri film yang tidak memiliki informasi genre, yaitu yang bernilai '(no genres listed)', karena informasi tersebut diperlukan untuk model content-based filtering. Selanjutnya, dilakukan penggabungan antara data film dan rating untuk memperoleh informasi yang lebih komprehensif mengenai setiap film beserta nilai rating yang diterimanya. Selain itu, data duplikat pada kolom judul film juga dihapus guna memastikan setiap film direpresentasikan secara unik dalam proses analisis.  

### Data Preparation Algoritma Content-Based Filtering
Pada tahap preparation data untuk sistem rekomendasi berbasis genre, dilakukan beberapa langkah penting untuk menyusun data menjadi format yang siap digunakan dalam perhitungan kemiripan antar film. Pertama, dilakukan penyusunan ulang (data formatting) terhadap data film agar menjadi struktur yang konsisten, terutama pada kolom movieId, title, dan genres. Tujuannya adalah memastikan data memiliki format yang seragam sehingga memudahkan proses pemrosesan lebih lanjut. Selanjutnya, dilakukan penghapusan kata-kata umum (stopwords) dalam bahasa Inggris dari teks pada kolom genre menggunakan pustaka pemrosesan bahasa alami. Ini bertujuan untuk menyaring hanya kata-kata yang relevan dan memiliki makna penting dalam konteks genre film. Kemudian, teks genre diolah melalui proses tokenisasi dan vektorisasi TF-IDF (Term Frequency–Inverse Document Frequency) menggunakan TfidfVectorizer dari pustaka scikit-learn. Proses ini mengubah data teks menjadi representasi numerik berdasarkan frekuensi relatif dan kepentingan setiap token di seluruh dataset, sehingga menghasilkan matriks fitur yang dapat digunakan untuk menghitung kemiripan antar film. Setelah TF-IDF dihitung, dilakukan ekstraksi daftar fitur/token unik yang mewakili genre-genre dalam bentuk kolom, agar hasilnya dapat dianalisis dan ditafsirkan lebih mudah. Matriks TF-IDF yang awalnya dalam bentuk sparse matrix kemudian dikonversi menjadi dense matrix, agar data lebih mudah ditampilkan dan divisualisasikan. Terakhir, hasil matriks TF-IDF disusun ke dalam sebuah DataFrame, dengan baris berlabel judul film (title) dan kolom berisi token-token genre, sehingga struktur data akhir dapat digunakan dalam analisis dan sistem rekomendasi content-based filtering.


### Data Preparation untuk Algoritma Collaborative Filtering  
Pada tahap preprocessing data untuk sistem rekomendasi, beberapa langkah penting dilakukan agar data siap digunakan dalam pembangunan model. Pertama, kolom timestamp dihapus karena informasi waktu dianggap tidak relevan dalam model berbasis rating yang akan dibangun, sehingga penghapusan ini membantu menyederhanakan data. Selanjutnya, data userId dan movieId dilakukan encoding menjadi nilai numerik yang berurutan mulai dari 0 hingga N-1. Proses encoding ini sangat penting agar data dapat langsung dimanfaatkan sebagai input dalam model machine learning, khususnya untuk embedding layer pada metode collaborative filtering. Setelah itu, jumlah pengguna dan film unik dihitung untuk menentukan dimensi embedding serta struktur model yang sesuai.
Tipe data kolom rating kemudian dikonversi menjadi float32 untuk mengoptimalkan penggunaan memori dan memastikan kompatibilitas dengan framework seperti TensorFlow saat pelatihan model berlangsung. Nilai minimum dan maksimum rating juga diperiksa guna memahami distribusi skor serta sebagai bahan pertimbangan apakah perlu dilakukan normalisasi sebelum proses pelatihan. Agar model tidak mempelajari pola urutan data yang tidak relevan, dataset ratings_new diacak terlebih dahulu. Pengacakan ini penting untuk memastikan model dapat melakukan generalisasi dengan baik dan menghindari bias terhadap urutan data asli.
Kemudian, userId dan movieId asli dimapping ke indeks numerik berurutan dari 0 hingga N-1, sehingga setiap user dan film memiliki representasi numerik konsisten yang siap digunakan sebagai input model. Untuk membantu proses pembelajaran model, nilai rating juga dinormalisasi menggunakan metode min-max normalization agar nilainya berada dalam rentang 0 hingga 1. Terakhir, data dibagi menjadi dua bagian, yaitu 80% untuk pelatihan (training set) dan 20% untuk validasi (validation set). Pembagian ini bertujuan agar performa model dapat dievaluasi secara objektif pada data yang tidak pernah dilihat selama proses pelatihan, sehingga hasil evaluasi menjadi lebih representatif.


## Modeling  

### Content-Based Filtering  
Model Content-Based Filtering dikembangkan melalui fungsi movie_recommendations, yang dirancang untuk memberikan rekomendasi film berdasarkan kemiripan konten—dalam hal ini, genre—dengan memanfaatkan cosine similarity. Fungsi ini memanfaatkan matriks TF-IDF yang telah dibentuk sebelumnya dari data genre untuk mewakili setiap film dalam bentuk vektor numerik. Kemudian, kemiripan antar film dihitung menggunakan metode cosine similarity dari pustaka scikit-learn, yang menghasilkan matriks berisi nilai kemiripan antara setiap pasangan film berdasarkan kemiripan genre mereka.
Untuk memberikan rekomendasi, sistem akan mencari film dengan skor kemiripan tertinggi terhadap film yang telah ditonton atau diberi rating oleh pengguna. Fungsi ini menerima judul film sebagai input utama, lalu mengambil skor kemiripan antara film input dengan seluruh film lainnya dari matriks cosine similarity (similarity_data). Berdasarkan skor tersebut, sistem mengidentifikasi dan memilih sejumlah film (k) dengan nilai kemiripan tertinggi sebagai kandidat rekomendasi. Film yang menjadi input akan dikecualikan dari hasil rekomendasi untuk menghindari duplikasi. Akhirnya, daftar film hasil rekomendasi ini digabungkan dengan data atribut film seperti judul dan genre, sehingga informasi yang disajikan kepada pengguna menjadi lengkap dan informatif. Berikut ini merupakan contoh hasil rekomendasi film yang dihasilkan oleh sistem.
![hasil_rekomendasi](<./Dokumentasi rekomendasi/Simulasi Content Based.jpg>)

## Model Colaborative Filtering
Collaborative Filtering adalah pendekatan dalam sistem rekomendasi yang mengandalkan pola interaksi antara pengguna dan item tanpa mempertimbangkan isi atau konten dari item itu sendiri. Dalam proyek ini, metode yang digunakan adalah Neural Collaborative Filtering, yang dibangun menggunakan pendekatan Keras Subclassing API. Model terdiri dari dua layer embedding, masing-masing untuk `userId` dan `movieId`, dengan dimensi 50. Kedua embedding ini kemudian digabung (concatenate) dan diteruskan ke sebuah layer `Dense` dengan fungsi aktivasi ReLU, yang berfungsi menangkap interaksi non-linear antara pengguna dan film. Output dari model berupa satu neuron dengan aktivasi sigmoid, yang menghasilkan prediksi skor dalam rentang 0 hingga 1, mengindikasikan kemungkinan seorang pengguna menyukai suatu film.
Untuk melatih model, digunakan fungsi loss `Binary Crossentropy`, yang sesuai dalam konteks klasifikasi biner seperti menilai apakah pengguna akan menyukai film atau tidak. Proses optimisasi dilakukan menggunakan algoritma Adam dengan learning rate sebesar 0.001, yang dikenal efektif dalam mempercepat konvergensi model. Model dilatih selama 30 epoch dengan ukuran batch sebesar 8, memanfaatkan data pelatihan dan validasi untuk mengoptimalkan performa sekaligus mencegah overfitting. Pendekatan ini memungkinkan sistem rekomendasi memberikan prediksi personalisasi yang lebih akurat berdasarkan pola preferensi pengguna. Berikut contoh hasil rekomendasi film yang diperoleh dari model, berdasarkan input `userId` yang dipilih secara acak:
![hasil_precision](<./Dokumentasi rekomendasi/Simulasi Collaborative.jpg>)

## Evaluasi 
### Evaluasi Content-Based Filtering
**Precision** adalah matriks evaluasi kinerja model sistem rekomendasi yang mengukur seberapa banyak rekomendasi yang diberikan oleh sistem benar-benar relevan atau sesuai dengan preferensi pengguna. Secara matematis :

$$
\text{Precision} = \frac{\text{Jumlah item relevan yang direkomendasikan}}{\text{Jumlah total item yang direkomendasikan}}
$$

#### Alasan Memilih Precision sebagai Metrik Evaluasi
Precision dipilih sebagai metrik evaluasi dalam sistem rekomendasi karena fokus utamanya adalah pada kualitas dari hasil rekomendasi yang diberikan. Dalam pendekatan content-based filtering, keakuratan dalam menyajikan item yang benar-benar relevan kepada pengguna menjadi prioritas utama agar sistem terasa berguna dan mampu meningkatkan kepuasan pengguna. Precision mengukur proporsi item yang relevan dari total item yang direkomendasikan, sehingga memberikan interpretasi yang jelas dan mudah dipahami. Nilai precision yang tinggi menunjukkan bahwa sebagian besar rekomendasi yang diberikan memang sesuai dengan preferensi pengguna.
Selain itu, precision sangat cocok digunakan dalam konteks sistem rekomendasi yang hanya menampilkan daftar item dalam jumlah terbatas, seperti top-5 atau top-10 film. Dalam skenario semacam ini, lebih penting memastikan bahwa setiap item dalam daftar pendek tersebut benar-benar relevan, daripada mengetahui seberapa banyak item relevan yang belum direkomendasikan. Karena precision tidak bergantung pada jumlah total item relevan yang tersedia di seluruh database, metrik ini tetap valid meskipun cakupan sistem terbatas. Hal ini menjadikannya metrik yang efisien dan relevan untuk mengevaluasi kualitas rekomendasi dalam skala kecil maupun sistem yang bersifat real-time.

Berbeda dengan recall yang membutuhkan data lengkap tentang semua item relevan, precision hanya fokus pada hasil rekomendasi yang muncul. Ini memudahkan evaluasi terutama ketika data lengkap sulit diketahui. Evaluasi sistem rekomendasi dilakukan dengan menjalankan fungsi movie_recommendations('Toy Story (1995)'), yang bertujuan untuk menghasilkan daftar film berdasarkan kemiripan konten genre dengan film acuan. Untuk menilai seberapa relevan hasil rekomendasi tersebut, digunakan metrik precision, dengan pendekatan evaluasi manual: setiap film dalam daftar rekomendasi diperiksa apakah mengandung seluruh genre yang dimiliki oleh Toy Story (1995). Relevansi dihitung dari perbandingan jumlah film yang memenuhi kriteria tersebut terhadap jumlah film yang direkomendasikan.

Dalam pengujian ini, rekomendasi dibatasi pada top-5, yaitu lima film teratas dengan skor kemiripan tertinggi. Berdasarkan hasil evaluasi, kelima film tersebut terbukti mengandung genre yang sama dengan film acuan. Dengan demikian, precision yang dihasilkan adalah 5 dibagi 5, atau 100%. Hasil ini menunjukkan bahwa sistem secara efektif mampu mencocokkan film berdasarkan genre, dan seluruh hasil rekomendasi dianggap relevan. Evaluasi ini memperkuat bukti bahwa pendekatan content-based filtering yang digunakan mampu bekerja dengan baik dalam menghasilkan rekomendasi yang tepat sasaran dari sisi kesamaan konten.
![Precision](<./Dokumentasi rekomendasi/Precision.jpg>)



### Evaluasi Collaborative filtering
**Root Mean Squared Error (RMSE)** dipilih sebagai metrik evaluasi untukmodel collaborative filtering ini. RMSE 
memberikan gambaran seberapa besar rata-rata kesalahan prediksi model dibandingkan rating asli dari pengguna, dengan 
satuan yang sama seperti rating.

Rumus RMSE:

$$
\text{RMSE} = \sqrt{\frac{1}{N} \sum_{i=1}^N (y_i - \hat{y}_i)^2}
$$

di mana:

- $y_i$ adalah rating asli,
- $\hat{y}_i$ adalah rating prediksi,
- $N$ adalah jumlah data.

#### Hasil Evaluasi
Nilai Root Mean Squared Error (RMSE) akhir pada data pelatihan sebesar 0.1780 menunjukkan bahwa model mampu memprediksi rating film dengan rata-rata kesalahan sekitar 0.18 poin. Hal ini menandakan bahwa model telah belajar cukup baik dari data training yang tersedia. Sementara itu, nilai RMSE pada data validasi sebesar 0.1999 mencerminkan performa model terhadap data yang belum pernah dilihat sebelumnya. Selisih yang kecil antara nilai RMSE training dan validasi mengindikasikan bahwa model memiliki kemampuan generalisasi yang baik dan tidak menunjukkan gejala overfitting yang signifikan. Secara keseluruhan, RMSE di bawah 0.3 pada skala rating 1 hingga 5 dianggap cukup baik, yang berarti model dapat memberikan prediksi rating yang akurat dan andal. Ini memperkuat keyakinan bahwa model collaborative filtering yang dibangun layak digunakan untuk merekomendasikan film sesuai dengan preferensi pengguna.
![RMSE](<./Dokumentasi rekomendasi/rmse colaborative.png>)

- 
## Penyelesaian Permasalahan
1. Algoritma Content-Based Filtering dikembangkan dengan mendasarkan pada informasi genre dari masing-masing film. Proses diawali dengan pembersihan data genre untuk memastikan hanya informasi relevan yang digunakan. Selanjutnya, dilakukan ekstraksi fitur menggunakan metode TF-IDF (Term Frequency-Inverse Document Frequency) guna mengubah genre menjadi representasi numerik. Setelah representasi genre diperoleh, kemiripan antar film dihitung menggunakan cosine similarity Melalui pendekatan ini, sistem mampu merekomendasikan film yang memiliki genre serupa dengan film yang sebelumnya disukai pengguna, tanpa bergantung pada preferensi atau interaksi dari pengguna lain.
2. Algoritma Collaborative Filtering dibangun menggunakan pendekatan Neural Collaborative Filtering yang berbasis machine learning. Model ini dirancang dengan memanfaatkan embedding layer untuk mempelajari representasi laten dari pengguna dan film berdasarkan data rating yang tersedia. Representasi ini kemudian dikombinasikan menggunakan operasi dot product untuk memperkirakan rating yang mungkin diberikan pengguna terhadap film yang belum pernah ditonton. Dengan demikian, sistem mampu memberikan rekomendasi berdasarkan kesamaan pola interaksi antar pengguna dalam dataset.
3. Content-Based Filtering dievaluasi menggunakan metrik Precision, yang mengukur sejauh mana rekomendasi yang dihasilkan sesuai dengan preferensi pengguna. Evaluasi menunjukkan bahwa algoritma ini berhasil memberikan rekomendasi film yang relevan, dengan tingkat precision yang tinggi, khususnya dalam mencocokkan genre antar film. Sementara itu, Collaborative Filtering dievaluasi menggunakan metrik Root Mean Squared Error (RMSE) untuk menilai seberapa dekat hasil prediksi sistem terhadap nilai rating yang sebenarnya. Hasil evaluasi menunjukkan bahwa model collaborative filtering yang dibangun mampu mencapai nilai RMSE yang cukup rendah, menandakan bahwa prediksi yang dihasilkan cukup akurat dan dapat diandalkan. 
   
## Kesimpulan
Berdasarkan hasil implementasi dan evaluasi, sistem rekomendasi film yang dibangun melalui pendekatan content-based filtering dan collaborative filtering menunjukkan performa yang cukup baik dalam memberikan rekomendasi sesuai preferensi pengguna. Pendekatan content-based filtering mampu merekomendasikan film berdasarkan kesamaan genre dengan film yang disukai pengguna, dengan hasil evaluasi precision menunjukkan relevansi yang cukup tinggi. Sementara itu, pendekatan collaborative filtering yang menggunakan model neural network berhasil mempelajari pola perilaku pengguna dan memprediksi rating dengan akurasi yang baik, ditunjukkan melalui nilai RMSE yang rendah. Kedua pendekatan ini saling melengkapi dan membuka peluang untuk pengembangan sistem hybrid di masa depan, guna menghasilkan rekomendasi yang lebih akurat dan personal. Dengan demikian, sistem ini diharapkan dapat membantu pengguna dalam menemukan film yang sesuai dengan minat mereka, serta meningkatkan pengalaman dan keterlibatan pengguna dalam platform.
