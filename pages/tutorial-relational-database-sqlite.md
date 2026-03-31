# Tutorial Lengkap Relational Database dengan SQLite: Dari Pemula hingga Mahir

![Relational Database Banner](../images/database-hero.svg)

> **Panduan Komprehensif tentang Relational Database** - Dari konsep dasar hingga implementasi project perpustakaan dengan SQLite

---

## 📋 Daftar Isi

1. [Pendahuluan](#1-pendahuluan)
2. [Sejarah Database dan Model Relasional](#2-sejarah-database-dan-model-relasional)
3. [Mengapa Harus Belajar Relational Database?](#3-mengapa-harus-belajar-relational-database)
4. [Pengenalan SQLite](#4-pengenalan-sqlite)
5. [Instalasi dan Setup SQLite](#5-instalasi-dan-setup-sqlite)
6. [Konsep Dasar Database](#6-konsep-dasar-database)
7. [Tipe Data di SQLite](#7-tipe-data-di-sqlite)
8. [Membuat Database dan Tabel](#8-membuat-database-dan-tabel)
9. [Operasi CRUD Dasar](#9-operasi-crud-dasar)
10. [Filtering dan Kondisi WHERE](#10-filtering-dan-kondisi-where)
11. [Sorting dan Limiting Data](#11-sorting-dan-limiting-data)
12. [Fungsi Agregasi](#12-fungsi-agregasi)
13. [JOIN Operations](#13-join-operations)
14. [Relationships dalam Database](#14-relationships-dalam-database)
15. [Normalisasi Database](#15-normalisasi-database)
16. [Constraints dan Validasi Data](#16-constraints-dan-validasi-data)
17. [Indexes untuk Optimasi](#17-indexes-untuk-optimasi)
18. [Transactions dan ACID](#18-transactions-dan-acid)
19. [Project: Sistem Manajemen Perpustakaan](#19-project-sistem-manajemen-perpustakaan)
20. [Tools dan GUI untuk SQLite](#20-tools-dan-gui-untuk-sqlite)
21. [Best Practices](#21-best-practices)
22. [Kesimpulan dan Next Steps](#22-kesimpulan-dan-next-steps)

---

## 1. Pendahuluan

### 1.1 Apa itu Database?

**Database** adalah kumpulan data yang terorganisir dan terstruktur, yang disimpan secara elektronik dalam sistem komputer. Database memungkinkan kita untuk menyimpan, mengelola, dan mengambil data dengan cara yang efisien.

**Analogi sederhana**: Bayangkan perpustakaan. Perpustakaan memiliki sistem katalog yang terorganisir untuk menyimpan dan menemukan buku. Database bekerja dengan cara yang sama untuk data digital.

**Mengapa perlu Database?**

Tanpa database, kita harus menyimpan data dalam file-file terpisah (Excel, CSV, Text files), yang memiliki beberapa masalah:

- **Redundansi data** - Data yang sama disimpan berulang kali
- **Inkonsistensi** - Sulit memastikan data selalu sinkron di berbagai file
- **Kesulitan akses** - Sulit mencari data spesifik dari file besar
- **Konkurensi** - Masalah saat banyak user mengakses file yang sama
- **Keamanan** - Sulit mengatur siapa boleh akses data apa
- **Integritas** - Tidak ada jaminan data valid

### 1.2 Apa itu Relational Database?

**Relational Database** adalah jenis database yang menyimpan data dalam bentuk **tabel** (table) dengan **baris** (rows) dan **kolom** (columns), di mana tabel-tabel tersebut memiliki **relasi** (hubungan) satu sama lain.

**Karakteristik Relational Database:**

- Data disimpan dalam **tabel** (tables) yang memiliki struktur tetap
- Setiap tabel memiliki **kolom** (columns) yang mendefinisikan jenis data
- Setiap **baris** (row) dalam tabel adalah satu record/data
- Tabel-tabel dapat saling **berelasi** melalui **keys**
- Menggunakan **SQL** (Structured Query Language) untuk manipulasi data

**Contoh visual struktur tabel:**

```
Tabel: mahasiswa
┌────────────┬──────────────────┬──────────┬─────────────┐
│ id         │ nama             │ nim      │ jurusan     │
├────────────┼──────────────────┼──────────┼─────────────┤
│ 1          │ Budi Santoso     │ 2101001  │ Informatika │
│ 2          │ Siti Nurhaliza   │ 2101002  │ Sistem Info │
│ 3          │ Ahmad Dahlan     │ 2101003  │ Informatika │
└────────────┴──────────────────┴──────────┴─────────────┘
```

### 1.3 Apa itu RDBMS?

**RDBMS (Relational Database Management System)** adalah software yang digunakan untuk mengelola relational database. RDBMS menyediakan interface dan tools untuk membuat, membaca, mengupdate, dan menghapus data.

**Contoh RDBMS populer:**

| RDBMS      | Deskripsi                                         | Use Case                           |
| ---------- | ------------------------------------------------- | ---------------------------------- |
| MySQL      | Open-source, paling populer untuk web apps        | Website, aplikasi web, WordPress   |
| PostgreSQL | Open-source, powerful, supports advanced features | Enterprise apps, data analytics    |
| SQLite     | Lightweight, embedded, tanpa server               | Mobile apps, desktop apps, testing |
| Oracle     | Enterprise-grade, powerful, mahal                 | Bank, perusahaan besar             |
| SQL Server | Microsoft, terintegrasi dengan .NET               | Windows apps, enterprise           |

### 1.4 SQL: Bahasa untuk Database

**SQL (Structured Query Language)** adalah bahasa standar yang digunakan untuk berkomunikasi dengan database. SQL memungkinkan kita untuk:

- **Membuat** struktur database dan tabel
- **Menambah** data baru
- **Mengambil** data dengan filter tertentu
- **Mengupdate** data yang sudah ada
- **Menghapus** data

**Contoh SQL sederhana:**

```sql
-- Mengambil semua data mahasiswa
SELECT * FROM mahasiswa;

-- Menambah mahasiswa baru
INSERT INTO mahasiswa (nama, nim, jurusan)
VALUES ('Rina Wijaya', '2101004', 'Informatika');

-- Update jurusan mahasiswa
UPDATE mahasiswa
SET jurusan = 'Teknik Komputer'
WHERE id = 2;

-- Hapus data mahasiswa
DELETE FROM mahasiswa WHERE id = 3;
```

### 1.5 Kapan Menggunakan Relational Database?

**Gunakan Relational Database ketika:**

✅ Data memiliki **struktur yang jelas** dan konsisten
✅ Perlu **relationships** antara berbagai entitas
✅ Memerlukan **ACID compliance** (consistency, reliability)
✅ Query kompleks dengan **JOIN** antar tabel
✅ Data **transactional** (banking, e-commerce)

**Contoh use case:**

- 🏪 **E-commerce** - customers, products, orders, payments
- 🏥 **Hospital** - patients, doctors, appointments, medical records
- 🏫 **School** - students, teachers, classes, grades
- 💰 **Banking** - accounts, transactions, customers
- 📚 **Library** - books, members, loans, authors

**Jangan gunakan Relational Database ketika:**

❌ Data tidak memiliki struktur tetap (unstructured)
❌ Perlu **horizontal scaling** yang ekstrem
❌ Data dalam format **dokumen** atau **graph**
❌ Perlu **real-time streaming** dengan latency sangat rendah

**Alternative untuk kasus tersebut:**

- **NoSQL databases** - MongoDB, Cassandra, Redis
- **Document stores** - untuk data JSON/XML
- **Graph databases** - Neo4j untuk relasi kompleks
- **Time-series databases** - InfluxDB untuk IoT data

---

## 2. Sejarah Database dan Model Relasional

### 2.1 Era Pre-Database (1950-1960an)

Sebelum database modern, data disimpan dalam:

- **Flat files** - File text sederhana
- **Punch cards** - Kartu berlubang untuk input data
- **Magnetic tapes** - Pita magnetik

**Masalah:**

- Data redundancy tinggi
- Sulit maintain consistency
- Akses data lambat (sequential)
- Tidak ada standar

### 2.2 Hierarchical dan Network Database (1960-1970an)

**Hierarchical Database** (IBM IMS - 1966):

- Data dalam struktur tree/pohon
- Parent-child relationship
- Sulit represent many-to-many relationships

**Network Database** (CODASYL - 1969):

- Menggunakan pointer untuk link records
- Complex navigation
- Sulit dipelajari dan maintain

### 2.3 Lahirnya Model Relasional (1970)

**Dr. Edgar F. Codd**, ilmuwan komputer di IBM, menerbitkan paper revolusioner:
**"A Relational Model of Data for Large Shared Data Banks"** (1970)

**Konsep revolutionary:**

1. **Data dalam bentuk tabel** (relations) dengan rows dan columns
2. **Set theory dan predicate logic** sebagai fondasi matematika
3. **Data independence** - pemisahan logical dan physical data
4. **Declarative queries** - describe WHAT, bukan HOW
5. **Normalization** - menghilangkan redundansi

**Mengapa revolutionary?**

Sebelum Codd, programmer harus tahu **bagaimana** data disimpan (physical storage). Model relasional membebaskan programmer untuk fokus pada **apa** yang mereka butuhkan.

### 2.4 SQL: Bahasa Standar (1974-1986)

**SQL (Structured Query Language)** dikembangkan di IBM:

- **1974** - IBM mulai develop SEQUEL (Structured English Query Language)
- **1977** - Rename ke SQL karena trademark issues
- **1979** - Relational Software Inc. (sekarang Oracle) rilis produk komersial pertama
- **1986** - ANSI mengadopsi SQL sebagai standar
- **1987** - ISO mengadopsi SQL sebagai standar internasional

**Timeline RDBMS populer:**

```
1970 - Model Relasional (Codd)
1979 - Oracle Database
1983 - IBM DB2
1989 - Oracle 6 (first row-level locking)
1995 - MySQL
1996 - PostgreSQL
2000 - SQLite
2008 - PostgreSQL 8.3 (HOT updates)
2010 - MySQL 5.5 (InnoDB default)
```

### 2.5 Era Modern (2000-sekarang)

**Tren modern:**

- **Cloud databases** - AWS RDS, Google Cloud SQL, Azure SQL
- **NewSQL** - modern RDBMS dengan scalability (CockroachDB, TiDB)
- **Distributed SQL** - horizontal scaling dengan SQL semantics
- **Serverless databases** - pay-per-use, auto-scaling
- **Multi-model databases** - support SQL + NoSQL dalam satu system

**Fun facts:**

🏆 **Dr. Edgar F. Codd** menerima Turing Award (1981) - Nobel-nya Computer Science
📈 **SQL** masih menjadi bahasa database #1 setelah 50+ tahun
💼 **Database Administrator** adalah salah satu profesi IT dengan gaji tertinggi
📊 **70-80% aplikasi enterprise** masih menggunakan relational database

---

## 3. Mengapa Harus Belajar Relational Database?

### 3.1 Skill Fundamental untuk Programmer

Relational database adalah **core skill** yang dibutuhkan untuk hampir semua posisi programming:

#### 💼 Job Requirements

Berdasarkan analisis job postings (Stack Overflow, LinkedIn):

| Posisi                | % Requires SQL/Database |
| --------------------- | ----------------------- |
| Backend Developer     | 95%                     |
| Full-Stack Developer  | 90%                     |
| Data Analyst          | 100%                    |
| Data Engineer         | 100%                    |
| DevOps Engineer       | 70%                     |
| Mobile Developer      | 60%                     |
| QA/Test Engineer      | 65%                     |
| Business Intelligence | 100%                    |

**Kesimpulan:** Hampir mustahil menjadi programmer profesional tanpa menguasai database.

### 3.2 SQL: Bahasa Paling Stabil dalam Programming

**Perbandingan dengan bahasa lain:**

```
JavaScript frameworks:
2010 → jQuery
2013 → AngularJS
2015 → React
2016 → Vue
2020 → Svelte
... terus berganti!

SQL:
1986 → SQL-86 standard
2026 → Masih menggunakan sintaks yang sama! ✅
```

**Keuntungan:**

- ✅ **Invest once, use forever** - SQL yang Anda pelajari hari ini masih relevant 10 tahun lagi
- ✅ **Transferable skill** - SQL syntax hampir sama di MySQL, PostgreSQL, SQL Server
- ✅ **High ROI** - Return on Investment tinggi untuk waktu belajar

### 3.3 Use Cases di Dunia Nyata

#### 🌐 Web Development

**Setiap website modern butuh database:**

```
User Registration → INSERT INTO users
Login → SELECT * FROM users WHERE email = ?
Profile Update → UPDATE users SET ... WHERE id = ?
Delete Account → DELETE FROM users WHERE id = ?
```

**Contoh konkret:**

- **Instagram** - MySQL untuk user data, posts, likes
- **Twitter** - MySQL + Manhattan (distributed database)
- **Facebook** - MySQL dengan custom modifications (MyRocks)

#### 📱 Mobile Apps

**Mobile apps menggunakan SQLite:**

- **WhatsApp** - SQLite untuk menyimpan chat history
- **Evernote** - SQLite untuk notes dan attachments
- **Spotify** - SQLite untuk offline cache
- **Games** - SQLite untuk save game data, settings

#### 📊 Data Analysis & Business Intelligence

**Semua data analyst HARUS menguasai SQL:**

```sql
-- Analisis penjualan per kategori
SELECT
    category,
    COUNT(*) as total_transactions,
    SUM(amount) as revenue,
    AVG(amount) as avg_transaction
FROM sales
WHERE sale_date >= '2026-01-01'
GROUP BY category
ORDER BY revenue DESC;
```

**Tools yang menggunakan SQL:**

- **Tableau** - visualization dengan SQL queries
- **Power BI** - Microsoft BI tool
- **Google Data Studio** - free analytics tool
- **Metabase** - open-source BI

#### 🤖 Machine Learning & AI

**ML engineers butuh SQL untuk:**

- **Data extraction** - ambil data training dari database
- **Feature engineering** - transform data dengan SQL
- **Data cleaning** - filter missing values, outliers
- **Model deployment** - query untuk real-time predictions

**Example workflow:**

```sql
-- Extract features untuk training model
SELECT
    customer_id,
    COUNT(DISTINCT order_id) as total_orders,
    SUM(amount) as lifetime_value,
    DATEDIFF(CURRENT_DATE, first_order_date) as customer_age_days,
    CASE WHEN last_order_date > DATE_SUB(CURRENT_DATE, INTERVAL 90 DAY)
         THEN 1 ELSE 0 END as is_active
FROM orders
GROUP BY customer_id;
```

### 3.4 Career Benefits

#### 💰 Salary Impact

**Average salary increase dengan skill SQL** (data dari PayScale, 2025):

| Posisi               | Without SQL | With SQL | Increase |
| -------------------- | ----------- | -------- | -------- |
| Junior Developer     | $55,000     | $65,000  | +18%     |
| Backend Developer    | $75,000     | $90,000  | +20%     |
| Data Analyst         | N/A         | $80,000  | -        |
| Full-Stack Developer | $80,000     | $95,000  | +19%     |

#### 📈 Job Opportunities

**Jumlah job postings dengan keyword "SQL"** (Indeed, 2026):

- United States: 150,000+ jobs
- Europe: 80,000+ jobs
- Asia: 60,000+ jobs
- Remote: 30,000+ jobs

### 3.5 Problem-Solving Skills

Belajar SQL melatih **logical thinking:**

- **Declarative programming** - fokus pada WHAT, bukan HOW
- **Set theory** - berpikir dalam terms of sets/collections
- **Data modeling** - abstract real-world entities
- **Query optimization** - understand tradeoffs

**Skill transferable ke:**

- Algorithm design
- System architecture
- Problem decomposition
- Performance optimization

### 3.6 Personal Productivity

SQL tidak hanya untuk work projects! Gunakan untuk:

#### 📊 Personal Data Analysis

```sql
-- Analisis pengeluaran pribadi
SELECT
    category,
    SUM(amount) as total,
    COUNT(*) as transactions
FROM expenses
WHERE month = 3 AND year = 2026
GROUP BY category
ORDER BY total DESC;
```

#### 📚 Knowledge Management

- Organize notes dalam database (Obsidian, Notion menggunakan SQLite)
- Track reading list, books
- Personal CRM - track contacts, meetings

#### 🎯 Side Projects & Startups

- Build MVP dengan database proper sejak awal
- Tidak perlu refactor ketika scale
- Investor appreciate solid technical foundation

---

## 4. Pengenalan SQLite

### 4.1 Apa itu SQLite?

**SQLite** adalah **embedded relational database** yang paling banyak digunakan di dunia. SQLite adalah library yang menyediakan database engine dalam bentuk file tunggal.

**Tagline:** "Small. Fast. Reliable. Choose any three." 😊

### 4.2 Karakteristik SQLite

#### 🌟 Unique Features

**1. Serverless**

```
Traditional Database:        SQLite:
┌─────────┐                 ┌─────────┐
│  App    │←──Network──→    │  App    │
└─────────┘                 │ +SQLite │
     ↑                      └─────────┘
┌─────────┐                       ↓
│ DB      │                  database.db
│ Server  │                    (file)
└─────────┘
```

Tidak ada proses server terpisah. SQLite adalah library yang di-embed langsung ke aplikasi.

**2. Zero Configuration**

- Tidak perlu instalasi server
- Tidak perlu konfigurasi users/passwords
- Tidak perlu setup networking
- Langsung pakai!

**3. Single File Database**

- Seluruh database dalam 1 file
- Mudah di-copy, backup, share
- Portable antar platform

**4. Cross-Platform**

- Windows, Mac, Linux, iOS, Android
- File format sama di semua platform
- Tulis di Windows, baca di Linux tanpa konversi

**5. Self-Contained**

- Minimal dependencies
- Ukuran library kecil (~600 KB)
- Perfect untuk embedded systems

### 4.3 SQLite vs Database Lain

**Perbandingan:**

| Feature         | SQLite                     | MySQL                    | PostgreSQL                  |
| --------------- | -------------------------- | ------------------------ | --------------------------- |
| **Type**        | Embedded                   | Client-Server            | Client-Server               |
| **Setup**       | Zero config                | Server install           | Server install              |
| **File**        | Single .db file            | Multiple files           | Multiple files              |
| **Size**        | ~600 KB library            | ~200 MB install          | ~300 MB install             |
| **Concurrency** | Multiple readers, 1 writer | Many concurrent users    | Many concurrent users       |
| **Users**       | File permissions           | User management          | User management             |
| **Networking**  | No                         | TCP/IP                   | TCP/IP                      |
| **Max DB Size** | 281 TB                     | Unlimited\*              | Unlimited\*                 |
| **Use Case**    | Mobile, desktop, embedded  | Web apps, shared hosting | Enterprise, complex queries |

### 4.4 Kapan Menggunakan SQLite?

#### ✅ Perfect Use Cases

**1. Mobile Applications**

- iOS (Core Data uses SQLite)
- Android (SQLite built-in)
- React Native, Flutter

**2. Desktop Applications**

- Electron apps
- Native desktop apps
- Games (save data)

**3. Embedded Systems**

- IoT devices
- Set-top boxes
- Automotive systems

**4. Development & Testing**

- Prototype/MVP
- Unit tests
- Local development

**5. Small to Medium Web Apps**

- Personal blogs
- Internal tools
- Landing pages
- <= 100,000 hits/day

**6. Data Analysis**

- Local data analysis
- CSV to database conversion
- Data pipeline intermediate storage

**7. Application File Format**

- Alternative to XML/JSON files
- Structured data storage
- Query-able files

#### ❌ Ketika TIDAK Menggunakan SQLite

**1. High Write Concurrency**

```
❌ Many users writing simultaneously
   (SQLite has 1 writer limit)

✅ Use: MySQL, PostgreSQL
```

**2. Client-Server Applications**

```
❌ Multiple machines accessing same database
   (SQLite is local file)

✅ Use: MySQL, PostgreSQL, SQL Server
```

**3. Very Large Datasets**

```
❌ Terabytes of data with complex queries
   (SQLite can handle it, but other RDBMS better optimized)

✅ Use: PostgreSQL, Oracle
```

**4. Strong Access Control**

```
❌ Row-level permissions, complex user roles
   (SQLite uses file system permissions)

✅ Use: PostgreSQL, SQL Server
```

### 4.5 Who Uses SQLite?

**SQLite adalah database yang paling banyak di-deploy di dunia!**

**Estimated installations:** 1 trillion+ (yes, trillion!)

**Companies dan products:**

- 📱 **Apple** - iOS, macOS (Core Data, Safari)
- 📱 **Google** - Android OS, Chrome browser
- 💬 **WhatsApp** - Message storage
- ✈️ **Airbus** - Flight software (A350)
- 🚗 **Automotive** - Maybach, Bentley (info systems)
- 🎮 **Games** - Grand Theft Auto, SimCity
- 📺 **Smart TVs** - Many brands
- 🌐 **Browsers** - Firefox, Chrome (cookies, history)
- 📝 **Microsoft** - Windows 10 (various components)

**Every smartphone has SQLite:**

- iPhone = 100+ SQLite databases per device
- Android = multiple SQLite databases per app

**Quote dari D. Richard Hipp (creator SQLite):**

> "SQLite is likely used more than all other database engines combined."

### 4.6 Kelebihan dan Kekurangan SQLite

#### ✅ Kelebihan

1. **Simplicity**
   - Easy to learn, easy to use
   - No server management
   - No configuration

2. **Portability**
   - Single file database
   - Cross-platform compatibility
   - Easy backup (copy file)

3. **Reliability**
   - Battle-tested (20+ years)
   - Extensive testing (100% branch coverage)
   - ACID-compliant

4. **Performance**
   - Fast for read operations
   - Low memory footprint
   - No network latency

5. **Zero Cost**
   - Public domain (not even open source - more permissive!)
   - No licensing fees
   - Free commercial use

6. **Perfect for Learning**
   - Immediate start
   - No intimidating setup
   - SQL syntax standard

#### ❌ Kekurangan

1. **Limited Concurrency**
   - Only 1 writer at a time
   - Readers block writers

2. **No Network Access**
   - File must be local
   - Cannot share across network (use NFS can corrupt)

3. **Limited User Management**
   - No built-in users/roles
   - Uses OS file permissions

4. **No Stored Procedures**
   - Cannot store complex logic in database
   - Must handle in application code

5. **Limited Scalability**
   - Not designed for massive concurrent users
   - Better alternatives for high-traffic websites

### 4.7 SQLite untuk Pembelajaran

**SQLite adalah pilihan PERFECT untuk belajar database karena:**

1. ✅ **Instant start** - no installation hassles
2. ✅ **Standard SQL** - skills transferable to other RDBMS
3. ✅ **Real database** - not a toy, production-ready
4. ✅ **See the file** - database adalah file yang bisa di-explore
5. ✅ **Free tools** - DB Browser for SQLite (GUI gratis)
6. ✅ **Documentation excellent** - SQLite docs sangat lengkap

**Learning path:**

```
Learn SQLite → Transfer to MySQL/PostgreSQL
(Effort: 1x)   (Effort: 0.2x - mostly configuration)

Alternative: Learn MySQL first
(Effort: 1.3x - deal with server setup, config, permissions)
```

**Bottom line:** Start with SQLite, scale up later if needed!

---

## 5. Instalasi dan Setup SQLite

### 5.1 Cek Apakah SQLite Sudah Terinstall

SQLite sering sudah pre-installed di banyak sistem. Cek dengan:

**Windows (Command Prompt atau PowerShell):**

```bash
sqlite3 --version
```

**Mac atau Linux (Terminal):**

```bash
sqlite3 --version
```

**Hasil expected:**

```
3.41.2 2023-03-22 11:56:21 0d1fc92f94cb6b76bffe3ec34d69cffde2924203304e3b0e0aa936de0e62b13f
```

Jika muncul output seperti di atas, berarti SQLite sudah terinstall! ✅

Jika muncul error **"command not found"** atau **"'sqlite3' is not recognized"**, lanjut ke langkah instalasi.

### 5.2 Instalasi SQLite di Windows

#### Metode 1: Download Manual (Recommended untuk Pemula)

**Step 1:** Download SQLite

1. Buka browser, kunjungi: [https://www.sqlite.org/download.html](https://www.sqlite.org/download.html)
2. Di section **"Precompiled Binaries for Windows"**, download:
   - **sqlite-tools-win32-x86-XXXXXXX.zip**

   (XXXXXXX adalah versi number, contoh: 3410200)

**Step 2:** Extract File

1. Extract file zip ke folder, misalnya: `C:\sqlite`
2. Dalam folder tersebut ada beberapa file:
   - `sqlite3.exe` - Command-line interface
   - `sqldiff.exe` - Compare databases
   - `sqlite3_analyzer.exe` - Analyze database file

**Step 3:** Tambahkan ke PATH

Agar bisa menjalankan `sqlite3` dari folder mana saja:

1. Klik kanan **"This PC"** atau **"My Computer"** → **Properties**
2. Klik **"Advanced system settings"**
3. Klik **"Environment Variables"**
4. Di section **"System variables"**, cari variable **"Path"**, klik **Edit**
5. Klik **"New"**, masukkan path: `C:\sqlite`
6. Klik **OK** di semua dialog

**Step 4:** Verifikasi Instalasi

1. Buka **Command Prompt** baru (harus baru agar PATH terupdate)
2. Ketik:

```bash
sqlite3 --version
```

3. Jika muncul versi SQLite, instalasi berhasil! ✅

#### Metode 2: Menggunakan Package Manager (Chocolatey)

Jika Anda sudah punya **Chocolatey** (package manager untuk Windows):

```bash
choco install sqlite
```

#### Metode 3: Via Python

Jika Anda sudah install Python, SQLite sudah included:

```bash
python -c "import sqlite3; print(sqlite3.sqlite_version)"
```

### 5.3 Instalasi SQLite di Mac

**Mac sudah include SQLite by default!** Tapi jika ingin versi terbaru:

#### Metode 1: Homebrew (Recommended)

```bash
# Install Homebrew jika belum punya
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install SQLite
brew install sqlite

# Verifikasi
sqlite3 --version
```

#### Metode 2: Download Manual

1. Download dari [sqlite.org/download.html](https://www.sqlite.org/download.html)
2. Pilih **"Precompiled Binaries for Mac OS X"**
3. Extract dan copy `sqlite3` ke `/usr/local/bin/`

### 5.4 Instalasi SQLite di Linux

#### Ubuntu/Debian:

```bash
sudo apt update
sudo apt install sqlite3

# Verifikasi
sqlite3 --version
```

#### Fedora/RHEL/CentOS:

```bash
sudo dnf install sqlite

# Atau untuk CentOS:
sudo yum install sqlite

# Verifikasi
sqlite3 --version
```

#### Arch Linux:

```bash
sudo pacman -S sqlite

# Verifikasi
sqlite3 --version
```

### 5.5 Menggunakan SQLite Command-Line

#### Membuka SQLite Shell

**Cara 1: Membuat database baru**

```bash
sqlite3 mydatabase.db
```

**Cara 2: In-memory database (tidak disimpan ke file)**

```bash
sqlite3
```

**Cara 3: Membuka database existing**

```bash
sqlite3 perpustakaan.db
```

#### SQLite Prompt

Setelah menjalankan command di atas, Anda masuk ke SQLite shell:

```
SQLite version 3.41.2 2023-03-22 11:56:21
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite>
```

Tanda **`sqlite>`** berarti Anda sudah masuk ke SQLite shell dan bisa menjalankan SQL commands atau dot commands.

#### Dot Commands (Meta Commands)

SQLite memiliki special commands yang dimulai dengan dot (`.`):

```sql
-- Lihat semua dot commands
.help

-- Lihat daftar tabel
.tables

-- Lihat schema tabel
.schema

-- Lihat schema tabel tertentu
.schema mahasiswa

-- Keluar dari SQLite
.quit
-- atau
.exit

-- Import CSV ke tabel
.mode csv
.import data.csv mahasiswa

-- Export tabel ke CSV
.mode csv
.output mahasiswa.csv
SELECT * FROM mahasiswa;
.output stdout

-- Lihat mode output saat ini
.mode

-- Ubah mode tampilan
.mode column      -- Tampilan kolom rapi
.mode table       -- Tampilan tabel dengan border
.mode list        -- Tampilan list (default)
.mode csv         -- CSV format
.mode json        -- JSON format

-- Enable header untuk output
.headers on

-- Lihat file database yang aktif
.databases

-- Backup database
.backup backup.db

-- Restore database
.restore backup.db

-- Execute SQL dari file
.read script.sql

-- Timer untuk measure query performance
.timer on
SELECT * FROM mahasiswa;
.timer off
```

#### Contoh Session SQLite

```bash
# Buka SQLite dengan database baru
$ sqlite3 test.db

SQLite version 3.41.2 2023-03-22 11:56:21
Enter ".help" for usage hints.
sqlite>

# Buat tabel
sqlite> CREATE TABLE mahasiswa (
   ...>   id INTEGER PRIMARY KEY,
   ...>   nama TEXT,
   ...>   nim TEXT
   ...> );

# Insert data
sqlite> INSERT INTO mahasiswa (nama, nim) VALUES ('Budi', '2101001');

# Query data
sqlite> SELECT * FROM mahasiswa;
1|Budi|2101001

# Aktifkan header dan column mode untuk tampilan lebih rapi
sqlite> .mode column
sqlite> .headers on
sqlite> SELECT * FROM mahasiswa;
id  nama  nim
--  ----  ---------
1   Budi  2101001

# Keluar
sqlite> .quit
```

### 5.6 Tips dan Shortcuts

**SQLite Shell Tips:**

1. **Arrow keys** - Navigate command history (↑↓)
2. **Clear screen** - Ketik `.shell clear` (Windows: `cls`, Mac/Linux: `clear`)
3. **Multi-line command** - Tekan Enter tanpa semicolon untuk lanjut ke line berikut
4. **Cancel command** - Ctrl+C
5. **Auto-complete** - Tab (tergantung OS)

**Best Practices:**

```sql
-- Selalu akhiri SQL statement dengan semicolon
SELECT * FROM mahasiswa;

-- Dot commands TIDAK pakai semicolon
.tables

-- Format SQL untuk readability (multi-line)
SELECT
    id,
    nama,
    nim
FROM mahasiswa
WHERE jurusan = 'Informatika'
ORDER BY nama;
```

**Troubleshooting:**

```sql
-- Jika lupa dimana Anda berada
sqlite> .databases
main: D:\testing\mydb.db r/w

-- Jika stuck di "...>" prompt
sqlite> ...> ...>
-- Ketik semicolon dan Enter untuk complete command:
sqlite> ...> ;

-- Atau Ctrl+C untuk cancel

-- Jika error "database is locked"
-- Pastikan tidak ada program lain yang buka database
-- Close semua connections dan coba lagi
```

---

## 6. Konsep Dasar Database

### 6.1 Database dan Tabel

#### Database

**Database** adalah container yang menyimpan semua data Anda. Dalam SQLite, database adalah file dengan ekstensi `.db` atau `.sqlite`.

```
perpustakaan.db                 ← Database file
├── books                       ← Tabel 1
├── members                     ← Tabel 2
├── loans                       ← Tabel 3
└── authors                     ← Tabel 4
```

#### Tabel (Table)

**Tabel** adalah struktur yang menyimpan data dalam bentuk rows dan columns. Analogi: tabel = spreadsheet Excel.

**Contoh tabel `books`:**

```
┌────────┬──────────────────────────┬──────────────┬──────┬────────┐
│ id     │ title                    │ author       │ year │ status │
├────────┼──────────────────────────┼──────────────┼──────┼────────┤
│ 1      │ Clean Code               │ Robert Martin│ 2008 │available│
│ 2      │ Design Patterns          │ Gang of Four │ 1994 │borrowed│
│ 3      │ Refactoring              │ Martin Fowler│ 1999 │available│
└────────┴──────────────────────────┴──────────────┴──────┴────────┘
```

### 6.2 Rows (Baris) dan Columns (Kolom)

#### Columns (Kolom)

**Column** adalah field yang mendefinisikan jenis data yang disimpan. Setiap column memiliki:

- **Nama** - identifier untuk column (contoh: `title`, `author`)
- **Tipe data** - jenis data yang bisa disimpan (contoh: TEXT, INTEGER)
- **Constraints** - aturan validasi (contoh: NOT NULL, UNIQUE)

**Contoh definisi columns:**

```sql
CREATE TABLE books (
    id INTEGER,          -- Column 1: id, type INTEGER
    title TEXT,          -- Column 2: title, type TEXT
    author TEXT,         -- Column 3: author, type TEXT
    year INTEGER,        -- Column 4: year, type INTEGER
    status TEXT          -- Column 5: status, type TEXT
);
```

#### Rows (Baris / Records)

**Row** adalah satu instance data dalam tabel. Juga disebut **record** atau **tuple**.

Setiap row berisi nilai untuk semua columns:

```sql
-- Row 1:
(1, 'Clean Code', 'Robert Martin', 2008, 'available')

-- Row 2:
(2, 'Design Patterns', 'Gang of Four', 1994, 'borrowed')
```

### 6.3 Primary Key (Kunci Utama)

**Primary Key** adalah column (atau kombinasi columns) yang **uniquely identifies** setiap row dalam tabel.

**Karakteristik Primary Key:**

- ✅ **Unique** - Tidak boleh ada nilai yang sama
- ✅ **Not NULL** - Tidak boleh kosong
- ✅ **Immutable** - Sebaiknya tidak diubah setelah dibuat
- ✅ **One per table** - Satu tabel hanya punya 1 primary key

**Contoh:**

```sql
CREATE TABLE students (
    id INTEGER PRIMARY KEY,      -- id adalah primary key
    nama TEXT,
    nim TEXT UNIQUE,             -- nim juga unique, tapi bukan PK
    email TEXT
);
```

**Mengapa Primary Key penting?**

1. **Identifikasi unik** - Setiap row punya identifier unik
2. **Relationship** - Digunakan untuk relasi dengan tabel lain (foreign key)
3. **Performance** - Automatic index untuk fast lookup
4. **Data integrity** - Prevent duplicate records

**Types of Primary Keys:**

**1. Auto-increment Integer (Most Common)**

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,  -- 1, 2, 3, 4, ...
    username TEXT,
    email TEXT
);

-- SQLite shorthand (preferred):
CREATE TABLE users (
    id INTEGER PRIMARY KEY,  -- Otomatis auto-increment
    username TEXT,
    email TEXT
);
```

**2. Natural Key (Dari data actual)**

```sql
CREATE TABLE countries (
    country_code TEXT PRIMARY KEY,  -- 'ID', 'US', 'JP'
    country_name TEXT
);
```

**3. Composite Key (Multiple columns)**

```sql
CREATE TABLE enrollment (
    student_id INTEGER,
    course_id INTEGER,
    semester TEXT,
    PRIMARY KEY (student_id, course_id, semester)  -- Kombinasi ketiganya unik
);
```

**4. UUID/GUID (Universally Unique Identifier)**

```sql
CREATE TABLE sessions (
    session_id TEXT PRIMARY KEY,  -- 'a1b2c3d4-e5f6-...'
    user_id INTEGER,
    created_at TEXT
);

-- Insert dengan UUID
INSERT INTO sessions (session_id, user_id, created_at)
VALUES (
    hex(randomblob(16)),  -- Generate random UUID
    123,
    datetime('now')
);
```

### 6.4 Foreign Key (Kunci Asing)

**Foreign Key** adalah column di satu tabel yang refers ke Primary Key di tabel lain. Digunakan untuk establish relationships antar tabel.

**Contoh:**

```sql
-- Tabel induk (parent)
CREATE TABLE authors (
    id INTEGER PRIMARY KEY,
    name TEXT,
    country TEXT
);

-- Tabel anak (child) - memiliki foreign key ke authors
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT,
    author_id INTEGER,            -- Foreign key
    FOREIGN KEY (author_id) REFERENCES authors(id)
);
```

**Visualisasi relationship:**

```
authors table:                 books table:
┌────┬─────────────┐          ┌────┬───────────────┬───────────┐
│ id │ name        │          │ id │ title         │ author_id │
├────┼─────────────┤          ├────┼───────────────┼───────────┤
│ 1  │ Robert C.   │ ←────────┤ 1  │ Clean Code    │ 1         │
│ 2  │ Martin F.   │          │ 2  │ Refactoring   │ 2         │ ←
└────┴─────────────┘          │ 3  │ Patterns      │ 1         │──┘
                               └────┴───────────────┴───────────┘
                                (author_id refers to authors.id)
```

**Manfaat Foreign Key:**

1. **Referential Integrity** - Ensure data consistency
2. **Prevent orphan records** - Tidak bisa delete author yang masih punya books
3. **Cascade operations** - Auto-delete/update related records

**Foreign Key Constraints:**

```sql
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT,
    author_id INTEGER,
    FOREIGN KEY (author_id) REFERENCES authors(id)
        ON DELETE CASCADE      -- Jika author dihapus, hapus juga books nya
        ON UPDATE CASCADE      -- Jika author id diupdate, update juga di books
);

-- Alternatif:
-- ON DELETE SET NULL      -- Jika author dihapus, set author_id ke NULL
-- ON DELETE RESTRICT      -- Prevent delete jika masih ada books
-- ON DELETE NO ACTION     -- Default, tidak lakukan apa-apa (bisa orphan)
```

**⚠️ Important: Enable Foreign Keys di SQLite!**

Foreign key constraints **DISABLED by default** di SQLite! Harus di-enable:

```sql
-- Enable foreign keys untuk session saat ini
PRAGMA foreign_keys = ON;

-- Cek status
PRAGMA foreign_keys;  -- 1 = enabled, 0 = disabled
```

**Untuk enable permanent**, jalankan setiap kali open database:

```bash
sqlite3 mydatabase.db
sqlite> PRAGMA foreign_keys = ON;
```

### 6.5 Schema

**Schema** adalah struktur atau blueprint dari database. Mendefinisikan:

- Tables apa saja yang ada
- Columns di setiap table
- Data types untuk setiap column
- Constraints (PRIMARY KEY, FOREIGN KEY, UNIQUE, dll)
- Indexes
- Relationships antar tables

**Contoh schema perpustakaan:**

```sql
-- Schema lengkap untuk sistem perpustakaan

-- Tabel authors
CREATE TABLE authors (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    country TEXT,
    birth_year INTEGER
);

-- Tabel books
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    author_id INTEGER,
    isbn TEXT UNIQUE,
    published_year INTEGER,
    copies_available INTEGER DEFAULT 1,
    FOREIGN KEY (author_id) REFERENCES authors(id)
);

-- Tabel members
CREATE TABLE members (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE,
    phone TEXT,
    join_date TEXT DEFAULT (date('now')),
    status TEXT DEFAULT 'active' CHECK(status IN ('active', 'suspended'))
);

-- Tabel loans
CREATE TABLE loans (
    id INTEGER PRIMARY KEY,
    book_id INTEGER NOT NULL,
    member_id INTEGER NOT NULL,
    loan_date TEXT DEFAULT (date('now')),
    due_date TEXT,
    return_date TEXT,
    FOREIGN KEY (book_id) REFERENCES books(id),
    FOREIGN KEY (member_id) REFERENCES members(id)
);
```

**Melihat schema di SQLite:**

```sql
-- Lihat semua tables
.tables

-- Lihat schema semua tables
.schema

-- Lihat schema table tertentu
.schema books

-- Lihat detail columns
PRAGMA table_info(books);
```

### 6.6 NULL Values

**NULL** adalah special value yang menandakan **"no value"** atau **"unknown value"**.

**NULL ≠ 0, NULL ≠ empty string, NULL ≠ false**

```sql
-- Contoh NULL values
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,        -- Tidak boleh NULL
    description TEXT,          -- Boleh NULL
    price REAL NOT NULL,       -- Tidak boleh NULL
    discount REAL              -- Boleh NULL jika tidak ada discount
);

INSERT INTO products (name, price)
VALUES ('Laptop', 5000000);
-- description dan discount akan NULL

-- Query dengan NULL
SELECT * FROM products WHERE description IS NULL;     -- ✅ Correct
SELECT * FROM products WHERE description = NULL;      -- ❌ Wrong! Always false

-- NULL dalam operasi
SELECT 10 + NULL;           -- Result: NULL
SELECT 'Hello' || NULL;     -- Result: NULL
SELECT NULL = NULL;         -- Result: NULL (not TRUE!)
```

**Handling NULL:**

```sql
-- Cek NULL
WHERE column IS NULL
WHERE column IS NOT NULL

-- Replace NULL dengan default value
SELECT name, COALESCE(discount, 0) as discount FROM products;
-- Jika discount NULL, gunakan 0

-- NULLIF - return NULL jika dua values sama
SELECT NULLIF(status, 'unknown');  -- Return NULL jika status = 'unknown'

-- IFNULL (SQLite specific)
SELECT name, IFNULL(description, 'No description') FROM products;
```

### 6.7 Indexes

**Index** adalah struktur data yang meningkatkan kecepatan query, seperti index di buku yang memudahkan menemukan halaman.

**Analogi:**

```
Tanpa Index:                   Dengan Index:
━━━━━━━━━━━━━                  ━━━━━━━━━━━━━
Cari "Ahmad" →                 Cari "Ahmad" →
Scan semua rows                Langsung ke location
(Slow! O(n))                   (Fast! O(log n))
```

**Membuat Index:**

```sql
-- Index pada single column
CREATE INDEX idx_books_title ON books(title);

-- Index pada multiple columns (composite index)
CREATE INDEX idx_loans_book_member ON loans(book_id, member_id);

-- Unique index
CREATE UNIQUE INDEX idx_members_email ON members(email);
```

**Kapan menggunakan Index?**

✅ **Buat index ketika:**

- Column sering di-query dalam WHERE clause
- Column digunakan untuk JOIN
- Column digunakan untuk ORDER BY
- Column memiliki high cardinality (banyak unique values)

❌ **Jangan buat index ketika:**

- Table sangat kecil (< 1000 rows)
- Column jarang di-query
- Column sering di-update (index harus di-update juga)
- Column memiliki low cardinality (sedikit unique values, contoh: gender)

**Trade-offs:**

```
Pros:                          Cons:
✅ Faster SELECT queries        ❌ Slower INSERT/UPDATE/DELETE
✅ Faster JOIN operations       ❌ Extra disk space
✅ Faster WHERE filtering       ❌ Maintenance overhead
```

---

## 7. Tipe Data di SQLite

SQLite memiliki sistem tipe data yang **flexible** dan **simple**. Ada 5 **storage classes**:

### 7.1 Storage Classes di SQLite

| Storage Class | Description                 | Example Values                      |
| ------------- | --------------------------- | ----------------------------------- |
| **NULL**      | Null value                  | NULL                                |
| **INTEGER**   | Signed integer              | 1, -5, 1000000, 9223372036854775807 |
| **REAL**      | Floating point              | 3.14, -0.001, 1.23e10               |
| **TEXT**      | Text string (UTF-8, UTF-16) | 'Hello', 'Indonesia 🇮🇩'             |
| **BLOB**      | Binary data                 | Images, files, binary data          |

### 7.2 Type Affinity

SQLite menggunakan **type affinity** - column memiliki "preferred" type, tapi bisa menyimpan type lain juga (dynamic typing).

**Type affinities:**

1. **TEXT** - store as TEXT
2. **NUMERIC** - store as INTEGER atau REAL, tergantung nilai
3. **INTEGER** - prefer INTEGER
4. **REAL** - prefer REAL
5. **BLOB** - prefer BLOB

**Contoh:**

```sql
CREATE TABLE test (
    col_text TEXT,
    col_integer INTEGER,
    col_real REAL
);

-- SQLite flexible dengan types
INSERT INTO test VALUES ('123', '123', '123');  -- Semua stored sesuai column affinity
INSERT INTO test VALUES (456, 456, 456);        -- Semua stored as number
INSERT INTO test VALUES ('text', 'text', 'text'); -- col_integer dan col_real tetap simpan TEXT!
```

### 7.3 Tipe Data TEXT

**TEXT** menyimpan string text dengan encoding UTF-8 atau UTF-16.

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL,
    full_name TEXT,
    bio TEXT,
    country TEXT DEFAULT 'Indonesia'
);

-- Insert text
INSERT INTO users (username, full_name, bio, country) VALUES
('budi123', 'Budi Santoso', 'Software engineer di Jakarta', 'Indonesia'),
('sarah_m', 'Sarah Miller', 'Love coding 💻', 'USA');

-- Text operations
SELECT
    username,
    LENGTH(username) as length,           -- Panjang string
    UPPER(username) as uppercase,         -- Uppercase
    LOWER(username) as lowercase,         -- Lowercase
    SUBSTR(username, 1, 4) as first_4     -- Substring
FROM users;
```

**Text functions:**

| Function         | Description                 | Example                              |
| ---------------- | --------------------------- | ------------------------------------ |
| LENGTH(str)      | Length of string            | LENGTH('hello') → 5                  |
| UPPER(str)       | Convert to uppercase        | UPPER('hello') → 'HELLO'             |
| LOWER(str)       | Convert to lowercase        | LOWER('HELLO') → 'hello'             |
| SUBSTR(str,x,y)  | Extract substring           | SUBSTR('hello', 2, 3) → 'ell'        |
| REPLACE(str,x,y) | Replace x with y            | REPLACE('hello', 'l', 'L') → 'heLLo' |
| TRIM(str)        | Remove whitespace           | TRIM(' hello ') → 'hello'            |
| \|\|             | Concatenation operator      | 'hello' \|\| ' ' \|\| 'world'        |
| LIKE             | Pattern matching            | WHERE name LIKE 'A%'                 |
| GLOB             | Unix-style pattern matching | WHERE name GLOB 'A\*'                |

### 7.4 Tipe Data INTEGER

**INTEGER** menyimpan signed integer (bilangan bulat). Range: -9,223,372,036,854,775,808 hingga 9,223,372,036,854,775,807 (64-bit).

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT,
    price INTEGER,              -- Harga dalam rupiah (integer)
    stock INTEGER DEFAULT 0,
    sold INTEGER DEFAULT 0,
    rating INTEGER CHECK(rating >= 1 AND rating <= 5)
);

-- Insert integers
INSERT INTO products (name, price, stock, rating) VALUES
('Laptop', 5000000, 10, 5),
('Mouse', 150000, 50, 4),
('Keyboard', 300000, 30, 4);

-- Math operations
SELECT
    name,
    price,
    stock,
    price * stock as total_inventory_value,
    sold * 100.0 / (sold + stock) as sold_percentage
FROM products;
```

**INTEGER operations:**

| Operation      | Operator | Example     |
| -------------- | -------- | ----------- |
| Addition       | +        | 5 + 3 → 8   |
| Subtraction    | -        | 5 - 3 → 2   |
| Multiplication | \*       | 5 \* 3 → 15 |
| Division       | /        | 5 / 2 → 2   |
| Modulo         | %        | 5 % 2 → 1   |

**⚠️ INTEGER Division:**

```sql
SELECT 5 / 2;           -- Result: 2 (INTEGER division!)
SELECT 5 / 2.0;         -- Result: 2.5 (REAL division)
SELECT 5 * 1.0 / 2;     -- Result: 2.5 (REAL division)
SELECT CAST(5 AS REAL) / 2;  -- Result: 2.5 (REAL division)
```

### 7.5 Tipe Data REAL

**REAL** menyimpan floating-point numbers (bilangan desimal). 8-byte IEEE floating point.

```sql
CREATE TABLE measurements (
    id INTEGER PRIMARY KEY,
    temperature REAL,     -- Celsius
    humidity REAL,        -- Percentage
    pressure REAL,        -- Atmospheric pressure
    latitude REAL,        -- GPS coordinate
    longitude REAL,       -- GPS coordinate
    timestamp TEXT
);

-- Insert REAL values
INSERT INTO measurements VALUES
(1, 28.5, 65.3, 1013.25, -6.2088, 106.8456, '2026-03-31 10:00:00'),
(2, 29.1, 63.8, 1012.80, -6.2088, 106.8456, '2026-03-31 11:00:00');

-- REAL calculations
SELECT
    temperature,
    ROUND(temperature, 1) as temp_rounded,
    ROUND(temperature * 9.0 / 5.0 + 32, 2) as fahrenheit,
    ABS(temperature - 28) as diff_from_normal
FROM measurements;
```

**REAL functions:**

| Function    | Description               | Example                  |
| ----------- | ------------------------- | ------------------------ |
| ROUND(x)    | Round to nearest integer  | ROUND(3.7) → 4.0         |
| ROUND(x, y) | Round to y decimal places | ROUND(3.14159, 2) → 3.14 |
| ABS(x)      | Absolute value            | ABS(-5.5) → 5.5          |
| CEIL(x)     | Round up                  | Tidak ada di SQLite      |
| FLOOR(x)    | Round down                | Tidak ada di SQLite      |

**⚠️ Floating Point Precision:**

```sql
-- Floating-point dapat memiliki precision issues
SELECT 0.1 + 0.2;                    -- Mungkin: 0.30000000000000004
SELECT ROUND(0.1 + 0.2, 10);         -- 0.3

-- Untuk financial data, lebih baik gunakan INTEGER (store in cents/sen)
-- ✅ Good: Store Rp 100.000 as 100000 (integer sen/rupiah)
-- ❌ Bad: Store as 100000.00 (REAL, precision issues)
```

### 7.6 Tipe Data BLOB

**BLOB (Binary Large OBject)** menyimpan data binary exactly as input. Untuk images, files, encrypted data.

```sql
CREATE TABLE files (
    id INTEGER PRIMARY KEY,
    filename TEXT,
    file_content BLOB,     -- Binary data
    file_size INTEGER,
    upload_date TEXT
);

-- Insert BLOB (example from command-line)
-- Biasanya dilakukan via programming language

-- Query BLOB
SELECT
    filename,
    file_size,
    LENGTH(file_content) as actual_size,
    upload_date
FROM files;
```

**Use cases untuk BLOB:**

- 📷 Images (photos, thumbnails)
- 📄 Documents (PDF, Word files)
- 🔐 Encrypted data
- 🎵 Audio files (small files)
- 🎬 Video files (very small clips only)

**⚠️ Best Practice:**

Untuk files besar, **lebih baik simpan di filesystem** dan simpan path di database:

```sql
-- ✅ Better approach for large files
CREATE TABLE documents (
    id INTEGER PRIMARY KEY,
    filename TEXT,
    file_path TEXT,        -- Store path: /uploads/2026/03/document.pdf
    file_size INTEGER,
    mime_type TEXT,
    upload_date TEXT
);
```

**Alasan:**

- Database file size tetap manageable
- Faster backup/restore
- Easier to serve files with web server
- CDN integration lebih mudah

### 7.7 Date and Time

**SQLite tidak memiliki dedicated DATE atau TIME type.** Date/time disimpan sebagai:

1. **TEXT** - ISO 8601 format: "YYYY-MM-DD HH:MM:SS"
2. **INTEGER** - Unix timestamp (seconds since 1970-01-01)
3. **REAL** - Julian day numbers

**Recommended: TEXT dengan ISO 8601 format**

```sql
CREATE TABLE events (
    id INTEGER PRIMARY KEY,
    event_name TEXT,
    event_date TEXT,            -- DATE: 'YYYY-MM-DD'
    event_time TEXT,            -- TIME: 'HH:MM:SS'
    event_datetime TEXT,        -- DATETIME: 'YYYY-MM-DD HH:MM:SS'
    created_at TEXT DEFAULT (datetime('now'))
);

-- Insert dengan date/time functions
INSERT INTO events (event_name, event_date, event_time, event_datetime) VALUES
('Meeting', date('now'), time('now'), datetime('now')),
('Conference', '2026-05-15', '09:00:00', '2026-05-15 09:00:00');

-- Query date/time
SELECT
    event_name,
    event_datetime,
    date(event_datetime) as date_only,
    time(event_datetime) as time_only,
    strftime('%Y', event_datetime) as year,
    strftime('%m', event_datetime) as month,
    strftime('%d', event_datetime) as day,
    strftime('%H:%M', event_datetime) as hour_minute
FROM events;
```

**Date/Time functions:**

| Function          | Description       | Example                            |
| ----------------- | ----------------- | ---------------------------------- |
| date('now')       | Current date      | '2026-03-31'                       |
| time('now')       | Current time      | '14:30:45'                         |
| datetime('now')   | Current datetime  | '2026-03-31 14:30:45'              |
| julianday('now')  | Julian day number | 2460396.1046875                    |
| strftime(fmt, dt) | Format datetime   | strftime('%Y-%m', datetime('now')) |

**Date calculations:**

```sql
-- Add/subtract days
SELECT date('now', '+7 days');          -- 7 hari dari sekarang
SELECT date('now', '-1 month');         -- 1 bulan lalu
SELECT datetime('now', '+2 hours');     -- 2 jam dari sekarang

-- Difference between dates
SELECT julianday('2026-12-31') - julianday('2026-01-01') as days;  -- Days in 2026

-- Format dates
SELECT strftime('%d/%m/%Y', '2026-03-31');  -- Indonesian format: 31/03/2026
SELECT strftime('%W', date('now'));         -- Week number of year
```

**Format specifiers untuk strftime:**

| Specifier | Description       | Example    |
| --------- | ----------------- | ---------- |
| %Y        | Year (4 digits)   | 2026       |
| %m        | Month (01-12)     | 03         |
| %d        | Day (01-31)       | 31         |
| %H        | Hour (00-23)      | 14         |
| %M        | Minute (00-59)    | 30         |
| %S        | Second (00-59)    | 45         |
| %w        | Day of week (0-6) | 1 (Monday) |
| %W        | Week number       | 13         |
| %j        | Day of year       | 090        |
| %s        | Unix timestamp    | 1711891845 |

### 7.8 Boolean Values

**SQLite tidak memiliki dedicated BOOLEAN type.** Boolean disimpan sebagai INTEGER:

- **0** = FALSE
- **1** = TRUE

```sql
CREATE TABLE features (
    id INTEGER PRIMARY KEY,
    feature_name TEXT,
    is_enabled INTEGER DEFAULT 0,    -- 0 = false, 1 = true
    is_premium INTEGER DEFAULT 0,
    is_beta INTEGER DEFAULT 0,
    CHECK(is_enabled IN (0, 1)),
    CHECK(is_premium IN (0, 1)),
    CHECK(is_beta IN (0, 1))
);

-- Insert boolean values
INSERT INTO features (feature_name, is_enabled, is_premium) VALUES
('Dark Mode', 1, 0),       -- enabled, not premium
('Export PDF', 1, 1),      -- enabled, premium
('AI Assistant', 0, 1);    -- disabled, premium

-- Query boolean
SELECT * FROM features WHERE is_enabled = 1;              -- Enabled features
SELECT * FROM features WHERE is_premium = 1 AND is_enabled = 1;  -- Premium enabled features
```

**Alternative: TEXT untuk readability**

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT,
    status TEXT DEFAULT 'inactive' CHECK(status IN ('active', 'inactive', 'suspended'))
);
```

### 7.9 Type Conversion (Casting)

**CAST** function untuk konversi explicit antara types:

```sql
-- INTEGER to TEXT
SELECT CAST(12345 AS TEXT);              -- '12345'

-- TEXT to INTEGER
SELECT CAST('12345' AS INTEGER);         -- 12345
SELECT CAST('12.34' AS INTEGER);         -- 12 (truncated)
SELECT CAST('hello' AS INTEGER);         -- 0 (invalid conversion)

-- INTEGER to REAL
SELECT CAST(5 AS REAL);                  -- 5.0

-- TEXT to REAL
SELECT CAST('3.14159' AS REAL);          -- 3.14159

-- REAL to INTEGER
SELECT CAST(3.14159 AS INTEGER);         -- 3 (truncated)
```

**Type conversion dalam expressions:**

```sql
-- Automatic conversion
SELECT '5' + 3;              -- 8 (TEXT '5' converted to INTEGER)
SELECT 5 || 3;               -- '53' (INTEGER concatenated as TEXT)
SELECT '5.5' * 2;            -- 11.0 (TEXT '5.5' converted to REAL)

-- Explicit for clarity
SELECT CAST('5' AS INTEGER) + 3;    -- 8
SELECT CAST(5 AS TEXT) || CAST(3 AS TEXT);  -- '53'
```

---

## 8. Membuat Database dan Tabel

### 8.1 Membuat Database di SQLite

Di SQLite, database dibuat otomatis ketika Anda specify filename:

```bash
# Cara 1: Buat database baru
sqlite3 perpustakaan.db

# Cara 2: Buat in-memory database (tidak disimpan ke file)
sqlite3 :memory:

# Cara 3: Open multiple databases
sqlite3
sqlite> ATTACH DATABASE 'database1.db' AS db1;
sqlite> ATTACH DATABASE 'database2.db' AS db2;
```

**Lokasi file:**

```bash
# Database disimpan di current working directory
# Windows: C:\Users\YourName\perpustakaan.db
# Mac/Linux: /home/yourname/perpustakaan.db

# Specify full path
sqlite3 "C:\Users\Jim\Documents\perpustakaan.db"
sqlite3 "/home/jim/databases/perpustakaan.db"
```

### 8.2 CREATE TABLE - Syntax Dasar

**Syntax CREATE TABLE:**

```sql
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    column3 datatype constraints,
    ...
    table_constraints
);
```

**Contoh sederhana:**

```sql
-- Buat tabel mahasiswa
CREATE TABLE mahasiswa (
    id INTEGER PRIMARY KEY,
    nama TEXT NOT NULL,
    nim TEXT UNIQUE NOT NULL,
    jurusan TEXT,
    angkatan INTEGER
);
```

**Let's break it down:**

```sql
CREATE TABLE mahasiswa (              -- Nama tabel: mahasiswa
    id INTEGER PRIMARY KEY,            -- Column 1: id, auto-increment primary key
    nama TEXT NOT NULL,                -- Column 2: nama, text, tidak boleh NULL
    nim TEXT UNIQUE NOT NULL,          -- Column 3: nim, text, harus unique
    jurusan TEXT,                      -- Column 4: jurusan, optional (boleh NULL)
    angkatan INTEGER                   -- Column 5: angkatan, integer, optional
);
```

### 8.3 Naming Conventions

**Best practices untuk naming:**

✅ **DO:**

- gunakan **lowercase** dengan **underscore** (snake_case)
- Nama deskriptif dan jelas
- Plural untuk table names: `users`, `products`, `orders`
- Singular untuk column names: `user_id`, `product_name`

```sql
-- ✅ Good naming
CREATE TABLE book_authors (
    book_id INTEGER,
    author_id INTEGER,
    contribution_role TEXT
);
```

❌ **DON'T:**

- Hindari nama yang merupakan SQL keywords: `order`, `user`, `group`
- Avoid spaces dan special characters
- Avoid camelCase atau PascalCase (prefer snake_case)
- Avoid prefix: `tbl_users`, `db_products`

```sql
-- ❌ Bad naming
CREATE TABLE "User Data" (        -- Spaces require quotes
    UserID INTEGER,               -- CamelCase inconsistent
    user_name TEXT,               -- Mixed conventions
    "Order" TEXT                  -- SQL keyword
);
```

**Reserved keywords workaround:**

Jika harus menggunakan keyword, wrap dengan quotes:

```sql
CREATE TABLE "order" (            -- Not recommended
    id INTEGER PRIMARY KEY,
    "user" TEXT,
    total REAL
);

-- Better: use different names
CREATE TABLE orders (              -- ✅ Recommended
    id INTEGER PRIMARY KEY,
    user_id INTEGER,
    total REAL
);
```

### 8.4 Column Constraints

**Constraints** adalah rules yang diterapkan ke column untuk enforce data integrity.

#### NOT NULL

Column tidak boleh berisi NULL value.

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT NOT NULL,         -- Harus diisi
    email TEXT NOT NULL,            -- Harus diisi
    bio TEXT                        -- Optional, boleh NULL
);

-- ✅ Valid insert
INSERT INTO users (username, email) VALUES ('budi', 'budi@email.com');

-- ❌ Error: username tidak boleh NULL
INSERT INTO users (email) VALUES ('test@email.com');
```

#### UNIQUE

Value dalam column harus unique (tidak boleh duplicate).

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,  -- Setiap username harus unique
    email TEXT UNIQUE NOT NULL,     -- Setiap email harus unique
    phone TEXT
);

-- ✅ Valid insert
INSERT INTO users (username, email) VALUES ('budi', 'budi@email.com');

-- ❌ Error: username 'budi' sudah ada
INSERT INTO users (username, email) VALUES ('budi', 'another@email.com');
```

#### DEFAULT

Nilai default jika tidak di-specify saat INSERT.

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    price REAL NOT NULL,
    stock INTEGER DEFAULT 0,                    -- Default: 0
    status TEXT DEFAULT 'available',            -- Default: 'available'
    created_at TEXT DEFAULT (datetime('now'))   -- Default: current datetime
);

-- Tidak specify stock, status, created_at → use defaults
INSERT INTO products (name, price) VALUES ('Laptop', 5000000);

-- Result:
-- stock = 0, status = 'available', created_at = '2026-03-31 14:30:45'
```

#### CHECK

Validasi custom untuk nilai column.

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    price REAL CHECK(price > 0),                -- Price harus positif
    discount REAL CHECK(discount >= 0 AND discount <= 100),  -- 0-100%
    rating INTEGER CHECK(rating BETWEEN 1 AND 5),  -- Rating 1-5
    status TEXT CHECK(status IN ('draft', 'published', 'archived'))
);

-- ✅ Valid insert
INSERT INTO products (name, price, discount, rating, status)
VALUES ('Laptop', 5000000, 10, 5, 'published');

-- ❌ Error: price harus > 0
INSERT INTO products (name, price) VALUES ('Item', -100);

-- ❌ Error: discount tidak boleh > 100
INSERT INTO products (name, price, discount) VALUES ('Item', 1000, 150);
```

#### PRIMARY KEY

Mengidentifikasi unique setiap row. Auto-increment di SQLite.

```sql
-- Cara 1: PRIMARY KEY pada satu column
CREATE TABLE users (
    id INTEGER PRIMARY KEY,         -- Auto-increment
    username TEXT
);

-- Cara 2: PRIMARY KEY AUTOINCREMENT (explicit)
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT
);

-- Cara 3: Composite PRIMARY KEY (multiple columns)
CREATE TABLE enrollment (
    student_id INTEGER,
    course_id INTEGER,
    semester TEXT,
    PRIMARY KEY (student_id, course_id, semester)
);
```

**Perbedaan PRIMARY KEY vs PRIMARY KEY AUTOINCREMENT:**

| Feature           | PRIMARY KEY | PRIMARY KEY AUTOINCREMENT |
| ----------------- | ----------- | ------------------------- |
| Auto-increment    | ✅ Yes      | ✅ Yes                    |
| Reuse deleted IDs | ✅ Possible | ❌ Never reuse            |
| Performance       | ✅ Faster   | Slightly slower           |
| Use case          | Most cases  | When ID reuse is problem  |

**Recommendation:** Use `INTEGER PRIMARY KEY` (without AUTOINCREMENT) for most cases.

### 8.5 Table Constraints

Constraints yang diterapkan pada level table (bukan column).

#### FOREIGN KEY

```sql
-- Enable foreign keys
PRAGMA foreign_keys = ON;

CREATE TABLE authors (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    author_id INTEGER,
    FOREIGN KEY (author_id) REFERENCES authors(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

#### UNIQUE (Multiple Columns)

```sql
CREATE TABLE user_roles (
    user_id INTEGER,
    role_id INTEGER,
    organization_id INTEGER,
    UNIQUE(user_id, role_id, organization_id)  -- Kombinasi harus unique
);
```

#### CHECK (Table-level)

```sql
CREATE TABLE events (
    id INTEGER PRIMARY KEY,
    start_date TEXT,
    end_date TEXT,
    CHECK(end_date > start_date)    -- end_date harus setelah start_date
);
```

### 8.6 CREATE TABLE Examples

**Example 1: Simple table**

```sql
CREATE TABLE categories (
    id INTEGER PRIMARY KEY,
    name TEXT UNIQUE NOT NULL,
    description TEXT,
    created_at TEXT DEFAULT (datetime('now'))
);
```

**Example 2: Table dengan validasi lengkap**

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    sku TEXT UNIQUE NOT NULL,
    name TEXT NOT NULL,
    description TEXT,
    price REAL NOT NULL CHECK(price > 0),
    cost REAL CHECK(cost > 0),
    stock INTEGER DEFAULT 0 CHECK(stock >= 0),
    category_id INTEGER,
    is_active INTEGER DEFAULT 1 CHECK(is_active IN (0, 1)),
    created_at TEXT DEFAULT (datetime('now')),
    updated_at TEXT DEFAULT (datetime('now')),
    FOREIGN KEY (category_id) REFERENCES categories(id),
    CHECK(price > cost)  -- Selling price harus lebih besar dari cost
);
```

**Example 3: Many-to-many junction table**

```sql
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    isbn TEXT UNIQUE
);

CREATE TABLE authors (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

-- Junction table untuk many-to-many relationship
CREATE TABLE book_authors (
    book_id INTEGER,
    author_id INTEGER,
    author_order INTEGER DEFAULT 1,  -- Urutan author (first author, second author)
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES authors(id) ON DELETE CASCADE
);
```

### 8.7 IF NOT EXISTS

Prevent error jika table sudah ada:

```sql
-- ✅ Safe: tidak error jika table sudah ada
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY,
    username TEXT
);

-- ❌ Error jika table sudah ada
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT
);
-- Error: table users already exists
```

### 8.8 Melihat dan Mengelola Tables

```sql
-- Lihat semua tables
.tables

-- Lihat schema table
.schema users

-- Lihat detail columns
PRAGMA table_info(users);

-- Drop (hapus) table
DROP TABLE IF EXISTS old_table;

-- Rename table
ALTER TABLE users RENAME TO customers;

-- Add column ke table existing
ALTER TABLE users ADD COLUMN phone TEXT;

-- ⚠️ SQLite limitations:
-- Tidak bisa: DROP COLUMN, MODIFY COLUMN, ADD CONSTRAINT
-- Workaround: Create new table, copy data, drop old table, rename new table
```

### 8.9 ALTER TABLE

SQLite memiliki limited ALTER TABLE support.

**Yang bisa dilakukan:**

```sql
-- Rename table
ALTER TABLE old_name RENAME TO new_name;

-- Add column
ALTER TABLE users ADD COLUMN phone TEXT;

-- Add column dengan DEFAULT
ALTER TABLE users ADD COLUMN status TEXT DEFAULT 'active';

-- Rename column (SQLite 3.25.0+)
ALTER TABLE users RENAME COLUMN old_column TO new_column;

-- Drop column (SQLite 3.35.0+)
ALTER TABLE users DROP COLUMN unwanted_column;
```

**Yang TIDAK bisa dilakukan directly:**

- Modify column type
- Add PRIMARY KEY constraint
- Add FOREIGN KEY constraint
- Add UNIQUE constraint

**Workaround (recreate table):**

```sql
-- Contoh: Add UNIQUE constraint ke existing column

-- Step 1: Rename old table
ALTER TABLE users RENAME TO users_old;

-- Step 2: Create new table dengan constraint baru
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE NOT NULL,  -- Add UNIQUE
    email TEXT
);

-- Step 3: Copy data
INSERT INTO users SELECT * FROM users_old;

-- Step 4: Drop old table
DROP TABLE users_old;
```

---

## 9. Operasi CRUD Dasar

**CRUD** = **C**reate, **R**ead, **U**pdate, **D**elete - operasi fundamental dalam database.

```
CREATE → INSERT
READ   → SELECT
UPDATE → UPDATE
DELETE → DELETE
```

### 9.1 INSERT - Menambah Data

#### INSERT: Syntax Dasar

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

#### INSERT: Single Row

```sql
-- Buat table untuk contoh
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    nama TEXT NOT NULL,
    nim TEXT UNIQUE NOT NULL,
    jurusan TEXT,
    angkatan INTEGER
);

-- Insert satu row
INSERT INTO students (nama, nim, jurusan, angkatan)
VALUES ('Budi Santoso', '2101001', 'Informatika', 2021);

-- Insert tanpa specify columns (harus lengkap sesuai urutan)
INSERT INTO students
VALUES (NULL, 'Siti Nurhaliza', '2101002', 'Sistem Informasi', 2021);
-- NULL otomatis jadi auto-increment ID
```

#### INSERT: Multiple Rows

```sql
-- Insert beberapa rows sekaligus (lebih efficient)
INSERT INTO students (nama, nim, jurusan, angkatan) VALUES
('Ahmad Dahlan', '2101003', 'Informatika', 2021),
('Dewi Lestari', '2101004', 'Sistem Informasi', 2021),
('Rahman Hidayat', '2101005', 'Informatika', 2022),
('Ani Wijaya', '2101006', 'Teknik Komputer', 2022);
```

#### INSERT: Dengan DEFAULT Values

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    price REAL NOT NULL,
    stock INTEGER DEFAULT 0,
    status TEXT DEFAULT 'available',
    created_at TEXT DEFAULT (datetime('now'))
);

-- Tidak specify stock, status, created_at → use defaults
INSERT INTO products (name, price)
VALUES ('Laptop', 5000000);

-- Result:
-- id = 1, name = 'Laptop', price = 5000000,
-- stock = 0, status = 'available', created_at = '2026-03-31 14:30:45'
```

#### INSERT: SELECT (Copy dari table lain)

```sql
-- Insert data dari hasil SELECT query
INSERT INTO students_archive
SELECT * FROM students WHERE angkatan < 2020;

-- Insert with transformation
INSERT INTO summary (jurusan, total_students)
SELECT jurusan, COUNT(*)
FROM students
GROUP BY jurusan;
```

### 9.2 SELECT - Membaca Data

#### SELECT: Syntax Dasar

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column
LIMIT number;
```

#### SELECT: All Columns

```sql
-- Ambil semua columns dan rows
SELECT * FROM students;
```

**Output:**

```
id  nama              nim      jurusan            angkatan
--  ----              ---      -------            --------
1   Budi Santoso      2101001  Informatika        2021
2   Siti Nurhaliza    2101002  Sistem Informasi   2021
3   Ahmad Dahlan      2101003  Informatika        2021
```

#### SELECT: Specific Columns

```sql
-- Ambil columns tertentu saja
SELECT nama, nim, jurusan FROM students;
```

**Output:**

```
nama              nim      jurusan
----              ---      -------
Budi Santoso      2101001  Informatika
Siti Nurhaliza    2101002  Sistem Informasi
```

#### SELECT: Column Aliases

```sql
-- Gunakan alias untuk rename columns di output
SELECT
    nama AS 'Nama Lengkap',
    nim AS 'NIM',
    jurusan AS 'Program Studi'
FROM students;
```

**Output:**

```
Nama Lengkap      NIM      Program Studi
------------      ---      -------------
Budi Santoso      2101001  Informatika
```

#### SELECT: Calculated Columns

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT,
    price REAL,
    cost REAL,
    stock INTEGER
);

INSERT INTO products VALUES
(1, 'Laptop', 5000000, 4000000, 10),
(2, 'Mouse', 150000, 100000, 50);

-- Calculations dalam SELECT
SELECT
    name,
    price,
    cost,
    price - cost AS profit,
    (price - cost) * stock AS total_profit,
    ROUND((price - cost) * 100.0 / price, 2) AS profit_margin_percent
FROM products;
```

**Output:**

```
name    price    cost     profit  total_profit  profit_margin_percent
------  -------  -------  ------  ------------  ---------------------
Laptop  5000000  4000000  1000000 10000000      20.0
Mouse   150000   100000   50000   2500000       33.33
```

### 9.3 UPDATE - Mengubah Data

#### UPDATE: Syntax Dasar

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

**⚠️ WARNING: Jika tidak pakai WHERE, semua rows akan diupdate!**

#### UPDATE: Single Row

```sql
-- Update berdasarkan ID
UPDATE students
SET jurusan = 'Teknik Komputer'
WHERE id = 1;

-- Update dengan kondisi
UPDATE students
SET angkatan = 2020
WHERE nim = '2101001';
```

#### UPDATE: Multiple Columns

```sql
-- Update beberapa columns sekaligus
UPDATE students
SET
    jurusan = 'Informatika',
    angkatan = 2022
WHERE id = 3;
```

#### UPDATE: Multiple Rows

```sql
-- Update banyak rows sekaligus
UPDATE students
SET angkatan = 2023
WHERE jurusan = 'Informatika';

-- Update dengan calculation
UPDATE products
SET price = price * 1.1  -- Naikkan harga 10%
WHERE category = 'Electronics';
```

#### UPDATE: Dengan Subquery

```sql
-- Update berdasarkan data dari table lain
UPDATE students
SET jurusan = (SELECT name FROM departments WHERE id = 5)
WHERE nim = '2101001';
```

### 9.4 DELETE - Menghapus Data

#### DELETE: Syntax Dasar

```sql
DELETE FROM table_name
WHERE condition;
```

**⚠️ WARNING: Jika tidak pakai WHERE, semua rows akan dihapus!**

#### DELETE: Single Row

```sql
-- Delete berdasarkan ID
DELETE FROM students WHERE id = 1;

-- Delete berdasarkan kondisi lain
DELETE FROM students WHERE nim = '2101001';
```

#### DELETE: Multiple Rows

```sql
-- Delete banyak rows
DELETE FROM students WHERE angkatan < 2020;

-- Delete dengan IN clause
DELETE FROM students WHERE jurusan IN ('Teknik Komputer', 'Teknik Elektro');
```

#### DELETE: All Rows

```sql
-- Hapus semua data (struktur table tetap ada)
DELETE FROM students;

-- Alternative yang lebih cepat (tidak bisa di-rollback)
DELETE FROM students;  -- SQLite doesn't have TRUNCATE, use DELETE
```

### 9.5 CRUD: Complete Example

Mari kita buat complete example dengan semua operasi CRUD:

```sql
-- CREATE: Buat table
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    author TEXT NOT NULL,
    year INTEGER,
    price REAL,
    stock INTEGER DEFAULT 0,
    created_at TEXT DEFAULT (datetime('now'))
);

-- INSERT: Tambah data (CREATE)
INSERT INTO books (title, author, year, price, stock) VALUES
('Clean Code', 'Robert Martin', 2008, 450000, 10),
('Design Patterns', 'Gang of Four', 1994, 500000, 5),
('Refactoring', 'Martin Fowler', 1999, 480000, 8),
('The Pragmatic Programmer', 'Hunt & Thomas', 1999, 420000, 12);

-- SELECT: Baca data (READ)
-- Lihat semua buku
SELECT * FROM books;

-- Lihat buku tertentu
SELECT title, author, price FROM books WHERE year > 2000;

-- Lihat buku dengan sorting
SELECT title, author, price FROM books ORDER BY price DESC;

-- UPDATE: Update data (UPDATE)
-- Update harga buku
UPDATE books SET price = 475000 WHERE title = 'Clean Code';

-- Update stock setelah dijual
UPDATE books SET stock = stock - 1 WHERE id = 1;

-- Diskon 10% untuk buku published sebelum 2000
UPDATE books SET price = price * 0.9 WHERE year < 2000;

-- DELETE: Hapus data (DELETE)
-- Hapus buku yang out of stock
DELETE FROM books WHERE stock = 0;

-- Hapus buku lama
DELETE FROM books WHERE year < 1990;

-- VERIFY: Cek hasil akhir
SELECT * FROM books;
```

### 9.6 REPLACE - Insert or Update

**REPLACE** adalah shorthand untuk INSERT or UPDATE:

```sql
-- Buat table dengan UNIQUE constraint
CREATE TABLE settings (
    key TEXT PRIMARY KEY,
    value TEXT,
    updated_at TEXT
);

-- INSERT pertama kali
INSERT INTO settings VALUES ('theme', 'dark', datetime('now'));

-- REPLACE: jika key sudah ada, replace; jika belum, insert
REPLACE INTO settings VALUES ('theme', 'light', datetime('now'));
-- Result: row dengan key='theme' di-replace

-- Alternative syntax
INSERT OR REPLACE INTO settings VALUES ('language', 'id', datetime('now'));
```

**⚠️ Difference antara REPLACE dan UPDATE:**

- **REPLACE**: DELETE old row + INSERT new row (reset semua columns)
- **UPDATE**: Modify existing row (columns lain tidak berubah)

```sql
-- Setup
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE,
    email TEXT,
    created_at TEXT DEFAULT (datetime('now'))
);

INSERT INTO users (username, email) VALUES ('budi', 'budi@email.com');
-- created_at = '2026-03-31 10:00:00'

-- UPDATE: hanya email yang berubah, created_at tetap
UPDATE users SET email = 'new@email.com' WHERE username = 'budi';
-- created_at masih '2026-03-31 10:00:00'

-- REPLACE: seluruh row di-replace, created_at jadi baru
REPLACE INTO users (username, email) VALUES ('budi', 'another@email.com');
-- created_at sekarang '2026-03-31 14:30:00'
```

### 9.7 INSERT IGNORE (ON CONFLICT)

Handle duplicate key errors gracefully:

```sql
-- Syntax
INSERT OR IGNORE INTO table_name VALUES (...);
INSERT OR REPLACE INTO table_name VALUES (...);
INSERT OR ABORT INTO table_name VALUES (...);  -- Default
INSERT OR FAIL INTO table_name VALUES (...);
INSERT OR ROLLBACK INTO table_name VALUES (...);

-- Contoh praktis
CREATE TABLE users (
    id INTEGER PRIMARY KEY,
    username TEXT UNIQUE,
    email TEXT
);

-- Insert normal
INSERT INTO users (username, email) VALUES ('budi', 'budi@email.com');

-- ❌ Error: UNIQUE constraint failed
INSERT INTO users (username, email) VALUES ('budi', 'another@email.com');

-- ✅ IGNORE: tidak error, skip insert
INSERT OR IGNORE INTO users (username, email) VALUES ('budi', 'another@email.com');

-- ✅ REPLACE: replace existing row
INSERT OR REPLACE INTO users (username, email) VALUES ('budi', 'new@email.com');
```

---

## 10. Filtering dan Kondisi WHERE

**WHERE clause** digunakan untuk filter rows berdasarkan kondisi tertentu.

### 10.1 WHERE: Syntax Dasar

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

### 10.2 Comparison Operators

| Operator   | Description           | Example                  |
| ---------- | --------------------- | ------------------------ |
| =          | Equal                 | WHERE age = 25           |
| != atau <> | Not equal             | WHERE status != 'active' |
| >          | Greater than          | WHERE price > 1000       |
| <          | Less than             | WHERE stock < 10         |
| >=         | Greater than or equal | WHERE year >= 2020       |
| <=         | Less than or equal    | WHERE discount <= 50     |

**Examples:**

```sql
-- Setup data
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT,
    category TEXT,
    price REAL,
    stock INTEGER,
    rating REAL
);

INSERT INTO products VALUES
(1, 'Laptop', 'Electronics', 5000000, 10, 4.5),
(2, 'Mouse', 'Electronics', 150000, 50, 4.2),
(3, 'Desk Chair', 'Furniture', 1200000, 15, 4.7),
(4, 'Monitor', 'Electronics', 2000000, 20, 4.6),
(5, 'Webcam', 'Electronics', 800000, 5, 4.0);

-- Equal
SELECT * FROM products WHERE category = 'Electronics';

-- Not equal
SELECT * FROM products WHERE category != 'Electronics';
SELECT * FROM products WHERE category <> 'Electronics';  -- Same as !=

-- Greater than
SELECT name, price FROM products WHERE price > 1000000;

-- Less than or equal
SELECT name, stock FROM products WHERE stock <= 10;

-- Range
SELECT name, rating FROM products WHERE rating >= 4.5;
```

### 10.3 Logical Operators

| Operator | Description                  | Example                                |
| -------- | ---------------------------- | -------------------------------------- |
| AND      | Both conditions must be true | WHERE price > 100 AND stock > 0        |
| OR       | At least one condition true  | WHERE category = 'A' OR category = 'B' |
| NOT      | Negates a condition          | WHERE NOT status = 'deleted'           |

**Examples:**

```sql
-- AND: kedua kondisi harus TRUE
SELECT * FROM products
WHERE category = 'Electronics' AND price < 1000000;

-- OR: salah satu kondisi TRUE
SELECT * FROM products
WHERE price > 2000000 OR stock < 10;

-- NOT: negate kondisi
SELECT * FROM products
WHERE NOT category = 'Electronics';

-- Kombinasi dengan parentheses untuk precedence
SELECT * FROM products
WHERE (category = 'Electronics' OR category = 'Furniture')
  AND price > 1000000;

-- Complex condition
SELECT * FROM products
WHERE category = 'Electronics'
  AND (price > 2000000 OR stock < 10)
  AND rating >= 4.5;
```

**Operator Precedence (urutan evaluasi):**

```
1. Parentheses ()
2. NOT
3. AND
4. OR
```

**Example:**

```sql
-- ❌ Ambiguous (OR evaluated last)
SELECT * FROM products
WHERE category = 'Electronics' AND price > 1000000 OR stock < 10;
-- (category = 'Electronics' AND price > 1000000) OR (stock < 10)

-- ✅ Clear dengan parentheses
SELECT * FROM products
WHERE category = 'Electronics' AND (price > 1000000 OR stock < 10);
```

### 10.4 BETWEEN Operator

**BETWEEN** untuk range values (inclusive).

```sql
-- Syntax
WHERE column BETWEEN value1 AND value2

-- Equivalent to:
WHERE column >= value1 AND column <= value2
```

**Examples:**

```sql
-- Price range
SELECT * FROM products
WHERE price BETWEEN 500000 AND 2000000;

-- Year range
SELECT * FROM books
WHERE published_year BETWEEN 2000 AND 2020;

-- NOT BETWEEN
SELECT * FROM products
WHERE price NOT BETWEEN 1000000 AND 3000000;

-- Dates
SELECT * FROM orders
WHERE order_date BETWEEN '2026-01-01' AND '2026-03-31';
```

### 10.5 IN Operator

**IN** untuk match multiple values.

```sql
-- Syntax
WHERE column IN (value1, value2, value3, ...)
```

**Examples:**

```sql
-- Category IN list
SELECT * FROM products
WHERE category IN ('Electronics', 'Furniture', 'Gaming');

-- Equivalent to OR:
SELECT * FROM products
WHERE category = 'Electronics'
   OR category = 'Furniture'
   OR category = 'Gaming';

-- Rating IN specific values
SELECT * FROM products
WHERE rating IN (4.5, 4.7, 5.0);

-- NOT IN
SELECT * FROM products
WHERE category NOT IN ('Electronics', 'Furniture');

-- IN dengan subquery
SELECT * FROM products
WHERE category IN (SELECT name FROM categories WHERE is_active = 1);
```

### 10.6 LIKE Operator (Pattern Matching)

**LIKE** untuk pattern matching dengan wildcards.

**Wildcards:**

| Wildcard | Description              | Example              |
| -------- | ------------------------ | -------------------- |
| %        | Match zero or more chars | 'A%' = starts with A |
| \_       | Match exactly one char   | 'A\_' = A + 1 char   |

**Examples:**

```sql
-- Starts with
SELECT * FROM products WHERE name LIKE 'Lap%';
-- Matches: 'Laptop', 'Laptop Stand', 'Lapis'

-- Ends with
SELECT * FROM products WHERE name LIKE '%Chair';
-- Matches: 'Desk Chair', 'Office Chair', 'Chair'

-- Contains
SELECT * FROM products WHERE name LIKE '%top%';
-- Matches: 'Laptop', 'Desktop', 'Laptop Bag'

-- Exact length with underscore
SELECT * FROM users WHERE phone LIKE '08__________';
-- Matches phone numbers starting with 08 dan exactly 12 digits

-- Kombinasi
SELECT * FROM products WHERE name LIKE 'M____';
-- Matches: 'Mouse' (M + 4 chars)

-- NOT LIKE
SELECT * FROM products WHERE name NOT LIKE '%Lap%';

-- Case insensitive (SQLite default)
SELECT * FROM products WHERE name LIKE 'laptop';  -- Matches 'Laptop', 'LAPTOP'
```

**⚠️ LIKE Performance:**

```sql
-- ✅ Fast: pattern starts with known string
WHERE name LIKE 'Laptop%'  -- Uses index

-- ❌ Slow: pattern starts with wildcard
WHERE name LIKE '%Laptop%'  -- Cannot use index (full table scan)
```

### 10.7 GLOB Operator

**GLOB** seperti LIKE, tapi case-sensitive dan Unix-style wildcards.

**Wildcards:**

| Wildcard | Description                |
| -------- | -------------------------- |
| \*       | Match zero or more chars   |
| ?        | Match exactly one char     |
| [abc]    | Match any char in brackets |
| [a-z]    | Match any char in range    |

**Examples:**

```sql
-- GLOB is case-sensitive
SELECT * FROM products WHERE name GLOB 'Laptop*';  -- Matches 'Laptop', not 'laptop'

-- Character class
SELECT * FROM products WHERE code GLOB '[ABC]*';   -- Starts with A, B, or C

-- Range
SELECT * FROM products WHERE code GLOB '[A-Z][0-9]*';  -- Letter + digit + anything

-- NOT GLOB
SELECT * FROM products WHERE name NOT GLOB '*Chair';
```

### 10.8 NULL Handling

**NULL** memerlukan special operators.

```sql
-- ❌ WRONG: tidak akan work
WHERE column = NULL
WHERE column != NULL

-- ✅ CORRECT: use IS NULL / IS NOT NULL
WHERE column IS NULL
WHERE column IS NOT NULL
```

**Examples:**

```sql
-- Find rows with NULL values
SELECT * FROM products WHERE description IS NULL;

-- Find rows with non-NULL values
SELECT * FROM products WHERE description IS NOT NULL;

-- NULL in conditions
SELECT * FROM products
WHERE description IS NOT NULL AND price > 1000000;

-- COALESCE untuk handle NULL
SELECT name, COALESCE(description, 'No description') as desc
FROM products;
```

### 10.9 Complex WHERE Examples

**Example 1: E-commerce product search**

```sql
SELECT * FROM products
WHERE category IN ('Electronics', 'Gaming')
  AND price BETWEEN 500000 AND 3000000
  AND stock > 0
  AND rating >= 4.0
  AND (name LIKE '%Laptop%' OR name LIKE '%PC%')
ORDER BY price;
```

**Example 2: Student filtering**

```sql
SELECT * FROM students
WHERE (jurusan = 'Informatika' OR jurusan = 'Sistem Informasi')
  AND angkatan >= 2020
  AND (ipk >= 3.5 OR prestasi_count > 5)
  AND status = 'active'
ORDER BY ipk DESC;
```

**Example 3: Order filtering**

```sql
SELECT * FROM orders
WHERE order_date BETWEEN '2026-01-01' AND '2026-03-31'
  AND status NOT IN ('cancelled', 'refunded')
  AND (total > 1000000 OR customer_type = 'premium')
  AND shipping_city LIKE 'Jakarta%'
ORDER BY order_date DESC;
```

### 10.10 WHERE vs HAVING

**WHERE** filters rows **before** aggregation.
**HAVING** filters groups **after** aggregation.

```sql
-- WHERE: filter individual rows
SELECT category, COUNT(*) as total
FROM products
WHERE price > 1000000  -- Filter rows first
GROUP BY category;

-- HAVING: filter aggregated results
SELECT category, COUNT(*) as total
FROM products
GROUP BY category
HAVING COUNT(*) > 2;   -- Filter groups after counting

-- Kombinasi WHERE dan HAVING
SELECT category, AVG(price) as avg_price
FROM products
WHERE stock > 0                     -- Filter individual products
GROUP BY category
HAVING AVG(price) > 1000000;        -- Filter categories by average
```

---

(Tutorial akan dilanjutkan dengan sections berikutnya...)

## 11. Sorting dan Limiting Data

### 11.1 ORDER BY - Mengurutkan Data

**ORDER BY** mengurutkan hasil query berdasarkan satu atau lebih columns.

#### Syntax Dasar

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC];
```

- **ASC** = Ascending (default) - dari kecil ke besar, A-Z
- **DESC** = Descending - dari besar ke kecil, Z-A

#### Single Column Sorting

```sql
-- Sort by price (ascending)
SELECT * FROM products ORDER BY price;
SELECT * FROM products ORDER BY price ASC;  -- Same (ASC is default)

-- Sort by price (descending)
SELECT * FROM products ORDER BY price DESC;

-- Sort by name (alphabetically)
SELECT * FROM products ORDER BY name;
```

#### Multiple Column Sorting

```sql
-- Sort by category, then by price
SELECT name, category, price
FROM products
ORDER BY category ASC, price DESC;
-- First: group by category (A-Z)
-- Within each category: sort by price (high to low)

-- Three-level sorting
SELECT * FROM students
ORDER BY angkatan DESC, jurusan ASC, nama ASC;
```

#### Sort by Column Position

```sql
-- Sort by column number (1-based)
SELECT name, price, stock FROM products
ORDER BY 2;  -- Sort by price (column 2)

-- ⚠️ Not recommended (less readable)
-- Prefer column names for clarity
```

#### Sort by Calculated Column

```sql
-- Sort by calculated value
SELECT
    name,
    price,
    stock,
    price * stock as inventory_value
FROM products
ORDER BY inventory_value DESC;

-- Can also use expression directly
SELECT name, price, stock
FROM products
ORDER BY price * stock DESC;
```

#### NULL Handling in ORDER BY

```sql
-- NULLs appear first in ASC, last in DESC
SELECT * FROM products ORDER BY description ASC;
-- NULL values first, then A-Z

SELECT * FROM products ORDER BY description DESC;
-- Z-A first, then NULL values last
```

### 11.2 LIMIT - Membatasi Hasil

**LIMIT** membatasi jumlah rows yang dikembalikan.

#### Syntax Dasar

```sql
SELECT column1, column2, ...
FROM table_name
LIMIT number;
```

#### Basic LIMIT

```sql
-- Get top 5 products
SELECT * FROM products LIMIT 5;

-- Get top 10 most expensive products
SELECT * FROM products
ORDER BY price DESC
LIMIT 10;

-- Get 3 newest orders
SELECT * FROM orders
ORDER BY order_date DESC
LIMIT 3;
```

#### LIMIT with OFFSET (Pagination)

```sql
-- Syntax
SELECT * FROM table_name
LIMIT number OFFSET skip_number;

-- Alternative syntax
SELECT * FROM table_name
LIMIT skip_number, number;  -- SQLite/MySQL style
```

**Pagination Examples:**

```sql
-- Page 1 (first 10 items)
SELECT * FROM products
ORDER BY id
LIMIT 10 OFFSET 0;

-- Page 2 (items 11-20)
SELECT * FROM products
ORDER BY id
LIMIT 10 OFFSET 10;

-- Page 3 (items 21-30)
SELECT * FROM products
ORDER BY id
LIMIT 10 OFFSET 20;

-- Formula: OFFSET = (page_number - 1) * page_size
```

**Alternative Syntax:**

```sql
-- SQLite/MySQL style
SELECT * FROM products LIMIT 0, 10;   -- Page 1
SELECT * FROM products LIMIT 10, 10;  -- Page 2
SELECT * FROM products LIMIT 20, 10;  -- Page 3
```

### 11.3 Sorting dan Limiting Examples

**Example 1: Top 10 Leaderboard**

```sql
-- Top 10 students by GPA
SELECT
    rank() OVER (ORDER BY ipk DESC) as ranking,
    nama,
    nim,
    ipk
FROM students
WHERE status = 'active'
ORDER BY ipk DESC
LIMIT 10;
```

**Example 2: Latest Products**

```sql
-- 5 newest products
SELECT
    name,
    price,
    created_at
FROM products
ORDER BY created_at DESC
LIMIT 5;
```

**Example 3: Price Range with Limit**

```sql
-- Top 5 expensive products under 2 million
SELECT name, price
FROM products
WHERE price < 2000000
ORDER BY price DESC
LIMIT 5;
```

**Example 4: Random Selection**

```sql
-- Get 10 random products
SELECT * FROM products
ORDER BY RANDOM()
LIMIT 10;
```

---

## 12. Fungsi Agregasi

**Fungsi agregasi** melakukan kalkulasi pada multiple rows dan return single value.

### 12.1 COUNT - Menghitung Rows

**COUNT** menghitung jumlah rows.

```sql
-- Count all rows
SELECT COUNT(*) FROM products;

-- Count non-NULL values
SELECT COUNT(description) FROM products;

-- Count with condition
SELECT COUNT(*) FROM products WHERE price > 1000000;

-- Count DISTINCT values
SELECT COUNT(DISTINCT category) as total_categories FROM products;
```

**Examples:**

```sql
-- Total products per category
SELECT category, COUNT(*) as total
FROM products
GROUP BY category;

-- Students per major
SELECT jurusan, COUNT(*) as jumlah_mahasiswa
FROM students
GROUP BY jurusan;
```

### 12.2 SUM - Total Penjumlahan

**SUM** menjumlahkan nilai numeric.

```sql
-- Total inventory value
SELECT SUM(price * stock) as total_inventory_value
FROM products;

-- Total sales
SELECT SUM(amount) as total_sales
FROM orders
WHERE order_date >= '2026-01-01';

-- Sum per category
SELECT
    category,
    SUM(stock) as total_stock,
    SUM(price * stock) as total_value
FROM products
GROUP BY category;
```

### 12.3 AVG - Rata-rata

**AVG** menghitung average (mean).

```sql
-- Average price
SELECT AVG(price) as avg_price FROM products;

-- Average with rounding
SELECT ROUND(AVG(price), 2) as avg_price FROM products;

-- Average per category
SELECT
    category,
    AVG(price) as avg_price,
    AVG(rating) as avg_rating
FROM products
GROUP BY category;

-- Average GPA per major
SELECT
    jurusan,
    ROUND(AVG(ipk), 2) as rata_rata_ipk,
    COUNT(*) as jumlah_mahasiswa
FROM students
GROUP BY jurusan;
```

### 12.4 MIN dan MAX - Nilai Terkecil dan Terbesar

**MIN** dan **MAX** return nilai minimum dan maximum.

```sql
-- Min and max price
SELECT
    MIN(price) as cheapest,
    MAX(price) as most_expensive
FROM products;

-- Min/max per category
SELECT
    category,
    MIN(price) as min_price,
    MAX(price) as max_price,
    MAX(price) - MIN(price) as price_range
FROM products
GROUP BY category;

-- Get product with min/max price
SELECT * FROM products WHERE price = (SELECT MIN(price) FROM products);
SELECT * FROM products WHERE price = (SELECT MAX(price) FROM products);
```

### 12.5 GROUP BY - Mengelompokkan Data

**GROUP BY** mengelompokkan rows berdasarkan column values, digunakan dengan aggregate functions.

#### Syntax

```sql
SELECT column, aggregate_function(column)
FROM table_name
WHERE condition
GROUP BY column
HAVING group_condition
ORDER BY column;
```

#### Basic GROUP BY

```sql
-- Count products per category
SELECT
    category,
    COUNT(*) as total_products
FROM products
GROUP BY category;

-- Sales per month
SELECT
    strftime('%Y-%m', order_date) as month,
    COUNT(*) as total_orders,
    SUM(amount) as total_sales
FROM orders
GROUP BY strftime('%Y-%m', order_date)
ORDER BY month;
```

#### Multiple Columns GROUP BY

```sql
-- Products per category and price range
SELECT
    category,
    CASE
        WHEN price < 1000000 THEN 'Budget'
        WHEN price < 3000000 THEN 'Mid-range'
        ELSE 'Premium'
    END as price_range,
    COUNT(*) as total,
    AVG(price) as avg_price
FROM products
GROUP BY category, price_range
ORDER BY category, price_range;
```

#### GROUP BY with Aggregate Functions

```sql
-- Comprehensive statistics per category
SELECT
    category,
    COUNT(*) as total_products,
    SUM(stock) as total_stock,
    AVG(price) as avg_price,
    MIN(price) as min_price,
    MAX(price) as max_price,
    SUM(price * stock) as total_inventory_value
FROM products
GROUP BY category
ORDER BY total_inventory_value DESC;
```

### 12.6 HAVING - Filter Setelah GROUP BY

**HAVING** filter groups setelah agregasi (berbeda dengan WHERE yang filter sebelum agregasi).

```sql
-- Categories with more than 5 products
SELECT
    category,
    COUNT(*) as total
FROM products
GROUP BY category
HAVING COUNT(*) > 5;

-- Categories with average price > 1 million
SELECT
    category,
    AVG(price) as avg_price,
    COUNT(*) as total_products
FROM products
GROUP BY category
HAVING AVG(price) > 1000000;

-- Kombinasi WHERE dan HAVING
SELECT
    category,
    AVG(price) as avg_price
FROM products
WHERE stock > 0                    -- Filter: only in-stock products
GROUP BY category
HAVING AVG(price) > 1000000       -- Filter: categories with high avg price
ORDER BY avg_price DESC;
```

### 12.7 Aggregate Functions Examples

**Example 1: Sales Report**

```sql
SELECT
    strftime('%Y-%m', order_date) as month,
    COUNT(*) as total_orders,
    SUM(amount) as revenue,
    AVG(amount) as avg_order_value,
    MIN(amount) as min_order,
    MAX(amount) as max_order
FROM orders
WHERE order_date >= '2026-01-01'
GROUP BY strftime('%Y-%m', order_date)
ORDER BY month;
```

**Example 2: Student Statistics**

```sql
SELECT
    jurusan,
    angkatan,
    COUNT(*) as jumlah_mahasiswa,
    ROUND(AVG(ipk), 2) as rata_rata_ipk,
    MIN(ipk) as ipk_terendah,
    MAX(ipk) as ipk_tertinggi,
    COUNT(CASE WHEN ipk >= 3.5 THEN 1 END) as mahasiswa_cumlaude
FROM students
WHERE status = 'active'
GROUP BY jurusan, angkatan
HAVING COUNT(*) >= 10  -- Only show groups with 10+ students
ORDER BY jurusan, angkatan DESC;
```

**Example 3: Product Performance**

```sql
SELECT
    category,
    COUNT(*) as total_products,
    SUM(stock) as total_stock,
    ROUND(AVG(rating), 2) as avg_rating,
    COUNT(CASE WHEN rating >= 4.5 THEN 1 END) as highly_rated,
    ROUND(COUNT(CASE WHEN rating >= 4.5 THEN 1 END) * 100.0 / COUNT(*), 1) as percent_highly_rated
FROM products
GROUP BY category
ORDER BY percent_highly_rated DESC;
```

---

## 13. JOIN Operations

**JOIN** menggabungkan rows dari dua atau lebih tabel berdasarkan relasi column.

### 13.1 Konsep JOIN

**Mengapa perlu JOIN?**

Data di-normalize ke multiple tables untuk menghindari redundancy. JOIN memungkinkan kita query data dari multiple tables sekaligus.

**Contoh:**

```
Tanpa JOIN (redundant):              Dengan JOIN (normalized):
orders table:                        orders table:
┌────┬────────────┬─────────┐        ┌────┬────────────┬─────────┐
│ id │customer    │ amount  │        │ id │customer_id │ amount  │
├────┼────────────┼─────────┤        ├────┼────────────┼─────────┤
│ 1  │Budi        │ 100000  │        │ 1  │ 1          │ 100000  │
│ 2  │Budi        │ 150000  │        │ 2  │ 1          │ 150000  │
└────┴────────────┴─────────┘        │ 3  │ 2          │ 200000  │
(nama customer berulang)             └────┴────────────┴─────────┘
                                            ↓
                                     customers table:
                                     ┌────┬──────┬────────────┐
                                     │ id │ nama │ email      │
                                     ├────┼──────┼────────────┤
                                     │ 1  │ Budi │budi@e.com  │
                                     │ 2  │ Siti │siti@e.com  │
                                     └────┴──────┴────────────┘
```

### 13.2 INNER JOIN

**INNER JOIN** return rows yang memiliki match di both tables.

#### Setup Data untuk Contoh

```sql
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    city TEXT
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    product TEXT,
    amount REAL,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

INSERT INTO customers VALUES
(1, 'Budi', 'Jakarta'),
(2, 'Siti', 'Bandung'),
(3, 'Ahmad', 'Surabaya'),
(4, 'Dewi', 'Jakarta');  -- No orders

INSERT INTO orders VALUES
(1, 1, 'Laptop', 5000000),
(2, 1, 'Mouse', 150000),
(3, 2, 'Keyboard', 300000);
```

#### INNER JOIN Example

```sql
SELECT
    c.name,
    o.product,
    o.amount
FROM customers c
INNER JOIN orders o ON c.id = o.customer_id;
```

### 13.3 LEFT JOIN

**LEFT JOIN** return semua rows dari left table, dengan matched rows dari right table.

```sql
-- Get all customers, even without orders
SELECT
    c.name,
    o.product,
    o.amount
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id;
-- Dewi akan muncul dengan NULL untuk product dan amount
```

### 13.4 Multiple JOINs

```sql
SELECT
    c.name as customer,
    o.product,
    p.category,
    o.amount
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id
INNER JOIN products p ON o.product = p.name;
```

---

## 14. Relationships dalam Database

**Relationships** mendefinisikan bagaimana data di satu tabel relates ke data di tabel lain.

### 14.1 One-to-Many (1:N)

**Most common!** Satu row di table A relates to many rows di table B.

```sql
CREATE TABLE authors (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    author_id INTEGER,
    FOREIGN KEY (author_id) REFERENCES authors(id)
);
-- One author → many books
```

### 14.2 Many-to-Many (M:N)

**Requires junction table!**

```sql
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE courses (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
);

-- Junction table
CREATE TABLE enrollments (
    student_id INTEGER,
    course_id INTEGER,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

---

## 15. Normalisasi Database

**Normalisasi** organize database structure untuk reduce redundancy.

### 15.1 First Normal Form (1NF)

- Each column contains **atomic values** (no arrays)
- Each column contains values of **single type**

### 15.2 Second Normal Form (2NF)

- Must be in **1NF**
- All non-key attributes depend on **entire primary key**

### 15.3 Third Normal Form (3NF)

- Must be in **2NF**
- No **transitive dependencies**

**Example:**

```sql
-- ❌ Not normalized
CREATE TABLE orders_bad (
    id INTEGER,
    customer_name TEXT,  -- Repeated
    customer_email TEXT, -- Repeated
    product TEXT
);

-- ✅ Normalized
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    name TEXT,
    email TEXT
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    product TEXT,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

---

## 16. Constraints dan Validasi Data

```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL CHECK(LENGTH(name) >= 2),
    price REAL NOT NULL CHECK(price > 0),
    stock INTEGER DEFAULT 0 CHECK(stock >= 0),
    status TEXT DEFAULT 'active' CHECK(status IN ('active', 'inactive'))
);
```

---

## 17. Indexes untuk Optimasi

```sql
-- Create index untuk faster queries
CREATE INDEX idx_products_name ON products(name);
CREATE INDEX idx_orders_date ON orders(order_date);

-- Composite index
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);
```

---

## 18. Transactions dan ACID

```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;  -- or ROLLBACK if error
```

---

## 19. Project: Sistem Manajemen Perpustakaan

Mari kita buat complete library management system!

### 19.1 Database Schema

```sql
PRAGMA foreign_keys = ON;

-- Table: authors
CREATE TABLE authors (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    country TEXT,
    birth_year INTEGER CHECK(birth_year > 1800 AND birth_year < 2100)
);

-- Table: books
CREATE TABLE books (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
    isbn TEXT UNIQUE,
    published_year INTEGER,
    copies_total INTEGER DEFAULT 1 CHECK(copies_total > 0),
    copies_available INTEGER DEFAULT 1,
    CHECK(copies_available <= copies_total AND copies_available >= 0)
);

-- Table: book_authors (many-to-many)
CREATE TABLE book_authors (
    book_id INTEGER,
    author_id INTEGER,
    author_order INTEGER DEFAULT 1,
    PRIMARY KEY (book_id, author_id),
    FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE,
    FOREIGN KEY (author_id) REFERENCES authors(id) ON DELETE CASCADE
);

-- Table: members
CREATE TABLE members (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL CHECK(email LIKE '%@%.%'),
    phone TEXT,
    join_date TEXT DEFAULT (date('now')),
    status TEXT DEFAULT 'active' CHECK(status IN ('active', 'suspended', 'inactive'))
);

-- Table: loans
CREATE TABLE loans (
    id INTEGER PRIMARY KEY,
    book_id INTEGER NOT NULL,
    member_id INTEGER NOT NULL,
    loan_date TEXT DEFAULT (date('now')),
    due_date TEXT NOT NULL,
    return_date TEXT,
    fine_amount REAL DEFAULT 0 CHECK(fine_amount >= 0),
    FOREIGN KEY (book_id) REFERENCES books(id),
    FOREIGN KEY (member_id) REFERENCES members(id),
    CHECK(due_date > loan_date)
);

-- Indexes untuk performance
CREATE INDEX idx_books_title ON books(title);
CREATE INDEX idx_loans_member ON loans(member_id);
CREATE INDEX idx_loans_book ON loans(book_id);
CREATE INDEX idx_loans_return ON loans(return_date);
```

### 19.2 Sample Data

```sql
-- Insert authors
INSERT INTO authors (name, country, birth_year) VALUES
('Robert C. Martin', 'USA', 1952),
('Martin Fowler', 'UK', 1963),
('Gang of Four', 'Various', 1960),
('Joshua Bloch', 'USA', 1961);

-- Insert books
INSERT INTO books (title, isbn, published_year, copies_total, copies_available) VALUES
('Clean Code', '978-0132350884', 2008, 3, 2),
('Refactoring', '978-0201485677', 1999, 2, 1),
('Design Patterns', '978-0201633610', 1994, 2, 2),
('Effective Java', '978-0134685991', 2017, 2, 2);

-- Link books with authors
INSERT INTO book_authors (book_id, author_id, author_order) VALUES
(1, 1, 1),  -- Clean Code → Robert Martin
(2, 2, 1),  -- Refactoring → Martin Fowler
(3, 3, 1),  -- Design Patterns → Gang of Four
(4, 4, 1);  -- Effective Java → Joshua Bloch

-- Insert members
INSERT INTO members (name, email, phone) VALUES
('Budi Santoso', 'budi@email.com', '081234567890'),
('Siti Nurhaliza', 'siti@email.com', '081234567891'),
('Ahmad Dahlan', 'ahmad@email.com', '081234567892');

-- Insert loans
INSERT INTO loans (book_id, member_id, loan_date, due_date) VALUES
(1, 1, '2026-03-15', '2026-03-29'),  -- Budi pinjam Clean Code
(2, 2, '2026-03-20', '2026-04-03');  -- Siti pinjam Refactoring
```

### 19.3 Practical Queries

**Query 1: List all books with authors**

```sql
SELECT
    b.title,
    GROUP_CONCAT(a.name, ', ') as authors,
    b.published_year,
    b.copies_available || '/' || b.copies_total as availability
FROM books b
LEFT JOIN book_authors ba ON b.id = ba.book_id
LEFT JOIN authors a ON ba.author_id = a.id
GROUP BY b.id
ORDER BY b.title;
```

**Query 2: Current loans (not yet returned)**

```sql
SELECT
    m.name as member,
    b.title as book,
    l.loan_date,
    l.due_date,
    CASE
        WHEN date('now') > l.due_date THEN 'OVERDUE'
        ELSE 'On Time'
    END as status,
    MAX(0, julianday('now') - julianday(l.due_date)) as days_overdue
FROM loans l
INNER JOIN members m ON l.member_id = m.id
INNER JOIN books b ON l.book_id = b.id
WHERE l.return_date IS NULL
ORDER BY l.due_date;
```

**Query 3: Member borrowing history**

```sql
SELECT
    m.name,
    COUNT(l.id) as total_loans,
    COUNT(CASE WHEN l.return_date IS NULL THEN 1 END) as current_loans,
    COUNT(CASE WHEN date('now') > l.due_date AND l.return_date IS NULL THEN 1 END) as overdue_loans,
    SUM(l.fine_amount) as total_fines
FROM members m
LEFT JOIN loans l ON m.id = l.member_id
GROUP BY m.id
ORDER BY total_loans DESC;
```

**Query 4: Most popular books**

```sql
SELECT
    b.title,
    COUNT(l.id) as times_borrowed,
    COUNT(CASE WHEN l.return_date IS NULL THEN 1 END) as currently_borrowed
FROM books b
LEFT JOIN loans l ON b.id = l.book_id
GROUP BY b.id
ORDER BY times_borrowed DESC
LIMIT 10;
```

### 19.4 Loan Operations

**Borrow a book:**

```sql
BEGIN TRANSACTION;

-- Check availability
SELECT copies_available FROM books WHERE id = 1;
-- If > 0, proceed

-- Create loan record
INSERT INTO loans (book_id, member_id, due_date)
VALUES (1, 3, date('now', '+14 days'));

-- Decrease available copies
UPDATE books
SET copies_available = copies_available - 1
WHERE id = 1;

COMMIT;
```

**Return a book:**

```sql
BEGIN TRANSACTION;

-- Mark as returned
UPDATE loans
SET return_date = date('now'),
    fine_amount = CASE
        WHEN date('now') > due_date
        THEN (julianday(date('now')) - julianday(due_date)) * 5000  -- Rp 5000/day
        ELSE 0
    END
WHERE id = 1 AND return_date IS NULL;

-- Increase available copies
UPDATE books
SET copies_available = copies_available + 1
WHERE id = (SELECT book_id FROM loans WHERE id = 1);

COMMIT;
```

---

## 20. Tools dan GUI untuk SQLite

### 20.1 DB Browser for SQLite (Recommended)

**Free, open-source, cross-platform GUI tool.**

**Features:**

- Visual table browser
- SQL query editor with syntax highlighting
- Import/export CSV, JSON
- Database structure viewer
- Visual table designer

**Download:** [https://sqlitebrowser.org](https://sqlitebrowser.org)

### 20.2 Other Tools

- **SQLite Studio** - Feature-rich, cross-platform
- **DBeaver** - Universal database tool (supports many databases)
- **DataGrip** - JetBrains (paid, professional)
- **VS Code Extensions** - SQLite Viewer, SQLite

### 20.3 Online Tools

- **SQLite Online** - [https://sqliteonline.com](https://sqliteonline.com)
- **SQL Fiddle** - Practice SQL queries online

---

## 21. Best Practices

### 21.1 Naming Conventions

```sql
-- ✅ Good naming
tables: users, products, order_items (plural, snake_case)
columns: user_id, first_name, created_at (snake_case)
indexes: idx_users_email, idx_orders_date (descriptive)

-- ❌ Avoid
tblUsers, UserID, CreateDate (inconsistent)
```

### 21.2 Database Design

1. **Normalize to 3NF** first, denormalize only if needed
2. **Use appropriate data types** (INTEGER for IDs, TEXT for strings)
3. **Always use PRIMARY KEY**
4. **Use FOREIGN KEY constraints** with CASCADE
5. **Add indexes** on frequently queried columns
6. **Use CHECK constraints** for data validation
7. **Use DEFAULT values** when appropriate

### 21.3 Query Optimization

```sql
-- ✅ Use indexes
CREATE INDEX idx_users_email ON users(email);
SELECT * FROM users WHERE email = 'test@example.com';

-- ✅ Limit results
SELECT * FROM products ORDER BY created_at DESC LIMIT 10;

-- ✅ Use specific columns
SELECT id, name, price FROM products;  -- Not SELECT *

-- ✅ Use EXPLAIN to check query plan
EXPLAIN QUERY PLAN SELECT * FROM products WHERE category_id = 5;
```

### 21.4 Security

```sql
-- ✅ Use parameterized queries (in application code)
-- Python example:
cursor.execute("SELECT * FROM users WHERE email = ?", (user_email,))

-- ❌ NEVER concatenate user input
-- cursor.execute(f"SELECT * FROM users WHERE email = '{user_email}'")
-- ^ Vulnerable to SQL injection!
```

### 21.5 Backup

```bash
# Backup database
sqlite3 mydatabase.db ".backup backup.db"

# Or simply copy file
cp mydatabase.db mydatabase_backup.db

# Restore
sqlite3 mydatabase.db ".restore backup.db"
```

---

## 22. Kesimpulan dan Next Steps

**Congratulations!** 🎉 Anda telah menyelesaikan tutorial lengkap Relational Database dengan SQLite.

### 22.1 Apa yang Sudah Dipelajari

✅ Konsep dasar database dan SQL
✅ SQLite installation dan setup
✅ Data types dan table creation
✅ CRUD operations (INSERT, SELECT, UPDATE, DELETE)
✅ Filtering, sorting, dan aggregation
✅ JOIN operations untuk multiple tables
✅ Relationships (one-to-one, one-to-many, many-to-many)
✅ Database normalization (1NF, 2NF, 3NF)
✅ Constraints dan data validation
✅ Indexes untuk performance optimization
✅ Transactions dan ACID properties
✅ Complete project: Library Management System

### 22.2 Next Steps

**1. Practice!**

- Build your own projects (todo app, expense tracker, blog)
- Contribute to open source projects
- Solve database challenges on [HackerRank](https://www.hackerrank.com/domains/sql) atau [LeetCode](https://leetcode.com/problemset/database/)

**2. Learn Advanced Topics**

- Query optimization dan performance tuning
- Database backup dan recovery strategies
- Database security dan authentication
- Full-text search dengan FTS5
- Triggers dan automated operations
- Views untuk simplify complex queries

**3. Explore Other Databases**

- **MySQL** - Most popular for web applications
- **PostgreSQL** - Advanced features, JSON support
- **MongoDB** - NoSQL document database
- **Redis** - In-memory cache database

**4. Learn Database Integration**

- Connect database dengan programming languages:
  - **Python**: sqlite3, SQLAlchemy
  - **Node.js**: better-sqlite3, Sequelize
  - **PHP**: PDO, Laravel Eloquent
  - **Java**: JDBC, Hibernate
  - **C#**: ADO.NET, Entity Framework

**5. Study Database Theory**

- Query optimization algorithms
- B-tree dan indexing internals
- ACID implementation
- CAP theorem
- Distributed databases

### 22.3 Resources untuk Belajar Lebih Lanjut

**Documentation:**

- [SQLite Official Documentation](https://www.sqlite.org/docs.html)
- [SQL Tutorial - W3Schools](https://www.w3schools.com/sql/)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)

**Books:**

- "SQL for Data Analysis" - Cathy Tanimura
- "Designing Data-Intensive Applications" - Martin Kleppmann
- "Database Systems: The Complete Book" - Garcia-Molina, Ullman, Widom

**Online Courses:**

- [SQLBolt](https://sqlbolt.com/) - Interactive SQL lessons
- [Khan Academy: SQL](https://www.khanacademy.org/computing/computer-programming/sql)
- Udemy, Coursera, edX - SQL courses

**Practice Platforms:**

- [HackerRank SQL](https://www.hackerrank.com/domains/sql)
- [LeetCode Database](https://leetcode.com/problemset/database/)
- [SQLZoo](https://sqlzoo.net/)

### 22.4 Penutup

**"Data is the new oil, but if unrefined it cannot really be used."**

Database adalah foundation dari hampir semua aplikasi modern. Menguasai SQL dan database design adalah skill yang akan selalu relevant dan valuable dalam karir programming Anda.

**Key Takeaways:**

1. **Start simple** - SQLite perfect untuk learning
2. **Practice regularly** - Build real projects
3. **Understand theory** - Know why, not just how
4. **Optimize later** - Make it work first, then make it fast
5. **Learn from mistakes** - Database adalah tempat terbaik untuk experiment

**Remember:**

- 💾 Always backup your data
- 🔒 Never store sensitive data unencrypted
- 🚀 Optimize queries, not prematurely
- 📝 Document your schema
- 🧪 Test your queries

**Good luck dengan database journey Anda!** 🚀

Jika ada pertanyaan atau feedback, feel free to contribute atau open issue di repository ini.

---

**Tutorial dibuat dengan ❤️ untuk membantu developer Indonesia menguasai database.**

_Last updated: March 31, 2026_
