# Tutorial Database: Normalisasi dan Relasi

## Daftar Isi

1. [Pengantar Database](#pengantar-database)
2. [Apa itu Normalisasi Database?](#apa-itu-normalisasi-database)
3. [Bentuk-Bentuk Normal (Normal Forms)](#bentuk-bentuk-normal)
4. [Contoh Proses Normalisasi](#contoh-proses-normalisasi)
5. [Relasi Database](#relasi-database)
6. [Contoh Implementasi](#contoh-implementasi)
7. [Kesimpulan](#kesimpulan)

---

## Pengantar Database

Database atau basis data adalah kumpulan data yang terorganisir dan tersimpan secara sistematis dalam komputer. Database relasional menggunakan tabel-tabel yang saling berhubungan untuk menyimpan dan mengelola data.

### Mengapa Normalisasi Penting?

Tanpa normalisasi yang baik, database dapat mengalami masalah seperti:

- **Redundansi data** (data yang berulang)
- **Anomali update** (kesulitan mengubah data)
- **Anomali insert** (kesulitan menambah data)
- **Anomali delete** (kehilangan data yang tidak diinginkan)
- **Ketidakkonsistenan data**

---

## Apa itu Normalisasi Database?

**Normalisasi** adalah proses mengorganisir data dalam database untuk mengurangi redundansi dan meningkatkan integritas data. Normalisasi dilakukan dengan memecah tabel besar menjadi tabel-tabel yang lebih kecil dan mendefinisikan relasi di antara mereka.

### Tujuan Normalisasi:

1. Menghilangkan redundansi data
2. Memastikan dependensi data yang logis
3. Mempermudah pemeliharaan database
4. Mengoptimalkan performa query

---

## Bentuk-Bentuk Normal

### 1. First Normal Form (1NF)

Suatu tabel dikatakan dalam **1NF** jika:

- Setiap kolom berisi nilai atomik (tidak dapat dibagi lagi)
- Setiap baris memiliki data yang unik
- Tidak ada grup berulang dalam satu kolom

**Contoh Pelanggaran 1NF:**

| ID_Siswa | Nama | Hobi                     |
| -------- | ---- | ------------------------ |
| 1        | Budi | Sepak bola, Musik        |
| 2        | Ani  | Membaca, Menulis, Renang |

**Setelah 1NF:**

| ID_Siswa | Nama | Hobi       |
| -------- | ---- | ---------- |
| 1        | Budi | Sepak bola |
| 1        | Budi | Musik      |
| 2        | Ani  | Membaca    |
| 2        | Ani  | Menulis    |
| 2        | Ani  | Renang     |

### 2. Second Normal Form (2NF)

Suatu tabel dikatakan dalam **2NF** jika:

- Sudah memenuhi 1NF
- Semua atribut non-kunci bergantung sepenuhnya pada primary key (tidak ada partial dependency)

**Contoh Pelanggaran 2NF:**

| ID_Siswa | ID_Mata_Kuliah | Nama_Siswa | Nilai | Nama_Mata_Kuliah |
| -------- | -------------- | ---------- | ----- | ---------------- |
| 1        | 101            | Budi       | A     | Database         |
| 1        | 102            | Budi       | B     | Pemrograman Web  |
| 2        | 101            | Ani        | A     | Database         |

_Masalah:_ Nama_Siswa hanya bergantung pada ID_Siswa, dan Nama_Mata_Kuliah hanya bergantung pada ID_Mata_Kuliah.

**Setelah 2NF:**

**Tabel Siswa:**
| ID_Siswa | Nama_Siswa |
|----------|------------|
| 1 | Budi |
| 2 | Ani |

**Tabel Mata_Kuliah:**
| ID_Mata_Kuliah | Nama_Mata_Kuliah |
|----------------|------------------|
| 101 | Database |
| 102 | Pemrograman Web |

**Tabel Nilai:**
| ID_Siswa | ID_Mata_Kuliah | Nilai |
|----------|----------------|-------|
| 1 | 101 | A |
| 1 | 102 | B |
| 2 | 101 | A |

### 3. Third Normal Form (3NF)

Suatu tabel dikatakan dalam **3NF** jika:

- Sudah memenuhi 2NF
- Tidak ada transitive dependency (atribut non-kunci tidak bergantung pada atribut non-kunci lainnya)

**Contoh Pelanggaran 3NF:**

| ID_Siswa | Nama | Kota    | Kode_Pos | Provinsi   |
| -------- | ---- | ------- | -------- | ---------- |
| 1        | Budi | Jakarta | 12345    | DKI        |
| 2        | Ani  | Bandung | 40111    | Jawa Barat |

_Masalah:_ Provinsi bergantung pada Kota, bukan pada ID_Siswa.

**Setelah 3NF:**

**Tabel Siswa:**
| ID_Siswa | Nama | Kode_Pos |
|----------|-----------|----------|
| 1 | Budi | 12345 |
| 2 | Ani | 40111 |

**Tabel Kota:**
| Kode_Pos | Kota | Provinsi |
|----------|-------------|------------|
| 12345 | Jakarta | DKI |
| 40111 | Bandung | Jawa Barat |

### 4. Boyce-Codd Normal Form (BCNF)

Suatu tabel dikatakan dalam **BCNF** jika:

- Sudah memenuhi 3NF
- Untuk setiap functional dependency A → B, A harus super key

BCNF adalah versi yang lebih ketat dari 3NF dan menangani anomali khusus yang jarang terjadi.

---

## Contoh Proses Normalisasi

Mari kita lihat contoh lengkap proses normalisasi dari tabel yang tidak ternormalisasi hingga 3NF.

### Tabel Awal (Tidak Ternormalisasi)

**Tabel Pemesanan:**
| ID_Pesanan | Tanggal | Nama_Customer | Telp_Customer | Alamat_Customer | Produk | Harga_Produk | Jumlah | Total |
|------------|------------|---------------|---------------|-----------------|----------------------|-------------------|-------------|---------|
| 1 | 2026-03-01 | John Doe | 081234567890 | Jl. Merdeka 1 | Mouse, Keyboard | 50000, 150000 | 2, 1 | 250000 |
| 2 | 2026-03-02 | Jane Smith | 081298765432 | Jl. Sudirman 5 | Monitor | 1000000 | 1 | 1000000 |

### Langkah 1: Menerapkan 1NF

Pisahkan nilai yang tidak atomik:

| ID_Pesanan | Tanggal    | Nama_Customer | Telp_Customer | Alamat_Customer | Produk   | Harga_Produk | Jumlah | Subtotal |
| ---------- | ---------- | ------------- | ------------- | --------------- | -------- | ------------ | ------ | -------- |
| 1          | 2026-03-01 | John Doe      | 081234567890  | Jl. Merdeka 1   | Mouse    | 50000        | 2      | 100000   |
| 1          | 2026-03-01 | John Doe      | 081234567890  | Jl. Merdeka 1   | Keyboard | 150000       | 1      | 150000   |
| 2          | 2026-03-02 | Jane Smith    | 081298765432  | Jl. Sudirman 5  | Monitor  | 1000000      | 1      | 1000000  |

### Langkah 2: Menerapkan 2NF

Pisahkan data yang bergantung partial:

**Tabel Pesanan:**
| ID_Pesanan | Tanggal | ID_Customer |
|------------|------------|-------------|
| 1 | 2026-03-01 | 1 |
| 2 | 2026-03-02 | 2 |

**Tabel Customer:**
| ID_Customer | Nama_Customer | Telp_Customer | Alamat_Customer |
|-------------|---------------|---------------|-----------------|
| 1 | John Doe | 081234567890 | Jl. Merdeka 1 |
| 2 | Jane Smith | 081298765432 | Jl. Sudirman 5 |

**Tabel Produk:**
| ID_Produk | Nama_Produk | Harga |
|-----------|-------------|-------------|
| 1 | Mouse | 50000 |
| 2 | Keyboard | 150000 |
| 3 | Monitor | 1000000 |

**Tabel Detail_Pesanan:**
| ID_Pesanan | ID_Produk | Jumlah | Subtotal |
|------------|-----------|--------|----------|
| 1 | 1 | 2 | 100000 |
| 1 | 2 | 1 | 150000 |
| 2 | 3 | 1 | 1000000 |

### Langkah 3: Menerapkan 3NF

Dalam kasus ini, tabel sudah memenuhi 3NF karena tidak ada transitive dependency. Subtotal sebenarnya dapat dihitung (Jumlah × Harga), jadi bisa dihapus untuk menghindari redundansi:

**Tabel Detail_Pesanan (Final):**
| ID_Pesanan | ID_Produk | Jumlah |
|------------|-----------|--------|
| 1 | 1 | 2 |
| 1 | 2 | 1 |
| 2 | 3 | 1 |

---

## Relasi Database

Relasi adalah hubungan antara tabel dalam database. Ada tiga jenis relasi utama:

### 1. One-to-One (1:1)

Satu record dalam tabel A berhubungan dengan tepat satu record dalam tabel B.

**Contoh:** Satu siswa memiliki satu KTP.

**Tabel Siswa:**
| ID_Siswa | Nama | Email |
|----------|-----------|-------------------|
| 1 | Budi | budi@email.com |
| 2 | Ani | ani@email.com |

**Tabel KTP:**
| ID_KTP | ID_Siswa | Nomor_KTP | Alamat_KTP |
|--------|----------|----------------|---------------|
| 1 | 1 | 3201010101010001| Jl. Merdeka 1|
| 2 | 2 | 3202020202020002| Jl. Sudirman 5|

**Diagram:**

```
Siswa (1) ←→ (1) KTP
```

### 2. One-to-Many (1:N)

Satu record dalam tabel A dapat berhubungan dengan banyak record dalam tabel B, tetapi satu record dalam tabel B hanya berhubungan dengan satu record dalam tabel A.

**Contoh:** Satu dosen dapat mengajar banyak mata kuliah, tetapi satu mata kuliah hanya diajar oleh satu dosen.

**Tabel Dosen:**
| ID_Dosen | Nama_Dosen | Email |
|----------|----------------|--------------------|
| 1 | Dr. Ahmad | ahmad@univ.ac.id |
| 2 | Dr. Siti | siti@univ.ac.id |

**Tabel Mata_Kuliah:**
| ID_Mata_Kuliah | Nama_Mata_Kuliah | ID_Dosen | SKS |
|----------------|------------------|----------|-----|
| 101 | Database | 1 | 3 |
| 102 | Pemrograman Web | 1 | 3 |
| 103 | Jaringan | 2 | 3 |

**Diagram:**

```
Dosen (1) ←→ (N) Mata_Kuliah
```

### 3. Many-to-Many (M:N)

Banyak record dalam tabel A dapat berhubungan dengan banyak record dalam tabel B, dan sebaliknya.

**Contoh:** Banyak siswa dapat mengambil banyak mata kuliah, dan satu mata kuliah dapat diambil oleh banyak siswa.

**Implementasi:** Relasi M:N membutuhkan **junction table** (tabel penghubung).

**Tabel Siswa:**
| ID_Siswa | Nama | NIM |
|----------|-----------|-----------|
| 1 | Budi | 20230001 |
| 2 | Ani | 20230002 |
| 3 | Dedi | 20230003 |

**Tabel Mata_Kuliah:**
| ID_Mata_Kuliah | Nama_Mata_Kuliah | SKS |
|----------------|------------------|-----|
| 101 | Database | 3 |
| 102 | Pemrograman Web | 3 |
| 103 | Jaringan | 3 |

**Tabel Siswa_Mata_Kuliah (Junction Table):**
| ID_Siswa | ID_Mata_Kuliah | Nilai | Semester |
|----------|----------------|-------|----------|
| 1 | 101 | A | 3 |
| 1 | 102 | B | 3 |
| 2 | 101 | A | 3 |
| 2 | 103 | B | 3 |
| 3 | 102 | A | 3 |

**Diagram:**

```
Siswa (M) ←→ Siswa_Mata_Kuliah ←→ (N) Mata_Kuliah
```

---

## Contoh Implementasi

### Membuat Database dengan SQL

Berikut contoh implementasi schema database untuk sistem akademik:

```sql
-- Tabel Mahasiswa
CREATE TABLE Mahasiswa (
    id_mahasiswa INT PRIMARY KEY AUTO_INCREMENT,
    nim VARCHAR(10) UNIQUE NOT NULL,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    tanggal_lahir DATE,
    alamat TEXT,
    id_jurusan INT,
    FOREIGN KEY (id_jurusan) REFERENCES Jurusan(id_jurusan)
);

-- Tabel Jurusan (One-to-Many dengan Mahasiswa)
CREATE TABLE Jurusan (
    id_jurusan INT PRIMARY KEY AUTO_INCREMENT,
    kode_jurusan VARCHAR(10) UNIQUE NOT NULL,
    nama_jurusan VARCHAR(100) NOT NULL,
    fakultas VARCHAR(100)
);

-- Tabel Dosen
CREATE TABLE Dosen (
    id_dosen INT PRIMARY KEY AUTO_INCREMENT,
    nip VARCHAR(20) UNIQUE NOT NULL,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    spesialisasi VARCHAR(100)
);

-- Tabel Mata_Kuliah
CREATE TABLE Mata_Kuliah (
    id_mata_kuliah INT PRIMARY KEY AUTO_INCREMENT,
    kode_mk VARCHAR(10) UNIQUE NOT NULL,
    nama_mk VARCHAR(100) NOT NULL,
    sks INT NOT NULL,
    id_dosen INT,
    FOREIGN KEY (id_dosen) REFERENCES Dosen(id_dosen)
);

-- Tabel Pengambilan_MK (Junction Table untuk Many-to-Many)
CREATE TABLE Pengambilan_MK (
    id_pengambilan INT PRIMARY KEY AUTO_INCREMENT,
    id_mahasiswa INT,
    id_mata_kuliah INT,
    semester INT,
    tahun_ajaran VARCHAR(10),
    nilai_angka DECIMAL(5,2),
    nilai_huruf CHAR(2),
    FOREIGN KEY (id_mahasiswa) REFERENCES Mahasiswa(id_mahasiswa),
    FOREIGN KEY (id_mata_kuliah) REFERENCES Mata_Kuliah(id_mata_kuliah),
    UNIQUE KEY unique_pengambilan (id_mahasiswa, id_mata_kuliah, semester, tahun_ajaran)
);
```

### Contoh Query

**1. Menampilkan semua mahasiswa dengan jurusannya:**

```sql
SELECT
    m.nim,
    m.nama,
    m.email,
    j.nama_jurusan,
    j.fakultas
FROM Mahasiswa m
JOIN Jurusan j ON m.id_jurusan = j.id_jurusan
ORDER BY m.nama;
```

**2. Menampilkan mata kuliah yang diambil oleh seorang mahasiswa:**

```sql
SELECT
    m.nama AS nama_mahasiswa,
    mk.kode_mk,
    mk.nama_mk,
    mk.sks,
    p.nilai_huruf,
    d.nama AS nama_dosen
FROM Pengambilan_MK p
JOIN Mahasiswa m ON p.id_mahasiswa = m.id_mahasiswa
JOIN Mata_Kuliah mk ON p.id_mata_kuliah = mk.id_mata_kuliah
JOIN Dosen d ON mk.id_dosen = d.id_dosen
WHERE m.nim = '20230001'
ORDER BY mk.nama_mk;
```

**3. Menghitung IPK mahasiswa:**

```sql
SELECT
    m.nim,
    m.nama,
    AVG(
        CASE p.nilai_huruf
            WHEN 'A' THEN 4.0
            WHEN 'AB' THEN 3.5
            WHEN 'B' THEN 3.0
            WHEN 'BC' THEN 2.5
            WHEN 'C' THEN 2.0
            WHEN 'D' THEN 1.0
            WHEN 'E' THEN 0.0
            ELSE 0
        END
    ) AS IPK
FROM Mahasiswa m
JOIN Pengambilan_MK p ON m.id_mahasiswa = p.id_mahasiswa
GROUP BY m.id_mahasiswa, m.nim, m.nama
ORDER BY IPK DESC;
```

**4. Menampilkan mata kuliah dengan jumlah mahasiswanya:**

```sql
SELECT
    mk.kode_mk,
    mk.nama_mk,
    d.nama AS dosen_pengampu,
    COUNT(p.id_mahasiswa) AS jumlah_mahasiswa
FROM Mata_Kuliah mk
LEFT JOIN Pengambilan_MK p ON mk.id_mata_kuliah = p.id_mata_kuliah
JOIN Dosen d ON mk.id_dosen = d.id_dosen
GROUP BY mk.id_mata_kuliah, mk.kode_mk, mk.nama_mk, d.nama
ORDER BY jumlah_mahasiswa DESC;
```

---

## Entity Relationship Diagram (ERD)

Berikut adalah representasi visual dari relasi antar tabel:

```
┌─────────────┐         ┌──────────────┐
│  Jurusan    │         │    Dosen     │
├─────────────┤         ├──────────────┤
│ id_jurusan  │         │ id_dosen     │
│ kode_jurusan│         │ nip          │
│ nama_jurusan│         │ nama         │
│ fakultas    │         │ email        │
└──────┬──────┘         │ spesialisasi │
       │                └──────┬───────┘
       │ 1:N                   │ 1:N
       │                       │
┌──────┴──────┐         ┌──────┴──────────┐
│  Mahasiswa  │         │  Mata_Kuliah    │
├─────────────┤         ├─────────────────┤
│id_mahasiswa │         │ id_mata_kuliah  │
│ nim         │         │ kode_mk         │
│ nama        │         │ nama_mk         │
│ email       │         │ sks             │
│tanggal_lahir│         │ id_dosen (FK)   │
│ alamat      │         └────────┬────────┘
│id_jurusan(FK)│                │
└──────┬──────┘                 │
       │                        │
       │ M:N                    │
       │                        │
       └────────┬───────────────┘
                │
        ┌───────┴──────────┐
        │ Pengambilan_MK   │
        ├──────────────────┤
        │ id_pengambilan   │
        │ id_mahasiswa (FK)│
        │id_mata_kuliah(FK)│
        │ semester         │
        │ tahun_ajaran     │
        │ nilai_angka      │
        │ nilai_huruf      │
        └──────────────────┘
```

---

## Best Practices

### 1. Normalisasi yang Seimbang

Meskipun normalisasi penting, terkadang **denormalisasi** diperlukan untuk performa. Pertimbangkan:

- **Query yang sering dilakukan:** Jika join terlalu banyak memperlambat query, pertimbangkan menyimpan beberapa data redundan
- **Read vs Write:** Database dengan banyak read benefit dari denormalisasi, sedangkan database dengan banyak write benefit dari normalisasi

### 2. Penggunaan Foreign Key

Selalu gunakan foreign key constraint untuk memastikan referential integrity:

```sql
FOREIGN KEY (id_column) REFERENCES parent_table(id_column)
    ON DELETE CASCADE
    ON UPDATE CASCADE
```

### 3. Indexing

Buat index pada kolom yang sering digunakan untuk:

- Primary key (otomatis)
- Foreign key
- Kolom yang sering di-query (WHERE, JOIN)

```sql
CREATE INDEX idx_mahasiswa_nim ON Mahasiswa(nim);
CREATE INDEX idx_mata_kuliah_kode ON Mata_Kuliah(kode_mk);
```

### 4. Naming Convention

Gunakan naming convention yang konsisten:

- **Tabel:** Gunakan singular atau plural, tetapi konsisten
- **Primary Key:** `id_nama_tabel` atau hanya `id`
- **Foreign Key:** `id_nama_tabel_referensi`
- **Junction Table:** `tabel1_tabel2` atau nama yang deskriptif

---

## Latihan

### Soal 1: Identifikasi Pelanggaran Normalisasi

Perhatikan tabel berikut:

| ID_Pegawai | Nama  | Departemen | Lokasi_Dept | Gaji    | Proyek              |
| ---------- | ----- | ---------- | ----------- | ------- | ------------------- |
| 1          | Ahmad | IT         | Gedung A    | 8000000 | Website, Mobile App |
| 2          | Budi  | HR         | Gedung B    | 6000000 | Recruitment         |
| 3          | Citra | IT         | Gedung A    | 8500000 | Mobile App          |

**Pertanyaan:**

1. Apakah tabel ini memenuhi 1NF? Mengapa?
2. Apa masalah jika lokasi departemen IT berubah?
3. Bagaimana Anda menormalisasi tabel ini hingga 3NF?

### Soal 2: Desain Schema

Buatlah schema database untuk sistem perpustakaan dengan requirement:

- Perpustakaan memiliki banyak buku
- Buku dapat memiliki banyak penulis
- Anggota dapat meminjam banyak buku
- Satu peminjaman dapat mencakup banyak buku
- Setiap peminjaman memiliki tanggal pinjam dan tanggal kembali

Tentukan:

1. Tabel-tabel yang diperlukan
2. Primary key dan foreign key
3. Jenis relasi antar tabel
4. ERD

---

## Kesimpulan

Normalisasi database adalah teknik fundamental dalam desain database yang bertujuan untuk:

1. **Mengurangi redundansi** dan menghemat storage
2. **Meningkatkan integritas data** dengan menghindari anomali
3. **Mempermudah maintenance** database
4. **Mengoptimalkan performa** query

**Poin-poin Penting:**

- **1NF:** Nilai atomik, tidak ada grup berulang
- **2NF:** 1NF + tidak ada partial dependency
- **3NF:** 2NF + tidak ada transitive dependency
- **Relasi:** One-to-One, One-to-Many, Many-to-Many
- **Junction Table:** Diperlukan untuk relasi Many-to-Many

Dengan memahami normalisasi dan relasi, Anda dapat mendesain database yang efisien, scalable, dan mudah dimaintain.

---

## Referensi

- Codd, E.F. (1970). "A Relational Model of Data for Large Shared Data Banks"
- Date, C.J. "An Introduction to Database Systems"
- Elmasri & Navathe. "Fundamentals of Database Systems"
- Database Design Documentation: https://dev.mysql.com/doc/
- SQL Standards: ISO/IEC 9075

---

**Dibuat pada:** 31 Maret 2026

**Tags:** #database #normalisasi #relasi #SQL #tutorial #basis-data
