# Sistem Rekomendasi Berbasis Graf Menggunakan Neo4j dan Python
Laporan Ujian Tengah Semester (UTS) Graf Sains Data  
Dataset: RetailRocket E-Commerce  

## Identitas
* Nama: Yanaka Sofia Pardede
* NIM: 24031554065[cite: 4]
* Program Studi: S1 Sains Data[cite: 4]
* Instansi: Universitas Negeri Surabaya (UNESA)[cite: 4]

## Ringkasan Proyek
Proyek ini bertujuan untuk membangun sistem rekomendasi produk item-to-item berbasis graf menggunakan dataset e-commerce RetailRocket[cite: 4]. Pemodelan dilakukan dengan menggabungkan pemrosesan data menggunakan Python (Pandas, SciPy, Scikit-learn, NetworkX) dan basis data graf Neo4j[cite: 4, 6]. 

Metode utama yang digunakan adalah pembentukan Vector Space Model (VSM) dengan teknik sparse matrix (csr_matrix), perhitungan Cosine Similarity antar produk, write-back relasi SIMILAR_TO ke database Neo4j, serta evaluasi performa rekomendasi menggunakan metrik Hit@5 dan NDCG@5[cite: 4, 6, 7, 8].

## Stack Teknologi dan Library
* Database Graf: Neo4j Database Server[cite: 4, 6]
* Bahasa Pemrograman: Python 3.x[cite: 4, 6]
* Pengolahan Data: pandas, numpy[cite: 4, 6]
* Pemodelan dan VSM: scipy.sparse.csr_matrix, sklearn.metrics.pairwise.cosine_similarity[cite: 6, 7]
* Konektor dan Visualisasi: neo4j (Python Driver), networkx, matplotlib[cite: 4, 6, 10]

## Tahapan Pengerjaan dan Alur Kerja

### 1. Data Wrangling dan Filtering
* Membaca data interaksi mentah (events.csv)[cite: 4].
* Mengonversi timestamp dari milidetik epoch ke format datetime[cite: 4].
* Menyaring data berdasarkan rentang tanggal yang ditentukan dan mengekspor hasilnya ke data/events_filtered.csv[cite: 4, 5].

### 2. Analisis Subgraph di Neo4j
* Menghitung komposisi simpul (Visitor dan Item) serta relasi interaksi awal (VIEWED, ADDED_TO_CART, BOUGHT) menggunakan Python Neo4j Driver.

### 3. Vector Space Modeling (VSM)
* Membangun matriks interaksi User-Item menggunakan Sparse Matrix (csr_matrix) untuk efisiensi memori RAM dengan dimensi 1,039,991 baris dan 206,100 kolom[cite: 6].

### 4. Perhitungan Similarity dan Write-Back
* Melakukan transpose matriks menjadi Item-User untuk menghitung Cosine Similarity antar produk dengan ukuran 206,100 x 206,100[cite: 7].
* Melakukan write-back relasi baru SIMILAR_TO beserta properti score ke dalam Neo4j[cite: 1, 7].

### 5. Evaluasi Sistem Rekomendasi
* Memilih user secara acak menggunakan reproducible random seed[cite: 3].
* Mengevaluasi Top-5 rekomendasi produk dengan metrik:
  * Hit@5: 1.0 (Sistem berhasil menemukan item relevan dalam Top-5)[cite: 8].
  * NDCG@5: 0.5000 (Memperhitungkan posisi atau ranking item relevan dalam daftar)[cite: 8].

## Visualisasi Perbandingan Graf
Visualisasi struktur graf menunjukkan perbedaan kondisi sebelum dan sesudah penambahan relasi SIMILAR_TO[cite: 3, 10]:

* Kondisi Sebelum: Hanya terdapat interaksi dasar antara Visitor, relasi VIEWED, dan Item[cite: 6, 10].
* Kondisi Sesudah: Terbentuk jaringan relasi kemiripan langsung antar produk melalui relasi SIMILAR_TO[cite: 1, 10].

## Struktur Direktori
```text
.
├── data/
│   ├── events.csv               # Dataset mentah RetailRocket
│   └── events_filtered.csv      # Dataset hasil penyaringan tanggal
├── Yanaka Sofia Pardede_UTS_GrafSainsData.ipynb # Notebook utama
└── README.md                    # Dokumentasi proyek
