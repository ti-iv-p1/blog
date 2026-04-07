# Membuat Database SQLite Web Absensi dengan Node.js

## Pendahuluan

Tutorial ini akan memandu Anda dalam membuat database SQLite untuk sistem absensi mahasiswa menggunakan Node.js dan library `better-sqlite3`. Sistem ini dirancang untuk mengelola data pengguna (admin, dosen, mahasiswa), mata kuliah, kelas, dan catatan absensi.

## Daftar Isi

1. [Persiapan Project](#persiapan-project)
2. [Instalasi Dependencies](#instalasi-dependencies)
3. [Struktur Database](#struktur-database)
4. [Penjelasan Kode](#penjelasan-kode)
5. [Menjalankan Aplikasi](#menjalankan-aplikasi)

## Persiapan Project

Sebelum memulai, pastikan Anda telah menginstall Node.js di komputer Anda. Anda dapat mengecek dengan menjalankan:

```bash
node --version
npm --version
```

## Instalasi Dependencies

### 1. Inisialisasi Project

Buat folder project dan inisialisasi npm:

```bash
mkdir web-absensi
cd web-absensi
npm init -y
```

### 2. Install Dependencies

Install library yang dibutuhkan:

```bash
npm install better-sqlite3 express
npm install --save-dev nodemon
```

**Penjelasan Dependencies:**

- **better-sqlite3**: Library untuk berinteraksi dengan database SQLite. Dipilih karena performanya yang sangat cepat dan API yang sederhana dibanding library SQLite lainnya.
- **express**: Framework web untuk Node.js (opsional untuk development selanjutnya).
- **nodemon**: Tool development yang akan restart aplikasi secara otomatis ketika ada perubahan file.

### 3. Konfigurasi package.json

File `package.json` Anda akan terlihat seperti ini:

```json
{
  "name": "web-absensi",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "commonjs",
  "dependencies": {
    "better-sqlite3": "^12.8.0",
    "express": "^5.2.1"
  },
  "devDependencies": {
    "nodemon": "^3.1.14"
  }
}
```

**Penjelasan Konfigurasi:**

- `"type": "commonjs"`: Menentukan bahwa kita menggunakan sistem module CommonJS (require/module.exports).
- `"scripts"`: Berisi perintah yang dapat dijalankan dengan `npm run`.
- `"dev": "nodemon index.js"`: Script untuk menjalankan aplikasi dalam mode development.

## Struktur Database

Database ini terdiri dari 8 tabel utama yang saling berelasi:

### Diagram Relasi

```
pengguna (users)
├── mahasiswa (students)
├── dosen (lecturers)

mata_kuliah (courses)

kelas (classes)
├── Foreign Key: mata_kuliah_id
├── Foreign Key: dosen_id

peserta_kelas (class participants)
├── Foreign Key: mahasiswa_id
├── Foreign Key: kelas_id

sesi_absensi (attendance sessions)
├── Foreign Key: kelas_id

absensi (attendance records)
├── Foreign Key: sesi_id
├── Foreign Key: mahasiswa_id
```

### Penjelasan Tabel

1. **pengguna**: Menyimpan data semua pengguna sistem (admin, dosen, mahasiswa)
2. **mahasiswa**: Data khusus mahasiswa (NIM, program studi, dll)
3. **dosen**: Data khusus dosen (NIDN, departemen)
4. **mata_kuliah**: Data mata kuliah yang tersedia
5. **kelas**: Data kelas perkuliahan
6. **peserta_kelas**: Relasi many-to-many antara mahasiswa dan kelas
7. **sesi_absensi**: Sesi pertemuan untuk setiap kelas
8. **absensi**: Record absensi mahasiswa per sesi

## Penjelasan Kode

Berikut adalah penjelasan lengkap untuk setiap bagian kode dalam file `index.js`:

### 1. Import Library

```javascript
const Database = require("better-sqlite3");
```

**Penjelasan:**

- Mengimport library `better-sqlite3` yang akan digunakan untuk berinteraksi dengan database SQLite.
- Menggunakan sintaks `require()` karena kita menggunakan CommonJS module system.

### 2. Inisialisasi Database

```javascript
const db = new Database("absensi.db", { verbose: console.log });
```

**Penjelasan:**

- **`new Database('absensi.db')`**: Membuat koneksi ke database SQLite. Jika file `absensi.db` belum ada, akan dibuat otomatis.
- **`{ verbose: console.log }`**: Opsi untuk menampilkan setiap query SQL yang dijalankan ke console. Berguna untuk debugging dan monitoring.

### 3. Mengaktifkan Foreign Keys

```javascript
db.pragma("foreign_keys = ON");
```

**Penjelasan:**

- SQLite secara default tidak mengaktifkan foreign key constraints.
- Perintah `PRAGMA foreign_keys = ON` mengaktifkan fitur foreign key untuk menjaga integritas referensial data.
- Dengan ini aktif, Anda tidak bisa menghapus record yang masih direferensi oleh tabel lain (kecuali menggunakan CASCADE).

### 4. Membuat Tabel

#### Tabel Pengguna

```javascript
CREATE TABLE IF NOT EXISTS pengguna (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  nama TEXT NOT NULL,
  email TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  peran TEXT NOT NULL CHECK(peran IN ('admin', 'dosen', 'mahasiswa')),
  dibuat_pada DATETIME DEFAULT CURRENT_TIMESTAMP,
  diperbarui_pada DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

**Penjelasan Detail:**

- **`id INTEGER PRIMARY KEY AUTOINCREMENT`**:
  - `PRIMARY KEY`: Kolom unik yang mengidentifikasi setiap record
  - `AUTOINCREMENT`: Nilai akan bertambah otomatis untuk setiap record baru
- **`nama TEXT NOT NULL`**:
  - Menyimpan nama pengguna
  - `NOT NULL`: Field wajib diisi
- **`email TEXT UNIQUE NOT NULL`**:
  - Email pengguna harus unik (tidak boleh duplikat)
  - Berfungsi sebagai username untuk login
- **`password TEXT NOT NULL`**:
  - Menyimpan password (dalam implementasi production harus di-hash menggunakan bcrypt atau argon2)
- **`peran TEXT NOT NULL CHECK(peran IN ('admin', 'dosen', 'mahasiswa'))`**:
  - Constraint `CHECK` memastikan nilai peran hanya bisa salah satu dari: 'admin', 'dosen', atau 'mahasiswa'
  - Mencegah input nilai yang tidak valid
- **`dibuat_pada DATETIME DEFAULT CURRENT_TIMESTAMP`**:
  - Otomatis menyimpan waktu pembuatan record
  - `DEFAULT CURRENT_TIMESTAMP`: Mengisi otomatis dengan waktu saat ini
- **`diperbarui_pada DATETIME DEFAULT CURRENT_TIMESTAMP`**:
  - Menyimpan waktu terakhir record diupdate

#### Tabel Mahasiswa

```javascript
CREATE TABLE IF NOT EXISTS mahasiswa (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id INTEGER NOT NULL,
  nim TEXT UNIQUE NOT NULL,
  program_studi TEXT,
  angkatan INTEGER,
  FOREIGN KEY (pengguna_id) REFERENCES pengguna(id) ON DELETE CASCADE
);
```

**Penjelasan Detail:**

- **`pengguna_id INTEGER NOT NULL`**:
  - Kolom yang mereferensikan ke tabel `pengguna`
  - Setiap mahasiswa harus memiliki akun pengguna
- **`nim TEXT UNIQUE NOT NULL`**:
  - Nomor Induk Mahasiswa harus unik
  - Menggunakan TEXT karena NIM bisa mengandung leading zeros (misal: 0123456)
- **`program_studi TEXT`**:
  - Nama program studi mahasiswa (Informatika, Sistem Informasi, dll)
- **`angkatan INTEGER`**:
  - Tahun angkatan mahasiswa (2020, 2021, dll)
- **`FOREIGN KEY (pengguna_id) REFERENCES pengguna(id) ON DELETE CASCADE`**:
  - Membuat relasi ke tabel `pengguna`
  - `ON DELETE CASCADE`: Jika pengguna dihapus, data mahasiswa juga ikut terhapus otomatis

#### Tabel Dosen

```javascript
CREATE TABLE IF NOT EXISTS dosen (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id INTEGER NOT NULL,
  nidn TEXT UNIQUE NOT NULL,
  departemen TEXT,
  FOREIGN KEY (pengguna_id) REFERENCES pengguna(id) ON DELETE CASCADE
);
```

**Penjelasan Detail:**

- **`nidn TEXT UNIQUE NOT NULL`**:
  - Nomor Induk Dosen Nasional harus unik
  - Identifikasi resmi dosen di Indonesia
- **`departemen TEXT`**:
  - Departemen/jurusan tempat dosen mengajar
- Struktur mirip dengan tabel mahasiswa, dengan relasi one-to-one ke tabel pengguna

#### Tabel Mata Kuliah

```javascript
CREATE TABLE IF NOT EXISTS mata_kuliah (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  kode TEXT UNIQUE NOT NULL,
  nama TEXT NOT NULL,
  sks INTEGER NOT NULL
);
```

**Penjelasan Detail:**

- **`kode TEXT UNIQUE NOT NULL`**:
  - Kode mata kuliah (contoh: "IF101", "SI202")
  - Harus unik untuk identifikasi
- **`nama TEXT NOT NULL`**:
  - Nama lengkap mata kuliah (contoh: "Pemrograman Web", "Basis Data")
- **`sks INTEGER NOT NULL`**:
  - Satuan Kredit Semester (SKS)
  - Biasanya bernilai 2, 3, atau 4

#### Tabel Kelas

```javascript
CREATE TABLE IF NOT EXISTS kelas (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  mata_kuliah_id INTEGER NOT NULL,
  dosen_id INTEGER NOT NULL,
  nama_kelas TEXT NOT NULL,
  semester TEXT NOT NULL,
  tahun_akademik TEXT NOT NULL,
  FOREIGN KEY (mata_kuliah_id) REFERENCES mata_kuliah(id) ON DELETE CASCADE,
  FOREIGN KEY (dosen_id) REFERENCES dosen(id) ON DELETE CASCADE
);
```

**Penjelasan Detail:**

- **`mata_kuliah_id`**: Referensi ke mata kuliah yang diajarkan
- **`dosen_id`**: Referensi ke dosen pengampu
- **`nama_kelas TEXT NOT NULL`**:
  - Nama kelas (contoh: "A", "B", "Kelas Malam")
- **`semester TEXT NOT NULL`**:
  - Semester (contoh: "Ganjil", "Genap")
- **`tahun_akademik TEXT NOT NULL`**:
  - Tahun akademik (contoh: "2023/2024")
- Memiliki dua foreign key untuk menghubungkan kelas dengan mata kuliah dan dosen

#### Tabel Peserta Kelas

```javascript
CREATE TABLE IF NOT EXISTS peserta_kelas (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  mahasiswa_id INTEGER NOT NULL,
  kelas_id INTEGER NOT NULL,
  FOREIGN KEY (mahasiswa_id) REFERENCES mahasiswa(id) ON DELETE CASCADE,
  FOREIGN KEY (kelas_id) REFERENCES kelas(id) ON DELETE CASCADE
);
```

**Penjelasan Detail:**

- Tabel junction/bridge untuk relasi many-to-many
- Satu mahasiswa bisa mengambil banyak kelas
- Satu kelas bisa diikuti banyak mahasiswa
- Tabel ini menyimpan relasi tersebut

#### Tabel Sesi Absensi

```javascript
CREATE TABLE IF NOT EXISTS sesi_absensi (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  kelas_id INTEGER NOT NULL,
  pertemuan_ke INTEGER NOT NULL,
  topik TEXT,
  tanggal DATE NOT NULL,
  jam_mulai TIME NOT NULL,
  jam_selesai TIME NOT NULL,
  FOREIGN KEY (kelas_id) REFERENCES kelas(id) ON DELETE CASCADE
);
```

**Penjelasan Detail:**

- **`pertemuan_ke INTEGER NOT NULL`**:
  - Nomor pertemuan (1, 2, 3, dst)
- **`topik TEXT`**:
  - Topik/materi yang dibahas pada pertemuan tersebut
- **`tanggal DATE NOT NULL`**:
  - Tanggal pelaksanaan pertemuan
- **`jam_mulai TIME NOT NULL`**:
  - Waktu mulai perkuliahan
- **`jam_selesai TIME NOT NULL`**:
  - Waktu selesai perkuliahan

#### Tabel Absensi

```javascript
CREATE TABLE IF NOT EXISTS absensi (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  sesi_id INTEGER NOT NULL,
  mahasiswa_id INTEGER NOT NULL,
  status TEXT NOT NULL CHECK(status IN ('hadir', 'izin', 'sakit', 'alpha')),
  waktu_absen DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (sesi_id) REFERENCES sesi_absensi(id) ON DELETE CASCADE,
  FOREIGN KEY (mahasiswa_id) REFERENCES mahasiswa(id) ON DELETE CASCADE
);
```

**Penjelasan Detail:**

- **`sesi_id`**: Referensi ke sesi pertemuan
- **`mahasiswa_id`**: Referensi ke mahasiswa
- **`status TEXT NOT NULL CHECK(status IN ('hadir', 'izin', 'sakit', 'alpha'))`**:
  - Status kehadiran dengan constraint:
    - `hadir`: Mahasiswa hadir
    - `izin`: Mahasiswa tidak hadir dengan izin
    - `sakit`: Mahasiswa sakit
    - `alpha`: Mahasiswa tidak hadir tanpa keterangan
- **`waktu_absen DATETIME DEFAULT CURRENT_TIMESTAMP`**:
  - Mencatat waktu mahasiswa melakukan absensi

### 5. Membuat Indexes

```javascript
CREATE INDEX IF NOT EXISTS idx_mahasiswa_pengguna ON mahasiswa(pengguna_id);
CREATE INDEX IF NOT EXISTS idx_dosen_pengguna ON dosen(pengguna_id);
CREATE INDEX IF NOT EXISTS idx_kelas_mata_kuliah ON kelas(mata_kuliah_id);
CREATE INDEX IF NOT EXISTS idx_kelas_dosen ON kelas(dosen_id);
CREATE INDEX IF NOT EXISTS idx_peserta_mahasiswa ON peserta_kelas(mahasiswa_id);
CREATE INDEX IF NOT EXISTS idx_peserta_kelas ON peserta_kelas(kelas_id);
CREATE INDEX IF NOT EXISTS idx_sesi_kelas ON sesi_absensi(kelas_id);
CREATE INDEX IF NOT EXISTS idx_absensi_sesi ON absensi(sesi_id);
CREATE INDEX IF NOT EXISTS idx_absensi_mahasiswa ON absensi(mahasiswa_id);
```

**Penjelasan Detail:**

Index adalah struktur data yang mempercepat pencarian data dalam database. Tanpa index, database harus melakukan full table scan (memeriksa setiap baris).

**Mengapa Index Penting?**

- Mempercepat query SELECT dengan kondisi WHERE
- Mempercepat JOIN antar tabel
- Meningkatkan performa secara signifikan pada tabel besar

**Index yang Dibuat:**

1. **`idx_mahasiswa_pengguna`**: Mempercepat pencarian mahasiswa berdasarkan pengguna_id
2. **`idx_dosen_pengguna`**: Mempercepat pencarian dosen berdasarkan pengguna_id
3. **`idx_kelas_mata_kuliah`**: Mempercepat pencarian kelas berdasarkan mata kuliah
4. **`idx_kelas_dosen`**: Mempercepat pencarian kelas berdasarkan dosen
5. **`idx_peserta_mahasiswa`**: Mempercepat pencarian kelas yang diambil mahasiswa
6. **`idx_peserta_kelas`**: Mempercepat pencarian mahasiswa dalam satu kelas
7. **`idx_sesi_kelas`**: Mempercepat pencarian sesi absensi untuk kelas tertentu
8. **`idx_absensi_sesi`**: Mempercepat pencarian absensi per sesi
9. **`idx_absensi_mahasiswa`**: Mempercepat pencarian riwayat absensi mahasiswa

**Best Practice:**

- Buat index pada kolom yang sering digunakan dalam WHERE, JOIN, atau ORDER BY
- Kolom foreign key sebaiknya selalu di-index
- Jangan terlalu banyak index karena akan memperlambat INSERT/UPDATE

### 6. Menjalankan Query

```javascript
db.exec(`
  -- SQL queries here
`);
```

**Penjelasan:**

- **`db.exec()`**: Method untuk menjalankan satu atau lebih SQL statement
- Cocok untuk DDL (Data Definition Language) seperti CREATE TABLE
- Tidak mengembalikan data, cocok untuk setup database

### 7. Pesan Sukses

```javascript
console.log("Database tables created successfully!");
```

**Penjelasan:**

- Menampilkan pesan konfirmasi bahwa semua tabel berhasil dibuat
- Membantu dalam monitoring eksekusi script

## Menjalankan Aplikasi

### 1. Jalankan Script Inisialisasi

```bash
node index.js
```

Atau gunakan nodemon untuk development:

```bash
npm run dev
```

### 2. Output yang Diharapkan

Anda akan melihat output berupa query SQL yang dijalankan (karena opsi `verbose`), diikuti dengan pesan:

```
Database tables created successfully!
```

### 3. Verifikasi Database

Setelah script dijalankan, file `absensi.db` akan dibuat di folder project. Anda bisa membukanya menggunakan:

- **DB Browser for SQLite** (GUI)
- **SQLite CLI**: `sqlite3 absensi.db`
- **VS Code Extension**: SQLite Viewer

## Struktur File Project

Setelah setup selesai, struktur project Anda:

```
web-absensi/
├── node_modules/
├── absensi.db           # Database SQLite (dibuat otomatis)
├── diagram.dbml         # Diagram database
├── index.js             # Script inisialisasi database
├── package.json         # Konfigurasi npm
└── package-lock.json    # Lock file dependencies
```

## Kesimpulan

Anda telah berhasil membuat database SQLite untuk sistem absensi mahasiswa dengan:

✅ 8 tabel yang saling berelasi  
✅ Foreign key constraints untuk integritas data  
✅ Indexes untuk performa optimal  
✅ Constraint validasi (CHECK) untuk data yang valid  
✅ Auto-increment primary keys  
✅ Timestamp otomatis untuk audit trail

Database ini siap digunakan sebagai fondasi untuk aplikasi web absensi yang lebih lengkap!

## Referensi

- [better-sqlite3 Documentation](https://github.com/WiseLibs/better-sqlite3)
- [SQLite Official Documentation](https://www.sqlite.org/docs.html)
- [Express.js Documentation](https://expressjs.com/)
- [Node.js Documentation](https://nodejs.org/docs/)

## Lisensi

ISC

---

**Dibuat dengan ❤️ untuk pembelajaran Database SQLite dan Node.js**
