# Tutorial Mengaktifkan GitHub Pages

## Apa itu GitHub Pages?

GitHub Pages adalah layanan hosting statis gratis yang disediakan oleh GitHub. Dengan GitHub Pages, Anda dapat meng-host website langsung dari repository GitHub Anda. Cocok untuk portfolio, dokumentasi proyek, blog, atau landing page.

## Keuntungan Menggunakan GitHub Pages

- **Gratis** - Hosting tanpa biaya
- **Mudah** - Langsung terintegrasi dengan repository GitHub
- **Custom Domain** - Mendukung domain kustom
- **HTTPS** - Keamanan SSL otomatis
- **Version Control** - Semua perubahan tercatat di Git

## Langkah-langkah Mengaktifkan GitHub Pages

### 1. Persiapan Repository

Pastikan Anda sudah memiliki repository di GitHub dengan file HTML, Markdown, atau file statis lainnya yang ingin dipublikasikan.

### 2. Masuk ke Pengaturan Repository

1. Buka repository Anda di GitHub
2. Klik tab **Settings** di bagian atas halaman repository
3. Scroll ke bawah hingga menemukan menu **Pages** di sidebar sebelah kiri
4. Klik menu **Pages**

### 3. Mengaktifkan GitHub Pages

Di halaman GitHub Pages, Anda akan melihat beberapa pengaturan:

#### **A. Pilih Source (Sumber)**

Ada beberapa opsi untuk memilih sumber GitHub Pages:

**Opsi 1: Deploy from a branch (Dari Branch)**

- Pilih branch yang akan digunakan (biasanya `main` atau `master`)
- Pilih folder:
  - **/ (root)** - Menggunakan file dari root repository
  - **/docs** - Menggunakan file dari folder `docs`
- Klik tombol **Save**

**Opsi 2: GitHub Actions**

- Pilih opsi **GitHub Actions** untuk deployment yang lebih fleksibel
- Cocok untuk static site generator seperti Jekyll, Hugo, Next.js, dll.

#### **B. Konfigurasi Branch (Jika Menggunakan Opsi 1)**

1. Pada dropdown **Branch**, pilih branch yang ingin digunakan (contoh: `main`)
2. Pada dropdown **Folder**, pilih folder sumber:
   - Pilih **/ (root)** jika file website ada di root repository
   - Pilih **/docs** jika file website ada di folder docs
3. Klik tombol **Save**

### 4. Tunggu Proses Deployment

Setelah menyimpan pengaturan:

- GitHub akan memproses dan mendeploy website Anda
- Proses ini biasanya memakan waktu beberapa menit
- Anda akan melihat notifikasi di bagian atas halaman

### 5. Akses Website Anda

Setelah deployment selesai, GitHub akan menampilkan URL website Anda:

```
https://username.github.io/repository-name/
```

atau untuk repository user/organization:

```
https://username.github.io/
```

Klik URL tersebut untuk melihat website Anda yang sudah live!

## Struktur File yang Direkomendasikan

### Untuk Website Biasa

```
repository/
├── index.html          # Halaman utama
├── about.html
├── css/
│   └── style.css
├── js/
│   └── script.js
└── images/
    └── logo.png
```

### Untuk Blog dengan Jekyll

```
repository/
├── _config.yml         # Konfigurasi Jekyll
├── index.md           # Halaman utama
├── _posts/            # Folder untuk posts
│   └── 2026-03-31-judul-post.md
├── _layouts/          # Template layouts
└── assets/            # CSS, JS, images
```

## Menggunakan Custom Domain

Jika Anda memiliki domain sendiri:

### 1. Tambahkan Custom Domain di GitHub

1. Di halaman **Settings > Pages**
2. Pada bagian **Custom domain**, masukkan nama domain Anda (contoh: `www.namadomain.com`)
3. Klik **Save**
4. GitHub akan membuat file `CNAME` di repository Anda

### 2. Konfigurasi DNS

Di provider domain Anda, tambahkan DNS record:

**Untuk subdomain (www atau blog):**

```
Type: CNAME
Name: www (atau subdomain lain)
Value: username.github.io
```

**Untuk apex domain (tanpa www):**

```
Type: A
Name: @
Value: 185.199.108.153
Value: 185.199.109.153
Value: 185.199.110.153
Value: 185.199.111.153
```

### 3. Aktifkan HTTPS

Setelah DNS terkonfigurasi (tunggu 24-48 jam):

1. Kembali ke **Settings > Pages**
2. Centang opsi **Enforce HTTPS**

## Tips dan Best Practices

### 1. Gunakan File index.html atau index.md

GitHub Pages secara otomatis mencari file `index.html` atau `index.md` sebagai halaman utama.

### 2. Perhatikan Case Sensitivity

URL di GitHub Pages bersifat case-sensitive. Gunakan huruf kecil untuk nama file dan folder.

### 3. Gunakan Relative Path

Gunakan relative path untuk link dan asset agar website tetap berfungsi saat pindah domain.

```html
<!-- Baik -->
<link rel="stylesheet" href="css/style.css" />
<img src="images/logo.png" />

<!-- Hindari -->
<link rel="stylesheet" href="https://username.github.io/repo/css/style.css" />
```

### 4. File .nojekyll

Jika Anda tidak menggunakan Jekyll dan memiliki folder yang dimulai dengan underscore (\_), buat file `.nojekyll` di root repository untuk menonaktifkan Jekyll processing.

### 5. Gunakan 404.html Custom

Buat file `404.html` di root untuk halaman error yang lebih friendly.

## Troubleshooting

### Website Tidak Muncul

- Cek apakah ada file `index.html` atau `index.md` di root atau folder yang dipilih
- Pastikan branch dan folder sudah benar
- Tunggu beberapa menit untuk proses deployment

### CSS/JS Tidak Ter-load

- Periksa path CSS dan JS (gunakan relative path)
- Cek case sensitivity nama file
- Lihat console browser (F12) untuk error

### Perubahan Tidak Muncul

- Tunggu beberapa menit setelah push
- Clear cache browser (Ctrl + Shift + R)
- Cek tab **Actions** di repository untuk melihat status deployment

## Mematikan GitHub Pages

Jika ingin mematikan GitHub Pages:

1. Buka **Settings > Pages**
2. Pada bagian **Source**, pilih **None**
3. Klik **Save**

Website Anda akan tidak lagi dapat diakses.

## Kesimpulan

GitHub Pages adalah solusi hosting yang sempurna untuk website statis. Dengan mengikuti tutorial ini, Anda dapat dengan mudah meng-host website Anda secara gratis dengan hanya beberapa klik. Selamat mencoba!

## Referensi

- [Dokumentasi Resmi GitHub Pages](https://docs.github.com/en/pages)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Custom Domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
