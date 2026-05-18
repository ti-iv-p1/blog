# Penjelasan Lengkap Setiap Kode di TUTORIAL.md

> Dokumen ini menjelaskan fungsi **setiap baris dan blok kode** yang terdapat di `docs/TUTORIAL.md` secara detail dan menyeluruh.

---

## 2. Persiapan Lingkungan

### 2.1 `mkdir web-absensi && cd web-absensi`

```bash
mkdir web-absensi
cd web-absensi
```

**Fungsi:**
- `mkdir web-absensi` — Membuat direktori (folder) baru bernama `web-absensi` di lokasi saat ini. Ini akan menjadi direktori utama proyek.
- `cd web-absensi` — Berpindah (*change directory*) ke dalam direktori `web-absensi` yang baru saja dibuat. Semua perintah selanjutnya akan dijalankan di dalam direktori ini.

---

### 2.2 `npm init -y`

```bash
npm init -y
```

**Fungsi:**
- `npm init` — Perintah untuk menginisialisasi proyek Node.js baru dengan membuat file `package.json`.
- Flag `-y` (singkatan dari `--yes`) — Menerima semua pertanyaan konfigurasi secara otomatis dengan nilai *default*, sehingga file `package.json` langsung dibuat tanpa perlu menjawab pertanyaan satu per satu (nama, versi, deskripsi, entry point, dll.).

**Output:** File `package.json` dengan isi default seperti:
```json
{
  "name": "web-absensi",
  "version": "1.0.0",
  "main": "index.js",
  ...
}
```

---

### 2.3 `npm install express express-handlebars better-sqlite3 bcrypt bootstrap`

```bash
npm install express express-handlebars better-sqlite3 bcrypt bootstrap
```

**Fungsi:**
- `npm install` — Mengunduh dan memasang paket-paket yang disebutkan dari registry npm ke direktori `node_modules/`.
- Tanpa flag `--save-dev`, semua paket otomatis dicatat di bagian `"dependencies"` di `package.json` (paket yang dibutuhkan saat aplikasi berjalan di *production*).

**Penjelasan masing-masing paket:**

| Paket | Kategori | Fungsi Detail |
|-------|----------|---------------|
| `express` | Web framework | Menyediakan fungsi untuk membuat server HTTP, mendefinisikan *route* (URL endpoint), menangani *request/response*, dan memasang *middleware*. Ini adalah fondasi aplikasi web. |
| `express-handlebars` | Template engine | *Plugin* untuk Express yang memungkinkan penggunaan file `.hbs` (Handlebars) sebagai *view*. Handlebars adalah *logic-less template engine* — memisahkan logika dari tampilan. |
| `better-sqlite3` | Database driver | Driver SQLite untuk Node.js yang bekerja secara **sinkron** (berbeda dengan kebanyakan driver database Node.js). Lebih cepat, lebih sederhana, dan tidak memerlukan `async/await`. Cocok untuk aplikasi skala kecil-menengah. |
| `bcrypt` | Keamanan | Library untuk melakukan *hashing* password menggunakan algoritma **bcrypt**. Bcrypt dirancang khusus untuk password — lambat secara sengaja untuk mencegah *brute-force attack*, dan otomatis menangani *salt* (string acak yang ditambahkan sebelum hashing). |
| `bootstrap` | CSS framework | Framework CSS dari Twitter yang menyediakan komponen UI siap pakai: *navbar*, *table*, *form*, *alert*, *button*, *grid system*, dll. Di proyek ini, file Bootstrap di-*serve* langsung dari `node_modules/bootstrap/dist/`. |

---

### 2.4 `npm install --save-dev nodemon`

```bash
npm install --save-dev nodemon
```

**Fungsi:**
- `--save-dev` — Mencatat paket di bagian `"devDependencies"` (hanya dibutuhkan saat pengembangan, bukan di *production*).
- `nodemon` — *Tool* yang memantau perubahan file di direktori proyek dan otomatis me-*restart* server Node.js setiap kali ada file yang disimpan. Tanpa nodemon, kita harus menghentikan dan menjalankan ulang server secara manual setiap kali ada perubahan kode.

---

### 2.5 `package.json`

```json
{
  "name": "web-absensi",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "dev": "nodemon index.js",
    "db:init": "node database/init.js"
  },
  "dependencies": {
    "bcrypt": "^6.0.0",
    "better-sqlite3": "^12.8.0",
    "bootstrap": "^5.3.8",
    "express": "^5.2.1",
    "express-handlebars": "^8.0.2"
  },
  "devDependencies": {
    "nodemon": "^3.1.14"
  }
}
```

**Fungsi setiap field:**

| Field | Fungsi |
|-------|--------|
| `"name"` | Nama proyek. Harus *lowercase*, tanpa spasi. Digunakan oleh npm untuk identifikasi paket. |
| `"version"` | Versi proyek mengikuti [Semantic Versioning](https://semver.org) (`MAJOR.MINOR.PATCH`). |
| `"main"` | *Entry point* — file yang akan dieksekusi saat seseorang menjalankan `require('web-absensi')` atau `node .`. Di sini diatur ke `index.js`. |
| `"scripts"` | Mendefinisikan perintah *shortcut* yang bisa dijalankan dengan `npm run <nama>`. `"dev": "nodemon index.js"` berarti `npm run dev` akan menjalankan `nodemon index.js`. `"db:init": "node database/init.js"` berarti `npm run db:init` akan menjalankan skrip inisialisasi database. |
| `"dependencies"` | Daftar paket yang **wajib** ada agar aplikasi bisa berjalan di *production*. Tanda `^` berarti npm boleh menginstall versi *minor* dan *patch* terbaru (misal `^5.2.1` bisa jadi `5.3.0` tapi tidak `6.0.0`). |
| `"devDependencies"` | Daftar paket yang hanya diperlukan saat **pengembangan** (testing, building, auto-reload). Tidak akan diinstall jika menjalankan `npm install --production`. |

---

### 2.6 Struktur Direktori

```bash
mkdir database models controllers routes views
mkdir views/layouts
mkdir views/pages
```

**Fungsi:**
- Baris pertama: Membuat 5 direktori sekaligus.
  - `database/` — Tempat file konfigurasi koneksi database (`config.js`) dan skrip inisialisasi (`init.js`).
  - `models/` — Tempat file *model* yang berisi fungsi-fungsi query SQL untuk setiap entitas.
  - `controllers/` — Tempat file *controller* yang berisi logika validasi dan koordinasi antara model dan view.
  - `routes/` — Tempat file definisi *route* (URL endpoint) untuk setiap modul.
  - `views/` — Tempat semua template Handlebars (`.hbs`).

- Baris kedua: Membuat sub-direktori untuk layout dan halaman.
  - `views/layouts/` — Untuk file layout utama (`main.hbs`) yang menjadi kerangka semua halaman.
  - `views/pages/` — Untuk file halaman individual yang akan mengisi `{{{body}}}` di layout.

---

## 3. Langkah 1 — Entry Point Minimal Express.js

### 3.1 `index.js` (versi minimal)

```javascript
const express = require('express');

// Inisialisasi aplikasi Express
const app = express();

// Middleware untuk membaca data dari form HTML
app.use(express.urlencoded({ extended: true }));

// Route halaman utama
app.get('/', (req, res) => {
    res.send('<h1>Selamat Datang di Web Absensi</h1>');
});

// Menjalankan server di port 3000
app.listen(3000, () => {
    console.log('Server berjalan di http://localhost:3000');
});
```

**Fungsi setiap baris:**

| Baris | Kode | Fungsi Detail |
|-------|------|---------------|
| 1 | `const express = require('express')` | Mengimpor modul Express. `require()` adalah fungsi bawaan Node.js untuk memuat modul (sistem modul CommonJS). Hasilnya adalah sebuah **fungsi** (bukan objek). |
| 4 | `const app = express()` | Memanggil fungsi `express()` untuk membuat **instance aplikasi Express**. Objek `app` inilah yang memiliki semua metode untuk mendefinisikan route, middleware, dan menjalankan server. |
| 7 | `app.use(express.urlencoded({ extended: true }))` | Memasang **middleware** bawaan Express untuk *parsing* body request HTTP POST yang dikirim dari `<form method="POST">` dengan format `application/x-www-form-urlencoded` (format default form HTML). Middleware ini mengubah data form menjadi objek JavaScript di `req.body`. |
| | `{ extended: true }` | Opsi yang memungkinkan parsing nested objects menggunakan library `qs`. Jika `false`, menggunakan `querystring` bawaan Node.js (tidak mendukung nested objects). `true` direkomendasikan untuk fleksibilitas. |
| 10-12 | `app.get('/', (req, res) => { ... })` | Mendefinisikan **route** HTTP GET untuk path `/` (root/halaman utama). |
| | `req` (request) | Objek yang berisi informasi tentang HTTP request yang masuk (URL, headers, query parameters, body, dll.). |
| | `res` (response) | Objek yang digunakan untuk mengirim HTTP response kembali ke browser. |
| | `res.send('<h1>...</h1>')` | Mengirim string HTML langsung sebagai response. Browser akan menerima dan menampilkannya. Ini hanya untuk testing — nanti akan diganti dengan `res.render()`. |
| 15-17 | `app.listen(3000, () => { ... })` | Memulai server HTTP dan **mendengarkan** (*listening*) di port 3000. *Callback* dijalankan setelah server siap. |
| 16 | `console.log(...)` | Mencetak pesan ke terminal sebagai indikator bahwa server sudah berjalan. |

> **Penjelasan `express.urlencoded({ extended: true })` lebih lanjut:**
> Saat browser mengirim form HTML, data dikirim dalam format `key1=value1&key2=value2` di body request. Middleware ini meng-*parse* string tersebut menjadi objek JavaScript: `{ key1: 'value1', key2: 'value2' }` dan menyimpannya di `req.body`. Tanpa middleware ini, `req.body` akan bernilai `undefined` dan controller tidak bisa membaca data form.

---

## 4. Langkah 2 — Konfigurasi Handlebars & Bootstrap

### 4.1 `index.js` (versi dengan Handlebars & Bootstrap)

```javascript
const express = require('express');
const { engine } = require('express-handlebars');
const path = require('path');

const app = express();
app.use(express.urlencoded({ extended: true }));

// Konfigurasi Handlebars sebagai view engine
app.engine('hbs', engine({
    extname: 'hbs',                           // Ekstensi file template
    defaultLayout: 'main',                     // Layout default
    layoutsDir: path.join(__dirname, 'views/layouts'),
    helpers: {
        inc: (value) => parseInt(value) + 1,  // Helper: increment
        eq: (a, b) => a === b                 // Helper: equality check
    }
}));

app.set('view engine', 'hbs');
app.set('views', path.join(__dirname, 'views'));

// Menyajikan file Bootstrap dari node_modules
app.use('/bootstrap',
    express.static(path.join(__dirname, 'node_modules/bootstrap/dist'))
);

app.get('/', (req, res) => {
    res.render('pages/index');
});

app.listen(3000, () => {
    console.log('Server berjalan di http://localhost:3000');
});
```

**Fungsi setiap bagian:**

#### Import & Inisialisasi (Baris 1-5)

| Baris | Kode | Fungsi |
|-------|------|--------|
| 1 | `const express = require('express')` | Mengimpor Express. |
| 2 | `const { engine } = require('express-handlebars')` | **Destructuring** — mengambil fungsi `engine` dari modul `express-handlebars`. Fungsi ini adalah *factory* yang membuat instance Handlebars engine dengan konfigurasi tertentu. |
| 3 | `const path = require('path')` | Mengimpor modul `path` bawaan Node.js. Digunakan untuk menggabungkan path direktori secara aman lintas sistem operasi (Windows: `\`, Linux/Mac: `/`). |

#### Konfigurasi Handlebars (Baris 8-16)

| Baris | Kode | Fungsi |
|-------|------|--------|
| 8 | `app.engine('hbs', engine({...}))` | Mendaftarkan **template engine** untuk file berekstensi `.hbs`. Parameter pertama (`'hbs'`) adalah nama ekstensi, parameter kedua adalah instance engine yang dikembalikan oleh `engine(...)`. Setelah ini, Express tahu cara merender file `.hbs`. |
| 9 | `extname: 'hbs'` | Menentukan bahwa file template akan menggunakan ekstensi `.hbs` (bukan `.handlebars` yang merupakan default Handlebars). |
| 10 | `defaultLayout: 'main'` | Menentukan layout default yang akan digunakan jika tidak disebutkan secara eksplisit. File `main.hbs` di direktori layouts akan otomatis membungkus setiap halaman. |
| 11 | `layoutsDir: path.join(__dirname, 'views/layouts')` | Menentukan direktori tempat file layout disimpan. `__dirname` adalah variabel global Node.js yang berisi path absolut ke direktori file yang sedang dieksekusi (`index.js`). `path.join` menggabungkan path dengan pemisah yang tepat (`/` atau `\`). Hasilnya misalnya `D:/testing/ibbi/.../views/layouts`. |
| 12-15 | `helpers: { ... }` | Mendefinisikan **custom helper** — fungsi yang bisa dipanggil dari dalam template Handlebars. Handlebars sengaja tidak mendukung logika kompleks di template; helper menjembatani kebutuhan yang terbatas. |
| 13 | `inc: (value) => parseInt(value) + 1` | Helper `inc` (*increment*): menerima nilai, mengubah ke integer, dan menambah 1. Digunakan untuk menampilkan nomor urut di tabel (karena `@index` Handlebars dimulai dari 0). Dipanggil dengan `{{inc @index}}`. |
| 14 | `eq: (a, b) => a === b` | Helper `eq` (*equality*): membandingkan dua nilai dengan *strict equality* (`===`). Mengembalikan `true`/`false`. Digunakan untuk kondisional karena Handlebars tidak memiliki operator perbandingan bawaan. Dipanggil dengan `{{#if (eq program_studi "ti")}}...{{/if}}`. |

#### Set View Engine & Direktori (Baris 18-19)

| Baris | Kode | Fungsi |
|-------|------|--------|
| 18 | `app.set('view engine', 'hbs')` | Mengatur *view engine* default ke `'hbs'`. Ini memberitahu Express bahwa setiap kali `res.render('namaFile')` dipanggil, ia harus mencari file `namaFile.hbs` dan merendernya menggunakan engine yang sudah didaftarkan. |
| 19 | `app.set('views', path.join(__dirname, 'views'))` | Menentukan direktori root untuk semua file template. `res.render('pages/index')` akan mencari file `views/pages/index.hbs`. |

#### Static Files — Bootstrap (Baris 22-24)

| Baris | Kode | Fungsi |
|-------|------|--------|
| 22-24 | `app.use('/bootstrap', express.static(...))` | **Middleware static** — melayani file-file statis (CSS, JS, gambar) dari direktori tertentu melalui URL prefix `/bootstrap`. |
| | `express.static(...)` | Middleware bawaan Express yang membaca file dari disk dan mengirimkannya ke browser dengan MIME type yang sesuai. |
| | `path.join(__dirname, 'node_modules/bootstrap/dist')` | Path ke direktori distribusi Bootstrap yang terinstal di `node_modules`. Direktori `dist/` berisi `css/bootstrap.min.css` dan `js/bootstrap.bundle.min.js`. |

**Efek:** Browser bisa mengakses:
- `http://localhost:3000/bootstrap/css/bootstrap.min.css` → membaca dari `node_modules/bootstrap/dist/css/bootstrap.min.css`
- `http://localhost:3000/bootstrap/js/bootstrap.bundle.min.js` → membaca dari `node_modules/bootstrap/dist/js/bootstrap.bundle.min.js`

> **Mengapa tidak menggunakan CDN?** Melayani Bootstrap dari `node_modules` membuat aplikasi bisa berjalan **offline** tanpa koneksi internet, dan versi Bootstrap terkunci di `package.json`.

#### Route Halaman Utama (Baris 26-28)

| Baris | Kode | Fungsi |
|-------|------|--------|
| 26-28 | `app.get('/', ...)` | Route GET untuk path `/`. |
| 27 | `res.render('pages/index')` | Merender file `views/pages/index.hbs` menggunakan Handlebars, membungkusnya dengan layout `views/layouts/main.hbs` (karena `defaultLayout: 'main'`), dan mengirim hasil HTML ke browser. |

---

### 4.2 Layout Utama `views/layouts/main.hbs`

```handlebars
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Absensi</title>
    <link rel="stylesheet" href="/bootstrap/css/bootstrap.min.css">
</head>
<body>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
            <a class="navbar-brand" href="/">Web Absensi</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                    data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link" href="/mata-kuliah/list">Mata Kuliah</a>
                    </li>
                    <li class="nav-item dropdown">
                        <a class="nav-link dropdown-toggle" href="#" role="button"
                           data-bs-toggle="dropdown">
                            Pengguna
                        </a>
                        <ul class="dropdown-menu">
                            <li><a class="dropdown-item" href="/dosen/list">Dosen</a></li>
                            <li><a class="dropdown-item" href="/mahasiswa/list">Mahasiswa</a></li>
                            <li><hr class="dropdown-divider"></li>
                            <li><a class="dropdown-item" href="/admin/list">Admin</a></li>
                        </ul>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <div class="container py-4">
        {{{ body }}}
    </div>

    <script src="/bootstrap/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

**Fungsi setiap bagian:**

#### Head (Baris 1-7)

| Elemen | Fungsi |
|--------|--------|
| `<html lang="id">` | Elemen root HTML. `lang="id"` menandakan bahasa konten adalah Bahasa Indonesia — penting untuk SEO dan *screen reader* (aksesibilitas). |
| `<meta charset="UTF-8">` | Menentukan *character encoding* UTF-8 yang mendukung semua karakter termasuk huruf beraksen, simbol, dan emoji. |
| `<meta name="viewport" ...>` | **Viewport meta tag** — esensial untuk **responsive design**. Memberitahu browser mobile untuk mengatur lebar viewport sesuai lebar perangkat (`width=device-width`) dan memulai dengan zoom 1:1 (`initial-scale=1.0`). Tanpa ini, halaman mobile akan terlihat seperti versi desktop yang dizoom out. |
| `<link rel="stylesheet" href="/bootstrap/css/bootstrap.min.css">` | Memuat CSS Bootstrap dari path `/bootstrap/` yang dilayani oleh middleware `express.static` (dijelaskan di 4.1). |

#### Navbar (Baris 9-32)

| Elemen Bootstrap | Fungsi |
|------------------|--------|
| `<nav class="navbar navbar-expand-lg bg-body-tertiary">` | **Navbar Bootstrap** yang melebar (*expand*) di layar ≥992px (`lg`). Di bawah ukuran itu, menu akan *collapse* menjadi tombol *hamburger*. `bg-body-tertiary` = warna latar abu-abu terang. |
| `container-fluid` | Membungkus konten navbar dengan lebar penuh dan padding kiri-kanan. |
| `navbar-brand` | Logo/nama brand (di sini "Web Absensi") dengan link ke halaman utama `/`. |
| `navbar-toggler` | **Tombol hamburger** untuk layar kecil. Atribut `data-bs-toggle="collapse"` dan `data-bs-target="#navbarNav"` adalah **Bootstrap JavaScript API** — saat diklik, ia akan menampilkan/menyembunyikan elemen dengan id `navbarNav`. |
| `navbar-toggler-icon` | Ikon tiga garis (hamburger) bawaan Bootstrap. |
| `collapse navbar-collapse` | Elemen yang akan *collapse* di layar kecil. Berisi menu navigasi. |
| `navbar-nav me-auto` | Daftar menu navigasi. `me-auto` = *margin-end auto* — mendorong menu ke kiri dan elemen lain ke kanan. |
| `nav-link` | Link menu individual. |
| `dropdown` / `dropdown-toggle` | **Dropdown menu Bootstrap**. `data-bs-toggle="dropdown"` membuat link menjadi trigger dropdown saat diklik. |
| `dropdown-menu` | Kontainer untuk item dropdown. |
| `dropdown-divider` | Garis pemisah horizontal dalam dropdown — memisahkan opsi Dosen/Mahasiswa dari Admin. |

#### Body Konten (Baris 33-35)

| Elemen | Fungsi |
|--------|--------|
| `<div class="container py-4">` | `container` = pusat konten dengan lebar maksimum responsif. `py-4` = padding vertikal (*padding-y*) sebesar 1.5rem (24px) — memberi ruang di atas dan bawah konten. |
| `{{{ body }}}` | **Ini adalah kunci layout Handlebars.** Tiga kurung kurawal (`{{{ }}}`) artinya *"raw/tidak di-escape"*. Layout akan menyisipkan konten dari view halaman di sini. Jika menggunakan dua kurung kurawal (`{{ body }}`), HTML di dalamnya akan di-*escape* (misal `<h1>` menjadi `&lt;h1&gt;`) sehingga tidak dirender dengan benar. |

#### Script (Baris 37)

| Elemen | Fungsi |
|--------|--------|
| `<script src="/bootstrap/js/bootstrap.bundle.min.js">` | Memuat JavaScript Bootstrap. `bundle` sudah mencakup **Popper.js** (library untuk positioning dropdown, tooltip, popover). Tanpa JavaScript ini, komponen interaktif Bootstrap (dropdown, collapse navbar, modal) tidak akan berfungsi. |

---

### 4.3 `views/pages/index.hbs`

```handlebars
<h1>Selamat Datang di Web Absensi</h1>
<p class="lead">Sistem manajemen kehadiran mahasiswa IBBI.</p>
```

**Fungsi:**
- `<h1>` — Heading utama halaman beranda.
- `<p class="lead">` — Kelas `lead` Bootstrap membuat paragraf ini lebih menonjol dengan ukuran font lebih besar (1.25rem) dan *font-weight* lebih ringan (300).
- Konten ini akan disisipkan ke `{{{ body }}}` di layout `main.hbs`.

---

## 5. Langkah 3 — Database

### 5.1 `database/config.js`

```javascript
const Database = require('better-sqlite3');

// Membuka (atau membuat) file database SQLite
const db = new Database('absensi-ibbi.db');

// Ekspor instance database yang sama ke semua modul
module.exports = db;
```

**Fungsi setiap baris:**

| Baris | Kode | Fungsi |
|-------|------|--------|
| 1 | `const Database = require('better-sqlite3')` | Mengimpor modul `better-sqlite3`. `require()` mengembalikan **class constructor** `Database` (bukan instance). |
| 4 | `const db = new Database('absensi-ibbi.db')` | Membuat instance koneksi database. Jika file `absensi-ibbi.db` belum ada, SQLite akan **membuatnya otomatis**. Jika sudah ada, akan membuka koneksi ke file tersebut. File database adalah file tunggal — portabel dan mudah di-*backup*. |
| 7 | `module.exports = db` | Mengekspor instance `db` yang sama (bukan class, bukan salinan). Ini menerapkan pola **singleton** — semua file yang me-`require('./database/config')` akan mendapatkan **objek yang persis sama**, berbagi satu koneksi database. |

> **Mengapa singleton penting?**
> SQLite hanya mendukung satu *writer* pada satu waktu. Jika kita membuat instance `new Database()` di setiap model, kita akan mendapat error `SQLITE_BUSY: database is locked`. Dengan berbagi satu instance, semua query diantrekan oleh `better-sqlite3` secara otomatis.

---

### 5.2 `database/init.js` — Skrip Inisialisasi Database

```javascript
const db = require('./config');

// Mengaktifkan foreign key (WAJIB — SQLite tidak mengaktifkan secara default)
db.pragma("foreign_keys = ON");

// Tabel pengguna — menyimpan data semua jenis pengguna
db.exec(`
  CREATE TABLE IF NOT EXISTS pengguna (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nama TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    peran TEXT NOT NULL CHECK(peran IN ('admin', 'dosen', 'mahasiswa')),
    dibuat_pada DATETIME DEFAULT CURRENT_TIMESTAMP,
    diperbarui_pada DATETIME DEFAULT CURRENT_TIMESTAMP
  );
`);

// ... tabel lainnya ...
```

**Fungsi setiap bagian:**

#### `db.pragma("foreign_keys = ON")`

| Fungsi | Detail |
|--------|--------|
| `PRAGMA` | Perintah SQLite untuk mengatur konfigurasi internal database. |
| `foreign_keys = ON` | Mengaktifkan **foreign key constraint enforcement**. **Ini WAJIB.** SQLite secara default **tidak mengeksekusi** constraint foreign key demi kompatibilitas ke belakang. Tanpa ini, `ON DELETE CASCADE`, `REFERENCES`, dan constraint foreign key lainnya akan **diabaikan** (tidak error, tapi tidak berfungsi). Ini adalah jebakan umum SQLite. |

#### Tabel `pengguna`

| Kolom | Type & Constraint | Penjelasan Detail |
|-------|-------------------|-------------------|
| `id` | `INTEGER PRIMARY KEY AUTOINCREMENT` | **Primary key** — identifier unik setiap baris. `AUTOINCREMENT` membuat SQLite secara otomatis mengisi nilai yang selalu bertambah (1, 2, 3...). Nilai yang sudah di-`DELETE` tidak akan digunakan kembali. Di SQLite, `INTEGER PRIMARY KEY` saja sudah auto-increment, tapi `AUTOINCREMENT` eksplisit mencegah penggunaan ulang rowid. |
| `nama` | `TEXT NOT NULL` | Nama lengkap pengguna. `NOT NULL` — tidak boleh kosong (NULL). `TEXT` — tipe teks (SQLite fleksibel dengan tipe data, tapi TEXT adalah yang paling tepat). |
| `email` | `TEXT UNIQUE NOT NULL` | Alamat email. `UNIQUE` — tidak boleh ada dua pengguna dengan email yang sama. SQLite akan menolak INSERT kedua dengan error `UNIQUE constraint failed`. |
| `password` | `TEXT NOT NULL` | Password dalam bentuk hash bcrypt (**bukan plaintext**). Panjang hash bcrypt selalu 60 karakter. |
| `peran` | `TEXT NOT NULL CHECK(peran IN ('admin', 'dosen', 'mahasiswa'))` | **Role-based access control** (RBAC). `CHECK` constraint membatasi nilai yang bisa dimasukkan — hanya tiga role yang valid. Jika INSERT mencoba nilai lain (misal `'guest'`), SQLite akan menolak dengan error `CHECK constraint failed`. |
| `dibuat_pada` | `DATETIME DEFAULT CURRENT_TIMESTAMP` | Timestamp otomatis saat baris dibuat. `DEFAULT CURRENT_TIMESTAMP` — jika tidak disebutkan saat INSERT, otomatis diisi dengan waktu sekarang. |
| `diperbarui_pada` | `DATETIME DEFAULT CURRENT_TIMESTAMP` | Timestamp untuk tracking update terakhir. Saat ini default-nya sama dengan `dibuat_pada`; idealnya di-update manual saat UPDATE via aplikasi. |

#### Tabel `mahasiswa`

| Kolom | Type & Constraint | Penjelasan |
|-------|-------------------|------------|
| `pengguna_id` | `INTEGER NOT NULL` + `FOREIGN KEY ... REFERENCES pengguna(id) ON DELETE CASCADE` | **Foreign key** ke `pengguna.id`. Menghubungkan data spesifik mahasiswa dengan data umum pengguna. `ON DELETE CASCADE` — jika pengguna dihapus, baris mahasiswa ini **otomatis** ikut terhapus. Tanpa CASCADE, menghapus pengguna akan error karena mahasiswa masih mereferensi ID tersebut. |
| `nim` | `TEXT UNIQUE NOT NULL` | Nomor Induk Mahasiswa — unik untuk setiap mahasiswa. |
| `program_studi` | `TEXT` | Kode program studi: `ti` (Teknik Informatika), `si` (Sistem Informasi), `it` (Teknologi Informasi). |
| `angkatan` | `INTEGER` | Tahun angkatan (2000-2100). |

#### Tabel `dosen`

| Kolom | Type & Constraint | Penjelasan |
|-------|-------------------|------------|
| `nidn` | `TEXT UNIQUE NOT NULL` | Nomor Induk Dosen Nasional — unik, tepat 10 digit. |
| `departemen` | `TEXT` | Kode departemen: `fish` atau `fast`. |

#### Tabel `mata_kuliah`

| Kolom | Type & Constraint | Penjelasan |
|-------|-------------------|------------|
| `kode` | `TEXT UNIQUE NOT NULL` | Kode unik mata kuliah (alfanumerik, misal `IF123`). |
| `nama` | `TEXT NOT NULL` | Nama lengkap mata kuliah. |
| `sks` | `INTEGER NOT NULL` | Jumlah SKS (1-6). Tipe INTEGER untuk menyimpan angka bulat. |

> **Mengapa `CREATE TABLE IF NOT EXISTS`?** Aman dijalankan berkali-kali — jika tabel sudah ada, perintah ini diabaikan (tidak error). Berguna saat development dan deployment.

---

## 6. Langkah 4 — Modul Mata Kuliah (CRUD Pertama)

### 6.1 Model `models/MataKuliah.js`

```javascript
const db = require('../database/config');

// Mengambil semua mata kuliah
function ambilSemuaMataKuliah() {
    return db.prepare('SELECT * FROM mata_kuliah').all();
}

// Mengambil satu mata kuliah berdasarkan ID
function ambilMataKuliahById(id) {
    return db.prepare('SELECT * FROM mata_kuliah WHERE id = ?').get(id);
}

// Membuat mata kuliah baru
function buatMataKuliah(nama, kode, sks) {
    const stmt = db.prepare(
        'INSERT INTO mata_kuliah (nama, kode, sks) VALUES (?, ?, ?)'
    );
    stmt.run(nama, kode, sks);
}

// Memperbarui mata kuliah
function updateMataKuliah(id, nama, kode, sks) {
    const stmt = db.prepare(
        'UPDATE mata_kuliah SET nama = ?, kode = ?, sks = ? WHERE id = ?'
    );
    stmt.run(nama, kode, sks, id);
}

// Menghapus mata kuliah
function hapusMataKuliah(id) {
    db.prepare('DELETE FROM mata_kuliah WHERE id = ?').run(id);
}

module.exports = {
    ambilSemuaMataKuliah,
    ambilMataKuliahById,
    buatMataKuliah,
    updateMataKuliah,
    hapusMataKuliah
};
```

**Fungsi setiap fungsi:**

#### `ambilSemuaMataKuliah()`

| Bagian | Fungsi |
|--------|--------|
| `db.prepare('SELECT * FROM mata_kuliah')` | **Mengkompilasi** (bukan mengeksekusi) query SQL menjadi *prepared statement*. Simbol `*` berarti "semua kolom". `prepare()` mem-*parsing* dan mem-validasi SQL sekali, lalu mengembalikan objek statement yang bisa dieksekusi berkali-kali. |
| `.all()` | Mengeksekusi prepared statement dan mengembalikan **semua baris** sebagai array of objects. Jika tidak ada data, mengembalikan array kosong `[]`. |

#### `ambilMataKuliahById(id)`

| Bagian | Fungsi |
|--------|--------|
| `WHERE id = ?` | Tanda tanya `?` adalah **placeholder parameter**. Nilai `id` akan dimasukkan di sini secara aman — `better-sqlite3` otomatis meng-*escape* nilai untuk mencegah SQL injection. |
| `.get(id)` | Mengeksekusi statement dengan parameter `id`, dan mengembalikan **satu baris pertama** sebagai object. Jika tidak ditemukan, mengembalikan `undefined`. |

#### `buatMataKuliah(nama, kode, sks)`

| Bagian | Fungsi |
|--------|--------|
| `const stmt = db.prepare(...)` | Menyimpan prepared statement ke variabel untuk digunakan sekali atau berkali-kali. |
| `stmt.run(nama, kode, sks)` | Mengeksekusi INSERT. Parameter di-`run()` akan menggantikan placeholder `?` secara berurutan. Tidak ada nilai balik yang signifikan (karena ini adalah tabel sederhana tanpa auto-increment yang dibutuhkan). |

#### `updateMataKuliah(id, nama, kode, sks)`

| Bagian | Fungsi |
|--------|--------|
| `UPDATE ... SET ... WHERE id = ?` | Memperbarui baris yang `id`-nya cocok. **Tanpa `WHERE`**, semua baris akan diperbarui — ini hampir selalu kesalahan fatal. |
| `stmt.run(nama, kode, sks, id)` | Urutan parameter HARUS sesuai dengan urutan placeholder `?` di SQL. Di sini: `?` ke-1 = nama, ke-2 = kode, ke-3 = sks, ke-4 = id. |

#### `hapusMataKuliah(id)`

| Bagian | Fungsi |
|--------|--------|
| `DELETE FROM mata_kuliah WHERE id = ?` | Menghapus satu baris. `WHERE` wajib untuk mencegah penghapusan semua data. |
| `.run(id)` | Eksekusi dengan parameter `id`. |

#### `module.exports = { ... }`

| Bagian | Fungsi |
|--------|--------|
| `module.exports` | Objek yang akan dikembalikan saat file ini di-`require()` oleh file lain. Hanya fungsi yang diekspor yang bisa diakses dari luar. Fungsi internal (jika ada) tetap privat. |

> **Mengapa `db.prepare()`?**
> 1. **Keamanan**: Placeholder `?` mencegah **SQL injection**. Jika kita menggunakan string concatenation (`'SELECT * FROM mhs WHERE id = ' + id`), attacker bisa menyisipkan SQL berbahaya.
> 2. **Kinerja**: SQL dikompilasi sekali, bisa dieksekusi berkali-kali tanpa parsing ulang. Untuk aplikasi web yang sering menjalankan query yang sama, ini signifikan.
> 3. **Kenyamanan**: Tidak perlu mengurung string dengan tanda kutip secara manual.

---

### 6.2 Controller `controllers/MataKuliahController.js`

#### `validateMataKuliah(kode, nama, sks)`

```javascript
function validateMataKuliah(kode, nama, sks) {
    const pesanError = [];

    if (!kode || kode.trim() === '') {
        pesanError.push("Kode mata kuliah tidak boleh kosong");
    } else if (kode.trim().length < 3) {
        pesanError.push("Kode mata kuliah harus terdiri dari minimal 3 karakter");
    } else if (!/^[a-zA-Z0-9]+$/.test(kode.trim())) {
        pesanError.push("Kode mata kuliah hanya boleh mengandung huruf dan angka");
    }

    if (!nama || nama.trim() === '') {
        pesanError.push("Nama mata kuliah tidak boleh kosong");
    } else if (nama.trim().length < 4) {
        pesanError.push("Nama mata kuliah harus terdiri dari minimal 4 karakter");
    }

    if (!sks || isNaN(sks) || sks <= 0) {
        pesanError.push("SKS harus berupa angka positif");
    } else if (sks < 1 || sks > 6) {
        pesanError.push("SKS harus antara 1 sampai 6");
    }

    return pesanError;
}
```

**Fungsi & alur validasi:**

| Baris | Kode | Fungsi Detail |
|-------|------|---------------|
| 1 | `function validateMataKuliah(kode, nama, sks)` | Fungsi validasi menerima tiga parameter dari form. Mengembalikan **array** (bukan boolean) — array kosong `[]` berarti valid, array berisi string berarti ada error. Pola ini memungkinkan menampilkan **semua error sekaligus** ke pengguna. |
| 2 | `const pesanError = []` | Inisialisasi array kosong untuk menampung pesan error. |
| 4-10 | Validasi `kode` | Tiga lapis validasi berjenjang (`if...else if`): |
| | `!kode \|\| kode.trim() === ''` | Cek kosong: `!kode` menangani `undefined`/`null`, `kode.trim() === ''` menangani string yang hanya berisi spasi. `.trim()` menghapus spasi di awal/akhir. |
| | `kode.trim().length < 3` | Cek panjang minimal. |
| | `!/^[a-zA-Z0-9]+$/.test(kode.trim())` | **Regex** — `/^[a-zA-Z0-9]+$/` artinya dari awal (`^`) sampai akhir (`$`) hanya boleh mengandung huruf (a-z, A-Z) dan angka (0-9), minimal 1 karakter (`+`). `.test()` mengembalikan `true` jika string cocok, `false` jika tidak. Tanda `!` membalikkan logika — jadi error jika TIDAK cocok. |
| 12-16 | Validasi `nama` | Mirip dengan validasi kode, tapi minimal 4 karakter dan tidak ada batasan karakter khusus (hanya cek kosong dan panjang). |
| 18-22 | Validasi `sks` | `isNaN(sks)` — mengecek apakah nilai bukan angka (karena input dari form selalu string, `isNaN('abc')` → `true`). `sks <= 0` — menolak nol dan negatif. |
| 24 | `return pesanError` | Mengembalikan array error. Jika tidak ada error, array kosong. |

#### `showCreateForm(req, res)`

```javascript
function showCreateForm(req, res) {
    res.render('pages/mata-kuliah/create');
}
```

**Fungsi:** Menampilkan form pembuatan mata kuliah. `res.render('pages/mata-kuliah/create')` akan merender `views/pages/mata-kuliah/create.hbs` dengan layout `main.hbs`. Tidak ada data yang dikirim ke view.

#### `listMataKuliah(req, res)`

```javascript
function listMataKuliah(req, res) {
    const mataKuliah = MataKuliahModel.ambilSemuaMataKuliah();
    res.render('pages/mata-kuliah/list', { mataKuliah });
}
```

**Fungsi:** 
1. Memanggil model untuk mengambil semua data dari database.
2. Merender view list dan mengirim data sebagai objek `{ mataKuliah: [...] }`. Di template, data ini bisa diakses sebagai `{{mataKuliah}}` atau di-loop dengan `{{#each mataKuliah}}`.

#### `showEditForm(req, res)`

```javascript
function showEditForm(req, res) {
    const { id } = req.params;
    const mataKuliah = MataKuliahModel.ambilMataKuliahById(id);
    res.render('pages/mata-kuliah/edit', { mataKuliah });
}
```

**Fungsi:**
1. `const { id } = req.params` — **Destructuring** untuk mengambil parameter `:id` dari URL (didefinisikan di route sebagai `/edit/:id`). `req.params` adalah objek yang berisi semua parameter URL. Jika URL-nya `/mata-kuliah/edit/5`, maka `req.params.id` = `"5"` (string).
2. Mengambil data mata kuliah berdasarkan ID dari database.
3. Merender form edit dengan data yang sudah diisi (sehingga field form menampilkan nilai saat ini).

#### `createMataKuliah(req, res)`

```javascript
function createMataKuliah(req, res) {
    const { nama, kode, sks } = req.body;
    const pesanError = validateMataKuliah(kode, nama, sks);

    if (pesanError.length > 0) {
        res.render('pages/mata-kuliah/create', {
            pesanError,
            formData: { nama, kode, sks }
        });
        return; // PENTING: hentikan eksekusi
    }

    MataKuliahModel.buatMataKuliah(nama, kode, sks);
    res.redirect('/mata-kuliah/list');
}
```

**Fungsi setiap langkah:**

| Langkah | Kode | Penjelasan |
|---------|------|------------|
| 1 | `const { nama, kode, sks } = req.body` | Mengambil data dari form yang dikirim via POST. `req.body` tersedia berkat middleware `express.urlencoded()`. Nama field diambil dari atribut `name` di `<input>` HTML. |
| 2 | `const pesanError = validateMataKuliah(...)` | Menjalankan validasi terhadap semua field. |
| 3 | `if (pesanError.length > 0)` | Cek apakah ada error. Jika array tidak kosong, berarti ada yang tidak valid. |
| 4 | `res.render('pages/mata-kuliah/create', { ... })` | **Menampilkan ulang form** dengan pesan error DAN data yang sudah diisi (`formData`). Ini penting untuk UX — pengguna tidak perlu mengisi ulang semua field. |
| 5 | `return;` | **KRITIS**. Menghentikan eksekusi fungsi. Tanpa `return`, kode di bawahnya akan tetap dijalankan meskipun ada error, menyebabkan data tidak valid tetap tersimpan ke database. |
| 6 | `MataKuliahModel.buatMataKuliah(...)` | Data valid → simpan ke database melalui model. |
| 7 | `res.redirect('/mata-kuliah/list')` | **Redirect setelah POST** — pola PRG (Post/Redirect/Get). Mencegah *double submission* jika pengguna me-refresh halaman. Browser akan membuat GET request baru ke `/mata-kuliah/list`. |

#### `editMataKuliah(req, res)`

```javascript
function editMataKuliah(req, res) {
    const { nama, kode, sks } = req.body;
    const { id } = req.params;
    const pesanError = validateMataKuliah(kode, nama, sks);

    if (pesanError.length > 0) {
        res.render('pages/mata-kuliah/edit', {
            pesanError,
            mataKuliah: { id, nama, kode, sks }
        });
        return;
    }

    MataKuliahModel.updateMataKuliah(id, nama, kode, sks);
    res.redirect('/mata-kuliah/list');
}
```

**Perbedaan dengan `createMataKuliah`:**
- `const { id } = req.params` — ID diambil dari URL (misal `edit/5`).
- Jika error, view edit di-render dengan objek `mataKuliah` (bukan `formData` seperti di create) karena view edit menggunakan `{{mataKuliah.nama}}`, bukan `{{formData.nama}}`.
- Memanggil `updateMataKuliah` (bukan `buatMataKuliah`).

#### `deleteMataKuliah(req, res)`

```javascript
function deleteMataKuliah(req, res) {
    const { id } = req.params;
    MataKuliahModel.hapusMataKuliah(id);
    res.redirect('/mata-kuliah/list');
}
```

**Fungsi:** Menghapus data dari database, lalu redirect ke halaman list. Tidak ada validasi khusus karena penghapusan hanya butuh ID (yang sudah tervalidasi oleh route).

---

### 6.3 Route `routes/matakuliahRoutes.js`

```javascript
const router = require('express').Router();
const MataKuliahController = require('../controllers/MataKuliahController');

router.get('/list', MataKuliahController.listMataKuliah);
router.get('/create', MataKuliahController.showCreateForm);
router.get('/edit/:id', MataKuliahController.showEditForm);
router.post('/create', MataKuliahController.createMataKuliah);
router.post('/edit/:id', MataKuliahController.editMataKuliah);
router.post('/delete/:id', MataKuliahController.deleteMataKuliah);

module.exports = router;
```

**Fungsi setiap baris:**

| Baris | Kode | Fungsi Detail |
|-------|------|---------------|
| 1 | `require('express').Router()` | Mengimpor Express dan langsung memanggil `.Router()` — sebuah **mini Express app** yang bisa memiliki route, middleware sendiri. Router ini akan "ditempelkan" ke app utama di `index.js`. |
| 2 | `require('../controllers/MataKuliahController')` | Mengimpor controller. Path `../` berarti naik satu level dari `routes/` ke root proyek. |
| 4-9 | `router.get(...)` / `router.post(...)` | Mendefinisikan route. **HTTP method** (`GET`/`POST`) harus sesuai — GET untuk membaca/menampilkan, POST untuk mengubah data (create, update, delete). |
| | `'/edit/:id'` | **Route parameter** — `:id` adalah *placeholder* yang akan menangkap nilai apa pun di segmen URL tersebut. Untuk URL `/mata-kuliah/edit/5`, nilai `req.params.id` akan menjadi `"5"`. |
| | Handler function | Referensi ke fungsi controller (**tanpa** `()` — tidak dipanggil di sini, hanya diberikan sebagai callback. Express akan memanggilnya dengan `req, res` saat request masuk). |
| 11 | `module.exports = router` | Mengekspor router agar bisa di-mount di `index.js` dengan `app.use(...)`. |

> **HTTP Method Convention (Konvensi REST):**
> - **GET** — Membaca data (tidak mengubah state server). Aman untuk di-*refresh* dan di-*bookmark*.
> - **POST** — Membuat/mengubah/menghapus data (mengubah state server). Form submission dengan `method="POST"`. Tidak boleh di-*refresh* tanpa konfirmasi.

---

### 6.4 Views

#### `views/pages/mata-kuliah/create.hbs`

```handlebars
<h1>Tambah Mata Kuliah</h1>

{{#if pesanError}}
    <div class="alert alert-danger">
        <ul>
            {{#each pesanError}}
                <li>{{this}}</li>
            {{/each}}
        </ul>
    </div>
{{/if}}

<form action="/mata-kuliah/create" method="POST">
    <div class="mb-3">
        <label for="kode" class="form-label">Kode</label>
        <input type="text" class="form-control" id="kode" name="kode"
               value="{{formData.kode}}" required>
    </div>
    <div class="mb-3">
        <label for="nama" class="form-label">Nama</label>
        <input type="text" class="form-control" id="nama" name="nama"
               value="{{formData.nama}}" required>
    </div>
    <div class="mb-3">
        <label for="sks" class="form-label">SKS</label>
        <input type="number" class="form-control" id="sks" name="sks"
               value="{{formData.sks}}" min="1" max="6" required>
    </div>
    <button type="submit" class="btn btn-primary">Simpan</button>
    <a href="/mata-kuliah/list" class="btn btn-secondary">Batal</a>
</form>
```

**Fungsi setiap blok:**

| Blok Handlebars / HTML | Fungsi Detail |
|------------------------|---------------|
| `{{#if pesanError}}...{{/if}}` | **Block helper** Handlebars. Jika `pesanError` ada dan *truthy* (array tidak kosong), render blok di dalamnya. Jika `pesanError` tidak dikirim atau array kosong, blok diabaikan. |
| `class="alert alert-danger"` | Kelas Bootstrap untuk kotak peringatan merah. |
| `{{#each pesanError}}...{{/each}}` | **Looping** melalui array `pesanError`. Untuk setiap elemen, render `<li>`. |
| `{{this}}` | Di dalam `{{#each}}`, `this` merujuk ke elemen array saat ini (string pesan error). |
| `value="{{formData.kode}}"` | **Mempertahankan input** setelah validasi gagal. `formData` adalah objek yang dikirim controller saat ada error. Jika pertama kali membuka form, `formData` tidak ada → `value=""` (kosong). |
| `name="kode"` | **Nama field form**. Inilah yang menjadi key di `req.body` (`req.body.kode`). HARUS sesuai dengan yang digunakan di controller. |
| `required` | Atribut HTML5 untuk validasi sisi klien — browser tidak akan mengirim form jika field kosong. Namun ini **bukan pengganti** validasi server (bisa di-bypass). |
| `type="number" min="1" max="6"` | Input khusus angka dengan batasan nilai. Browser akan menolak nilai di luar 1-6. |
| `<button type="submit">` | Tombol submit — mengirim form ke URL di atribut `action`. |
| `<a href="..." class="btn btn-secondary">Batal</a>` | Link bergaya tombol — kembali ke halaman list tanpa mengirim form. |

#### `views/pages/mata-kuliah/list.hbs`

```handlebars
<div class="d-flex justify-content-between mb-3">
    <h1>Daftar Mata Kuliah</h1>
    <a href="/mata-kuliah/create" class="btn btn-success">Tambah Mata Kuliah</a>
</div>

<table class="table table-bordered">
    <thead>
        <tr>
            <th>No</th>
            <th>Kode</th>
            <th>Nama</th>
            <th>SKS</th>
            <th colspan="2">Aksi</th>
        </tr>
    </thead>
    <tbody>
        {{#unless mataKuliah.length}}
        <tr>
            <td colspan="6" class="text-center">
                Belum ada mata kuliah.
                <a href="/mata-kuliah/create">Tambah sekarang</a>
            </td>
        </tr>
        {{/unless}}

        {{#each mataKuliah}}
        <tr>
            <td>{{inc @index}}</td>
            <td>{{kode}}</td>
            <td>{{nama}}</td>
            <td>{{sks}}</td>
            <td>
                <a href="/mata-kuliah/edit/{{id}}" class="btn btn-warning btn-sm">Edit</a>
            </td>
            <td>
                <form action="/mata-kuliah/delete/{{id}}" method="POST"
                      onsubmit="return confirm('Yakin ingin menghapus mata kuliah ini?');">
                    <button type="submit" class="btn btn-danger btn-sm">Hapus</button>
                </form>
            </td>
        </tr>
        {{/each}}
    </tbody>
</table>
```

**Fungsi setiap blok:**

| Blok | Fungsi Detail |
|------|---------------|
| `d-flex justify-content-between` | Flexbox Bootstrap — judul di kiri, tombol di kanan, sejajar vertikal. |
| `{{#unless mataKuliah.length}}...{{/unless}}` | **Kebalikan dari `#if`** — render jika `mataKuliah.length` *falsy* (0). Artinya: tampilkan pesan "Belum ada data" hanya jika array kosong. |
| `colspan="6"` | Menggabungkan 6 kolom menjadi satu baris penuh. |
| `{{#each mataKuliah}}...{{/each}}` | Looping setiap mahasiswa. Di dalam blok ini, **konteks berubah** — `this` merujuk ke objek mahasiswa saat ini. |
| `{{inc @index}}` | Memanggil helper `inc` dengan `@index` (variabel bawaan Handlebars yang dimulai dari 0). Hasil: 1, 2, 3... |
| `{{id}}`, `{{kode}}`, `{{nama}}`, `{{sks}}` | Properti objek yang di-loop. Setiap iterasi `#each`, properti ini langsung bisa diakses tanpa prefix. |
| `onsubmit="return confirm('...');"` | **JavaScript inline** — saat form disubmit, `confirm()` menampilkan dialog konfirmasi browser. Jika user klik "Cancel", `confirm()` mengembalikan `false` → `onsubmit="return false"` → form tidak jadi dikirim. |
| `<form action="/mata-kuliah/delete/{{id}}" method="POST">` | Setiap tombol Hapus adalah **form terpisah** — ini pola yang aman karena penghapusan adalah operasi POST (bukan GET). Jika menggunakan `<a href="...">`, browser bisa secara tidak sengaja menghapus data (misal saat crawler/search engine mengikuti link). |

#### `views/pages/mata-kuliah/edit.hbs`

```handlebars
<form action="/mata-kuliah/edit/{{mataKuliah.id}}" method="POST">
    <div class="mb-3">
        <label for="kode" class="form-label">Kode</label>
        <input type="text" class="form-control" id="kode" name="kode"
               value="{{mataKuliah.kode}}" required>
    </div>
    <!-- ... field lainnya ... -->
    <button type="submit" class="btn btn-primary">Perbarui</button>
    <a href="/mata-kuliah/list" class="btn btn-secondary">Batal</a>
</form>
```

**Perbedaan dengan create.hbs:**
- `action="/mata-kuliah/edit/{{mataKuliah.id}}"` — URL submit berisi ID yang akan diedit.
- `value="{{mataKuliah.kode}}"` — Data diisi dari objek `mataKuliah` (hasil query database), bukan `formData` (hasil input sebelumnya).
- Jika ada error validasi, controller mengirim ulang objek `mataKuliah` dengan nilai baru, sehingga field tetap menampilkan input terakhir.

---

### 6.5 Mendaftarkan Route di `index.js`

```javascript
const matakuliahRoutes = require('./routes/matakuliahRoutes');
app.use("/mata-kuliah", matakuliahRoutes);
```

**Fungsi:**
- `require('./routes/matakuliahRoutes')` — Mengimpor router yang sudah diekspor.
- `app.use("/mata-kuliah", matakuliahRoutes)` — **Mounting** router di prefix path `/mata-kuliah`. Semua route di dalam router akan otomatis ditambahkan prefix ini. Jadi route `/list` di router akan tersedia di URL `/mata-kuliah/list`, `/create` → `/mata-kuliah/create`, dsb.

---

## 7. Langkah 5 — Modul Mahasiswa (Relasi Tabel)

### 7.1 Model `models/Mahasiswa.js`

#### `ambilSemuaMahasiswa()` & `ambilMahasiswaById(id)`

```javascript
function ambilSemuaMahasiswa() {
    return db.prepare(`
        SELECT pengguna.id, mahasiswa.nim, mahasiswa.program_studi,
               mahasiswa.angkatan, pengguna.nama, pengguna.email
        FROM mahasiswa
        JOIN pengguna ON mahasiswa.pengguna_id = pengguna.id
    `).all();
}
```

**Fungsi SQL JOIN:**

| Klausa | Fungsi |
|--------|--------|
| `SELECT pengguna.id, mahasiswa.nim, ...` | Memilih kolom spesifik dari dua tabel. **Perhatikan**: `pengguna.id`, bukan `mahasiswa.id` — karena kita ingin ID dari tabel pengguna (yang digunakan sebagai referensi di seluruh aplikasi). Jika ada kolom dengan nama sama di dua tabel, harus diberi prefix (`pengguna.id`). |
| `FROM mahasiswa` | Tabel utama. |
| `JOIN pengguna ON mahasiswa.pengguna_id = pengguna.id` | **INNER JOIN** — menggabungkan baris dari `mahasiswa` dan `pengguna` di mana `pengguna_id` di tabel mahasiswa cocok dengan `id` di tabel pengguna. Hanya mahasiswa yang memiliki data pengguna valid yang akan muncul. |

> **Mengapa JOIN dibutuhkan?** Karena data tersebar di dua tabel:
> - `pengguna`: nama, email, password
> - `mahasiswa`: nim, program_studi, angkatan, pengguna_id (FK)
>
> Untuk menampilkan data lengkap (nama + nim), kita harus JOIN kedua tabel.

#### `buatMahasiswa(nim, nama, email, program_studi, angkatan)`

```javascript
function buatMahasiswa(nim, nama, email, program_studi, angkatan) {
    // Langkah 1: INSERT ke tabel pengguna
    const stmt = db.prepare(`
        INSERT INTO pengguna (nama, email, password, peran)
        VALUES (?, ?, ?, ?)
    `);
    const result = stmt.run(
        nama,
        email,
        bcrypt.hashSync(nim, 10),
        'mahasiswa'
    );

    // Langkah 2: Ambil ID pengguna yang baru dibuat
    const penggunaId = result.lastInsertRowid;

    // Langkah 3: INSERT ke tabel mahasiswa dengan pengguna_id
    const mahasiswaStmt = db.prepare(`
        INSERT INTO mahasiswa (pengguna_id, nim, program_studi, angkatan)
        VALUES (?, ?, ?, ?)
    `);
    mahasiswaStmt.run(penggunaId, nim, program_studi, angkatan);
}
```

**Fungsi setiap langkah (transaksi manual):**

| Langkah | Kode | Penjelasan Detail |
|---------|------|-------------------|
| 1 | `INSERT INTO pengguna ...` | Menyimpan data umum (nama, email, password, peran) ke tabel induk. Password di-hash dengan bcrypt. |
| | `bcrypt.hashSync(nim, 10)` | `hashSync` — versi sinkron dari bcrypt hash. Parameter pertama: string yang akan di-hash (NIM sebagai password default). Parameter kedua: `saltRounds = 10` — semakin besar semakin aman tapi semakin lambat. 10 adalah standar industri yang seimbang. |
| 2 | `result.lastInsertRowid` | **Kunci relasi.** Setelah INSERT ke tabel dengan kolom `INTEGER PRIMARY KEY AUTOINCREMENT`, `better-sqlite3` menyimpan ID yang baru dibuat di properti `lastInsertRowid`. Nilai ini digunakan untuk menghubungkan baris `mahasiswa` ke `pengguna`. |
| 3 | `INSERT INTO mahasiswa (pengguna_id, ...)` | Menyimpan data spesifik mahasiswa dengan `pengguna_id` yang didapat dari langkah 2. |

> **Mengapa tidak pakai transaksi database?** `better-sqlite3` berjalan sinkron — tidak ada concurrent request yang bisa menginterupsi di antara INSERT pengguna dan INSERT mahasiswa. Di production dengan database client-server (PostgreSQL, MySQL), ini harus dibungkus dalam `BEGIN...COMMIT` transaction.

#### `hapusMahasiswa(id)`

```javascript
function hapusMahasiswa(id) {
    db.prepare('DELETE FROM pengguna WHERE id = ?').run(id);
}
```

**Fungsi:** Hanya menghapus dari tabel `pengguna`. Karena foreign key `mahasiswa.pengguna_id` memiliki `ON DELETE CASCADE`, baris terkait di tabel `mahasiswa` akan **otomatis terhapus** oleh SQLite. Tidak perlu menghapus manual dari tabel `mahasiswa`.

---

### 7.2 Controller `controllers/MahasiswaController.js`

#### `validateMahasiswa(nim, nama, email, program_studi, angkatan)`

```javascript
// Validasi NIM: wajib, 8-15 digit angka
if (!nim || nim.trim() === '') {
    pesanError.push("NIM mahasiswa tidak boleh kosong");
} else if (!/^\d{8,15}$/.test(nim.trim())) {
    pesanError.push("NIM mahasiswa harus terdiri dari 8 sampai 15 digit angka");
}
```

**Regex `^\\d{8,15}$` detail:**
- `^` — Mulai dari awal string
- `\d` — Karakter digit (0-9)
- `{8,15}` — Harus ada 8 sampai 15 karakter (tidak kurang, tidak lebih)
- `$` — Sampai akhir string
- `.test()` — Mengembalikan `true` jika string **sepenuhnya** cocok dengan pola

```javascript
// Validasi Program Studi
if (!program_studi ||
    (program_studi !== 'ti' && program_studi !== 'si' && program_studi !== 'it')) {
    pesanError.push("Program studi mahasiswa harus dipilih dan valid");
}
```

**Fungsi:** Mengecek bahwa `program_studi` adalah salah satu dari tiga nilai valid (`ti`, `si`, `it`). Ini mencegah nilai arbitrer masuk ke database.

```javascript
// Validasi Angkatan
if (!angkatan || isNaN(angkatan) || angkatan < 2000 || angkatan > 2100) {
    pesanError.push("Angkatan mahasiswa harus diisi dan antara 2000 sampai 2100");
}
```

**Fungsi:** Memastikan angkatan adalah angka dalam rentang wajar.

---

### 7.4 Views — Dropdown Program Studi

```handlebars
<select class="form-control" id="program_studi" name="program_studi" required>
    <option value="">Pilih Program Studi</option>
    <option value="ti">Teknik Informatika (TI)</option>
    <option value="it">Teknologi Informasi (IT)</option>
    <option value="si">Sistem Informasi (SI)</option>
</select>
```

**Fungsi setiap opsi:**
- `value=""` — Opsi default/placeholder, dengan teks "Pilih Program Studi". Value kosong akan gagal validasi karena controller mengecek `!program_studi`.
- `value="ti"`, `value="si"`, `value="it"` — Masing-masing mengirim kode pendek ke server, sementara teks yang ditampilkan ke pengguna adalah nama lengkap.

**Tampilan di tabel (list.hbs):**

```handlebars
<td>
    {{#if (eq program_studi "ti")}}Teknik Informatika (TI)
    {{else if (eq program_studi "it")}}Teknologi Informasi (IT)
    {{else if (eq program_studi "si")}}Sistem Informasi (SI)
    {{/if}}
</td>
```

**Fungsi:** Mengkonversi kode pendek (`ti`) menjadi label lengkap (`Teknik Informatika (TI)`) untuk tampilan. `{{else if}}` adalah sintaks Handlebars untuk percabangan multi-kondisi (menggunakan helper `eq`).

**Di form edit, mempertahankan pilihan:**

```handlebars
<option value="ti" {{#if (eq mahasiswa.program_studi "ti")}}selected{{/if}}>
    Teknik Informatika (TI)
</option>
```

**Fungsi:** Jika data mahasiswa saat ini memiliki `program_studi = "ti"`, atribut `selected` akan ditambahkan ke `<option>`, sehingga dropdown menampilkan pilihan yang sesuai dengan data tersimpan. Tanpa ini, dropdown akan selalu kembali ke opsi pertama.

---

## 8-9. Langkah 6-7 — Modul Dosen & Admin

Modul Dosen dan Admin mengikuti pola yang **identik** dengan modul-modul sebelumnya. Perbedaan hanya pada:

### Dosen vs Mahasiswa

| Aspek | Mahasiswa | Dosen |
|-------|-----------|-------|
| Field unik | `nim` (8-15 digit) | `nidn` (tepat 10 digit) |
| Regex NIM/NIDN | `/^\d{8,15}$/` | `/^\d{10}$/` |
| Kategori | `program_studi` (ti/si/it) | `departemen` (fish/fast) |
| Password default | NIM | NIDN |

### Admin vs Entitas Lain

| Aspek | Mahasiswa/Dosen | Admin |
|-------|-----------------|-------|
| Tabel | 2 tabel (pengguna + entitas) | 1 tabel (pengguna saja) |
| Model CREATE | INSERT pengguna → ambil ID → INSERT entitas | INSERT pengguna saja |
| Model READ | JOIN pengguna + entitas | SELECT pengguna WHERE peran='admin' |
| Password default | NIM/NIDN | email |

---

## 10. Langkah 8 — Entry Point Final

### `index.js` (versi final)

```javascript
const express = require('express');
const { engine } = require('express-handlebars');
const path = require('path');

// Import semua route
const matakuliahRoutes = require('./routes/matakuliahRoutes');
const mahasiswaRoutes = require('./routes/mahasiswaRoutes');
const dosenRoutes = require('./routes/dosenRoutes');
const adminRoutes = require('./routes/adminRoutes');

const app = express();
app.use(express.urlencoded({ extended: true }));

// Konfigurasi Handlebars
app.engine('hbs', engine({
    extname: 'hbs',
    defaultLayout: 'main',
    layoutsDir: path.join(__dirname, 'views/layouts'),
    helpers: {
        inc: (value) => parseInt(value) + 1,
        eq: (a, b) => a === b
    }
}));

app.set('view engine', 'hbs');
app.set('views', path.join(__dirname, 'views'));

// Static files — Bootstrap
app.use('/bootstrap',
    express.static(path.join(__dirname, 'node_modules/bootstrap/dist'))
);

// Halaman beranda
app.get('/', (req, res) => {
    res.render('pages/index');
});

// Mendaftarkan route modul
app.use("/mata-kuliah", matakuliahRoutes);
app.use("/mahasiswa", mahasiswaRoutes);
app.use("/dosen", dosenRoutes);
app.use("/admin", adminRoutes);

// Menjalankan server
app.listen(3000, () => {
    console.log('Server berjalan di http://localhost:3000');
});
```

**Alur request lengkap (contoh: `GET /mahasiswa/list`):**

```
1. Browser mengirim GET request ke http://localhost:3000/mahasiswa/list
2. Express menerima request
3. Middleware urlencoded dijalankan (tidak berpengaruh karena bukan POST)
4. Express mencocokkan path "/mahasiswa/list" dengan route yang terdaftar
5. Prefix "/mahasiswa" cocok dengan app.use("/mahasiswa", mahasiswaRoutes)
6. Express menghapus prefix, menyisakan "/list"
7. Di dalam mahasiswaRoutes, router.get('/list', ...) cocok
8. Controller MahasiswaController.listMahasiswa dipanggil dengan (req, res)
9. Controller memanggil MahasiswaModel.ambilSemuaMahasiswa()
10. Model menjalankan SELECT ... JOIN ... dan mengembalikan array data
11. Controller memanggil res.render('pages/mahasiswa/list', { mahasiswa })
12. Handlebars engine membaca views/pages/mahasiswa/list.hbs
13. Handlebars membungkus konten dengan layout views/layouts/main.hbs
14. {{{ body }}} diganti dengan hasil render list.hbs
15. Hasil HTML final dikirim sebagai HTTP response ke browser
16. Browser merender HTML yang diterima
```

---

## Ringkasan Pola Berulang

Setiap modul dalam tutorial ini mengikuti pola yang persis sama. Berikut adalah template yang bisa digunakan untuk menambah modul baru:

### Model (5 fungsi)

```
ambilSemua*    → SELECT ... → .all()
ambil*ById     → SELECT ... WHERE id = ? → .get(id)
buat*          → INSERT ... → .run(...)
update*        → UPDATE ... SET ... WHERE id = ? → .run(..., id)
hapus*         → DELETE ... WHERE id = ? → .run(id)
```

### Controller (6 fungsi)

```
validate*      → Array pesanError
showCreateForm → res.render('pages/x/create')
list*          → Model.ambilSemua*() → res.render('pages/x/list', { data })
showEditForm   → req.params.id → Model.ambil*ById(id) → res.render('pages/x/edit', { data })
create*        → req.body → validate → if error: render + return → Model.buat* → redirect
edit*          → req.body + req.params.id → validate → if error: render + return → Model.update* → redirect
delete*        → req.params.id → Model.hapus* → redirect
```

### Route (6 endpoint)

```
GET    /x/create        → showCreateForm
GET    /x/list          → list*
GET    /x/edit/:id      → showEditForm
POST   /x/create        → create*
POST   /x/edit/:id      → edit*
POST   /x/delete/:id    → delete*
```

### View (3 file per modul)

```
create.hbs  → form kosong + error display + formData preservation
list.hbs    → tabel data + empty state + edit/delete actions
edit.hbs    → form terisi + error display + selected dropdown
```

---

> **Dibuat untuk menjelaskan** `docs/TUTORIAL.md` &middot; **Proyek**: Web Absensi IBBI
