# Tutorial Git dan GitHub untuk Pemula

## Daftar Isi

- [Apa itu Git?](#apa-itu-git)
- [Apa itu GitHub?](#apa-itu-github)
- [Instalasi Git](#instalasi-git)
- [Konfigurasi Awal Git](#konfigurasi-awal-git)
- [Perintah Dasar Git](#perintah-dasar-git)
- [Membuat Repository di GitHub](#membuat-repository-di-github)
- [Menghubungkan Repository Lokal dengan GitHub](#menghubungkan-repository-lokal-dengan-github)
- [Workflow Git yang Umum](#workflow-git-yang-umum)
- [Branching dan Merging](#branching-dan-merging)
- [Tips dan Best Practices](#tips-dan-best-practices)

---

## Apa itu Git?

**Git** adalah sistem kontrol versi (_version control system_) yang digunakan untuk melacak perubahan pada file dan kode sumber selama pengembangan perangkat lunak. Git memungkinkan banyak developer untuk bekerja pada proyek yang sama secara bersamaan tanpa saling menimpa pekerjaan satu sama lain.

### Keuntungan Menggunakan Git:

- **Tracking perubahan**: Melihat siapa yang membuat perubahan, kapan, dan apa yang diubah
- **Kolaborasi**: Bekerja dalam tim dengan efisien
- **Backup**: Setiap perubahan tersimpan dalam histori
- **Branching**: Membuat cabang untuk fitur baru tanpa mengganggu kode utama
- **Rollback**: Kembali ke versi sebelumnya jika ada kesalahan

---

## Apa itu GitHub?

**GitHub** adalah platform hosting untuk repository Git yang berbasis cloud. GitHub menyediakan interface web yang memudahkan kolaborasi, code review, dan manajemen proyek.

### Fitur Utama GitHub:

- **Repository hosting**: Menyimpan kode Anda di cloud
- **Pull requests**: Mengajukan perubahan dan melakukan code review
- **Issues**: Melacak bug dan fitur yang akan dikembangkan
- **GitHub Pages**: Hosting website statis gratis
- **GitHub Actions**: Otomasi CI/CD
- **Collaboration tools**: Wiki, project boards, discussions

---

## Instalasi Git

### Windows

1. Download Git dari [git-scm.com](https://git-scm.com/)
2. Jalankan installer dan ikuti wizard instalasi
3. Pilih opsi default atau sesuaikan dengan kebutuhan
4. Setelah instalasi selesai, buka **Git Bash** atau **Command Prompt**

### Mac

```bash
# Menggunakan Homebrew
brew install git

# Atau download dari git-scm.com
```

### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git
```

### Verifikasi Instalasi

```bash
git --version
```

Jika muncul versi Git, berarti instalasi berhasil.

---

## Konfigurasi Awal Git

Setelah menginstal Git, lakukan konfigurasi identitas Anda:

```bash
# Set nama Anda
git config --global user.name "Nama Anda"

# Set email Anda
git config --global user.email "email@example.com"

# Verifikasi konfigurasi
git config --list
```

**Catatan**: Email yang digunakan sebaiknya sama dengan email yang terdaftar di GitHub.

---

## Perintah Dasar Git

### 1. Inisialisasi Repository

```bash
# Membuat folder baru untuk proyek
mkdir my-project
cd my-project

# Inisialisasi Git repository
git init
```

### 2. Melihat Status

```bash
# Melihat status file (modified, staged, untracked)
git status
```

### 3. Menambahkan File ke Staging Area

```bash
# Menambahkan file tertentu
git add nama-file.txt

# Menambahkan semua file
git add .

# Menambahkan file dengan ekstensi tertentu
git add *.js
```

### 4. Commit Perubahan

```bash
# Commit dengan pesan
git commit -m "Pesan commit yang deskriptif"

# Commit semua perubahan (skip staging)
git commit -am "Pesan commit"
```

### 5. Melihat Histori Commit

```bash
# Melihat log commit
git log

# Log dengan format ringkas
git log --oneline

# Log dengan grafik branch
git log --graph --oneline --all
```

### 6. Melihat Perubahan

```bash
# Melihat perubahan yang belum di-stage
git diff

# Melihat perubahan yang sudah di-stage
git diff --staged
```

---

## Membuat Repository di GitHub

1. **Login ke GitHub** di [github.com](https://github.com/)
2. Klik tombol **"+"** di kanan atas, pilih **"New repository"**
3. Isi informasi repository:
   - **Repository name**: Nama repository Anda
   - **Description**: Deskripsi singkat (opsional)
   - **Public/Private**: Pilih visibilitas repository
   - **Initialize repository**: Pilih opsi sesuai kebutuhan
     - `README.md`: File dokumentasi
     - `.gitignore`: File untuk mengabaikan file tertentu
     - `License`: Lisensi proyek
4. Klik **"Create repository"**

---

## Menghubungkan Repository Lokal dengan GitHub

### Opsi 1: Repository Lokal yang Sudah Ada

Jika Anda sudah memiliki repository lokal dan ingin menghubungkannya dengan GitHub:

```bash
# Tambahkan remote repository
git remote add origin https://github.com/username/nama-repo.git

# Verifikasi remote
git remote -v

# Push ke GitHub
git push -u origin main
```

**Catatan**: Ganti `username` dan `nama-repo` dengan username GitHub Anda dan nama repository yang telah dibuat.

### Opsi 2: Clone Repository dari GitHub

Jika repository sudah ada di GitHub dan Anda ingin clone ke lokal:

```bash
# Clone repository
git clone https://github.com/username/nama-repo.git

# Masuk ke folder repository
cd nama-repo
```

---

## Workflow Git yang Umum

### 1. Alur Kerja Dasar

```bash
# 1. Cek status
git status

# 2. Tambahkan file yang diubah
git add .

# 3. Commit perubahan
git commit -m "Deskripsi perubahan"

# 4. Push ke GitHub
git push origin main
```

### 2. Mengambil Perubahan dari GitHub

```bash
# Fetch: Melihat perubahan tanpa merge
git fetch origin

# Pull: Mengambil dan merge perubahan
git pull origin main
```

### 3. Mengatasi Konflik

Jika terjadi konflik saat merge atau pull:

```bash
# 1. Git akan menandai file yang konflik
# 2. Buka file tersebut dan edit manual
# 3. Setelah selesai, tambahkan file
git add nama-file-konflik.txt

# 4. Lanjutkan merge
git commit -m "Resolved merge conflict"

# 5. Push perubahan
git push origin main
```

---

## Branching dan Merging

### Apa itu Branch?

Branch adalah cabang terpisah dari kode utama yang memungkinkan Anda mengembangkan fitur baru tanpa mengganggu kode di branch utama.

### Perintah Branch

```bash
# Melihat daftar branch
git branch

# Membuat branch baru
git branch nama-branch

# Berpindah ke branch lain
git checkout nama-branch

# Membuat dan langsung pindah ke branch baru
git checkout -b nama-branch

# Menghapus branch
git branch -d nama-branch
```

### Merging Branch

```bash
# 1. Pindah ke branch tujuan (biasanya main)
git checkout main

# 2. Merge branch fitur
git merge nama-branch

# 3. Push hasil merge ke GitHub
git push origin main
```

### Workflow dengan Branch

```bash
# 1. Buat branch untuk fitur baru
git checkout -b fitur-login

# 2. Kerjakan fitur
# ... edit file ...

# 3. Commit perubahan di branch fitur
git add .
git commit -m "Menambahkan fitur login"

# 4. Push branch ke GitHub
git push origin fitur-login

# 5. Buat Pull Request di GitHub

# 6. Setelah di-review, merge ke main
git checkout main
git merge fitur-login

# 7. Push main branch
git push origin main

# 8. Hapus branch fitur (opsional)
git branch -d fitur-login
```

---

## Tips dan Best Practices

### 1. Commit Message yang Baik

```bash
# ❌ Bad
git commit -m "update"
git commit -m "fix bug"

# ✅ Good
git commit -m "Add user authentication feature"
git commit -m "Fix navbar responsive issue on mobile"
git commit -m "Update README with installation instructions"
```

### 2. Commit Secara Berkala

- Commit perubahan kecil dan sering, bukan perubahan besar sekaligus
- Setiap commit sebaiknya merepresentasikan satu perubahan logis

### 3. Gunakan .gitignore

Buat file `.gitignore` untuk mengabaikan file yang tidak perlu di-track:

```
# Node modules
node_modules/

# Environment variables
.env

# Build files
dist/
build/

# OS files
.DS_Store
Thumbs.db

# IDE settings
.vscode/
.idea/
```

### 4. Pull Sebelum Push

Selalu lakukan `git pull` sebelum `git push` untuk menghindari konflik:

```bash
git pull origin main
git push origin main
```

### 5. Gunakan Branch untuk Fitur Baru

- `main` atau `master`: Branch utama yang stabil
- `develop`: Branch untuk pengembangan
- `feature/nama-fitur`: Branch untuk fitur baru
- `bugfix/nama-bug`: Branch untuk perbaikan bug

### 6. Lindungi Branch Utama

Di GitHub, aktifkan **branch protection rules** untuk branch `main`:

- Require pull request reviews
- Require status checks to pass
- Prevent force push

---

## Perintah Git Lainnya yang Berguna

```bash
# Membatalkan perubahan yang belum di-commit
git restore nama-file.txt

# Unstage file
git restore --staged nama-file.txt

# Kembali ke commit sebelumnya
git reset --hard HEAD~1

# Membuat tag untuk rilis
git tag v1.0.0
git push origin v1.0.0

# Melihat remote repository
git remote -v

# Mengubah URL remote
git remote set-url origin https://github.com/username/repo-baru.git

# Stash: Menyimpan perubahan sementara
git stash
git stash pop
git stash list
```

---

## Kesimpulan

Git dan GitHub adalah tools yang sangat penting dalam pengembangan software modern. Dengan menguasai perintah-perintah dasar dan workflow yang tepat, Anda dapat:

- ✅ Mengelola kode dengan efektif
- ✅ Berkolaborasi dengan tim
- ✅ Melacak setiap perubahan
- ✅ Menghindari kehilangan kode
- ✅ Deploy dan share proyek dengan mudah

Teruslah berlatih dan jangan takut untuk bereksperimen. Semua developer pasti pernah mengalami kesalahan dengan Git, yang penting adalah belajar dari kesalahan tersebut!

---

## Referensi dan Sumber Belajar

- [Git Official Documentation](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Learn Git Branching (Interactive)](https://learngitbranching.js.org/)
- [GitHub Learning Lab](https://lab.github.com/)

---

**Happy Coding! 🚀**
