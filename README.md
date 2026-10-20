# Sistem Rekomendasi Berbasis Graf Menggunakan Neo4j dan Python
Laporan Ujian Tengah Semester (UTS) Graf Sains Data  
Dataset: RetailRocket E-Commerce  

## Identitas
* Nama: Yanaka Sofia Pardede
* NIM: 24031554065
* Program Studi: S1 Sains Data
* Instansi: Universitas Negeri Surabaya (UNESA)

## Ringkasan Proyek
Proyek ini bertujuan untuk membangun sistem rekomendasi produk item-to-item berbasis graf menggunakan dataset e-commerce RetailRocket. Pemodelan dilakukan dengan menggabungkan pemrosesan data menggunakan Python (Pandas, SciPy, Scikit-learn, NetworkX) dan basis data graf Neo4j. 

Metode utama yang digunakan adalah pembentukan Vector Space Model (VSM) dengan teknik sparse matrix (csr_matrix), perhitungan Cosine Similarity antar produk, write-back relasi SIMILAR_TO ke database Neo4j, serta evaluasi performa rekomendasi menggunakan metrik Hit@5 dan NDCG@5.

## Stack Teknologi dan Library
* Database Graf: Neo4j Database Server
* Bahasa Pemrograman: Python 3.x
* Pengolahan Data: pandas, numpy
* Pemodelan dan VSM: scipy.sparse.csr_matrix, sklearn.metrics.pairwise.cosine_similarity
* Konektor dan Visualisasi: neo4j (Python Driver), networkx, matplotlib

## Dataset
Dikarenakan ukuran file dataset mentah RetailRocket melebihi batas upload langsung di GitHub (100 MB), arsip dataset dapat diunduh melalui tautan eksternal berikut:
* Tautan Unduh Dataset: [RetailRocket Dataset di Google Drive](https://drive.google.com/drive/folders/1u3JMehbQ9wFavSE_O5iXfsKydDRI7EEb?usp=sharing)

## Tahapan Pengerjaan dan Alur Kerja

### 1. Data Wrangling dan Filtering
* Membaca data interaksi mentah (events.csv).
* Mengonversi timestamp dari milidetik epoch ke format datetime.
* Menyaring data berdasarkan rentang tanggal yang ditentukan dan mengekspor hasilnya ke data/events_filtered.csv.

### 2. Analisis Subgraph di Neo4j
* Menghitung komposisi simpul (Visitor dan Item) serta relasi interaksi awal (VIEWED, ADDED_TO_CART, BOUGHT) menggunakan Python Neo4j Driver.

### 3. Vector Space Modeling (VSM)
* Membangun matriks interaksi User-Item menggunakan Sparse Matrix (csr_matrix) untuk efisiensi memori RAM dengan dimensi 1,039,991 baris dan 206,100 kolom.

### 4. Perhitungan Similarity dan Write-Back
* Melakukan transpose matriks menjadi Item-User untuk menghitung Cosine Similarity antar produk dengan ukuran 206,100 x 206,100.
* Melakukan write-back relasi baru SIMILAR_TO beserta properti score ke dalam Neo4j.

### 5. Evaluasi Sistem Rekomendasi
* Memilih user secara acak menggunakan reproducible random seed (random.seed(42)).
* Mengevaluasi Top-5 rekomendasi produk dengan metrik:
  * Hit@5: 1.0 (Sistem berhasil menemukan item relevan dalam Top-5).
  * NDCG@5: 0.5000 (Memperhitungkan posisi atau ranking item relevan dalam daftar).

## Visualisasi Perbandingan Graf
Visualisasi struktur graf menunjukkan perbedaan kondisi sebelum dan sesudah penambahan relasi SIMILAR_TO:

* Kondisi Sebelum: Hanya terdapat interaksi dasar antara Visitor, relasi VIEWED, dan Item.
* Kondisi Sesudah: Terbentuk jaringan relasi kemiripan langsung antar produk melalui relasi SIMILAR_TO.

![Visualisasi Perbandingan Graf](graph_comparison.png)

## Konfigurasi Keamanan dan Kredensial Database
Demi keamanan, kredensial akses basis data Neo4j pada notebook ini disesuaikan menggunakan placeholder. Jika Anda ingin mereplikasi atau menjalankan ulang kode di lingkungan lokal:
1. Pastikan layanan Neo4j lokal Anda sudah aktif.
2. Buka sel koneksi Neo4j di dalam notebook.
3. Ubah variabel `AUTH = ("neo4j", "YOUR_NEO4J_PASSWORD")` dengan mengganti `"YOUR_NEO4J_PASSWORD"` sesuai dengan password instance Neo4j lokal Anda.

## Struktur Direktori
```text
.
├── .gitignore                   # Konfigurasi file yang diabaikan Git
├── graph_comparison.png         # Gambar visualisasi struktur graf
├── 065_Yanaka Sofia Pardede...  # Jupyter Notebook utama proyek
└── README.md                    # Dokumentasi proyek
