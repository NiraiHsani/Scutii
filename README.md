# Sistem Manajemen Cuti Akademik

Sistem manajemen cuti akademik berbasis REST API menggunakan CodeIgniter 4 Framework. Sistem ini dirancang untuk mengelola proses pengajuan dan manajemen cuti akademik mahasiswa dengan berbagai role pengguna.

## 🚀 Fitur Utama

### 1. Manajemen User
- Multi-role system (Admin, BAUP, Dosen, Mahasiswa, Kajur, Koordinator Perpustakaan)
- Autentikasi dan autorisasi berbasis role
- Manajemen profil pengguna

### 2. Sistem Cuti Akademik
- Pengajuan cuti mahasiswa
- Validasi satu cuti per mahasiswa
- Tracking status pengajuan
- Riwayat pengajuan cuti

### 3. Dashboard per Role
- Dashboard Mahasiswa
- Dashboard Admin
- Dashboard Kajur

### 4. Manajemen Data
- Pengelolaan data mahasiswa
- Pengelolaan data kajur
- Pengelolaan data admin

## 💻 Teknologi

- PHP 8.3
- CodeIgniter 4.x
- MySQL 8.0
- Docker & Docker Compose
- Nginx
- REST API
- CORS enabled

## ⚙️ Persyaratan Sistem

### Untuk Pengembangan dengan Docker
- Docker Desktop
- Git
- Text Editor (VS Code, dll)
- Postman (untuk testing API)

### Untuk Instalasi Manual
- PHP >= 8.3
- MySQL/MariaDB
- Composer
- Apache/Nginx
- Extensions PHP:
  - intl
  - json
  - mbstring
  - mysqlnd
  - xml

## 📁 Struktur Folder

```bash
scutii/
│
├── backend_/                   
│   └── (isi file PHP & logic backend)
│
├── docker/
│   ├── nginx/
│   │   └── conf.d/
│   │       └── backend.conf
            └── frontend.conf 
│   └── php/
│       └── local.ini  
│       └── www.conf 
│
├── frontend/                    
│   └── (HTML/CSS/JS dll)
│
├── docker-compose.yml            # File utama untuk mengelola container
├── Dockerfile                    # Build image PHP backend
└── README.md
```
# 📦 Panduan Instalasi Docker di Windows 10/11

Panduan ini menjelaskan langkah-langkah instalasi Docker Desktop di Windows dari awal hingga selesai, termasuk aktivasi fitur Windows yang diperlukan seperti **WSL** dan **Virtual Machine Platform**.

---

## ✅ Persyaratan Sistem

- Sistem Operasi: Windows 10 64-bit (build 19044+) atau Windows 11
- CPU: Mendukung virtualisasi (Intel VT-x atau AMD-V)
- RAM: Minimal 4 GB
- Akun Windows dengan akses administrator
- Terhubung ke internet

---

## 🧰 1. Aktifkan Fitur Windows (WSL & Virtual Machine Platform)

1. Buka **Control Panel** atau tekan `Windows + S`, lalu cari **Turn Windows features on or off**.
2. Centang opsi berikut:
   - ✅ **Virtual Machine Platform**
   - ✅ **Windows Subsystem for Linux**

3. Klik **OK**, tunggu proses selesai.
4. Restart komputer jika diminta.

---

## 🔽 2. Unduh Docker Desktop

1. Kunjungi: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
2. Klik tombol **Download for Windows (WSL2)**.
3. Simpan dan jalankan installer (biasanya bernama `Docker Desktop Installer.exe`).

---

## ⚙️ 3. Jalankan Installer Docker Desktop

1. Klik dua kali file installer.
2. Pastikan opsi berikut dicentang:
   - ✅ **Use WSL 2 instead of Hyper-V (recommended)**
   - ✅ **Add shortcut to desktop** (opsional)

3. Klik **OK** dan tunggu hingga proses instalasi selesai.
4. Setelah selesai, klik **Close and restart** jika diminta.

---

## 📦 4. Setup Docker Pertama Kali

1. Setelah restart, buka **Docker Desktop**.
2. Docker akan melakukan konfigurasi awal. Tunggu hingga muncul tulisan:  
   ✅ **Docker Desktop is running**
3. Jika diminta login, bisa lewati atau daftar akun Docker (opsional).

---

## 📥 5. Uji Coba Instalasi

Buka **Command Prompt (CMD)** atau **Windows Terminal**, lalu jalankan perintah:

```bash
docker --version

## 📥 Panduan Install di folder.

link drive folder scuti = https://drive.google.com/drive/folders/1iNvVx447Tw7R1VreRIOydmBVeH-Neqjb 

### Instalasi dengan Docker

1. Clone repository
```bash
git clone [repository-url] backend

git clone [repo-url] frontend
```

2. Setup Environment di backend
```bash
cp env .env
ganti .env , hostname jadi [db]
ganti app, config, database.php jadi [db] 
#yang tadinya itu localhost atau 127.0.0.1 buat testing
```
Edit file .env di backend dan sesuaikan konfigurasi database untuk Docker:
```bash
database.default.hostname = db    # Nama service MySQL di docker-compose
database.default.database = cuti
database.default.username = root
database.default.password = 
database.default.port = 3306

# ! di dalam backend dan frontend harus ada file [.env]
# agar bisa kebaca buat build dockernya
```
3. Tambahkan File - file pendukung docker berupa :
   1. folder docker\ yang berisi folder php, nginx dan mysql
   2. file Dockerfile\
   3. file docker-compose.yml\
```bash
# bisa lihat di drive lengkap yang sudah disediakan
```

3.1. Build dan Jalankan Docker Container
```bash
# Build dan jalankan container
docker-compose build --no-cache
docker-compose up -d

# Cek status container
docker ps
```

4. Import Database (Opsional, jika tidak menggunakan phpMyAdmin)
```bash
# Copy file SQL ke container
docker cp ./cuti.sql scuti-db:/cuti.sql
docker cp cuti.sql scuti-db:/cuti.sql 
(sama kaya diatas)

# Import database
docker exec -it scuti-db mysql -uroot -proot123 cuti < cuti.sql
docker exec -i scuti-db mysql -uroot cuti < cuti.sql (yang ga pake passwd)
docker exec -i scuti-db mysql -h localhost -uroot cuti < cuti.sql 9versi lain)

# Masuk ke SQL (opsional)
docker exec -it scuti-db mysql -uroot cuti
# nunjukin table
show tables;
# keluar dari sql
exit;

# Restart docker
docker compose restart
# Cek Container yang berjalan
docker ps
```

## 🚀 Verifikasi Instalasi

- **Frontend Laravel**: `http://localhost:8082`
- **Backend CodeIgniter 4**: `http://localhost:8080`
- **API Endpoint**: `http://localhost:8080/mahasiswa`
- **phpMyAdmin**: `http://localhost:8081`

---

## ⚙️ Struktur Docker

Proyek ini menggunakan 3 container utama:

1. **Backend (scuti-app)**
   - Base Image: `php:8.3-fpm`
   - Framework: CodeIgniter 4
   - Ekstensi: `mysqli`, `pdo_mysql`, `mbstring`, `exif`, `pcntl`, `bcmath`, `gd`, `intl`
   - Composer untuk dependency management

2. **Frontend (scuti-frontend)**
   - Base Image: `php:8.3-fpm`
   - Framework: Laravel
   - Berinteraksi dengan backend via HTTP

3. **Nginx (scuti-nginx)**
   - Base Image: `nginx:alpine`
   - Port: `8080:80` (backend) dan `8082:80` (frontend)

4. **MySQL (scuti-db)**
   - Base Image: `mysql:8.0`
   - Port: `3307:3306`
   - Default Credentials:
     - Database: `cuti`
     - Username: `root`
     - Password: *(kosong)*

5. **phpMyAdmin**
   - Port: `8081:80`
   - Terhubung ke database `scuti-db`

---

## 🐳 Docker Commands Umun

```bash
# Start semua container
docker-compose up -d

# Stop semua container
docker-compose down

# Build ulang container setelah ubah Dockerfile
docker-compose build --no-cache
docker-compose up -d

# Masuk ke container
docker exec -it scuti-app bash
docker exec -it scuti-db bash

# Cek logs container
docker logs scuti-app       # Backend CI logs
docker logs scuti-nginx     # Nginx logs
docker logs scuti-db        # MySQL logs

# Hapus container dan volume
docker-compose down -v
```

---

## 🛠️ Manual Instalasi (Opsional)

### Backend (CodeIgniter 4)

```bash
git clone [repo-backend]
cd backend
composer install
cp env .env
```

Edit file `.env`:

```env
database.default.hostname = db
database.default.database = cuti
database.default.username = root
database.default.password =
```

Lalu jalankan:

```bash
php spark serve
```

---

### Frontend (Laravel)

```bash
git clone [repo-frontend]
cd frontend
composer install
cp .env.example .env
php artisan key:generate
```

Edit file `.env`:

```env
VITE_API_URL=http://localhost:8080
```

Lalu jalankan:

```bash
php artisan serve
```

---

## 🧾 Struktur Database

**Database: `cuti`**

### Tabel Utama

- `user`
  - `id_user` (PK)
  - `username`
  - `role` (admin/kajur/mahasiswa)
  - `status`

- `admin`
  - `id_admin` (PK)
  - `id_user` (FK)

- `mahasiswa`
  - `npm` (PK)
  - `id_user` (FK)
  - `nama_mahasiswa`
  - `id_kajur` (FK)

- `kajur`
  - `id_kajur` (PK)
  - `id_user` (FK)

- `cuti`
  - `id_cuti` (PK)
  - `npm` (FK)
  - `tanggal_pengajuan`
  - `status_cuti`

---

## 📡 API Endpoints

### Autentikasi

```http
POST    /user             # Tambah user
GET     /user             # Ambil semua user
GET     /user/{id}        # Detail user
PUT     /user/{id}        # Update user
DELETE  /user/{id}        # Hapus user
```

### Mahasiswa

```http
POST    /mahasiswa
GET     /mahasiswa
GET     /mahasiswa/{npm}
PUT     /mahasiswa/{npm}
DELETE  /mahasiswa/{npm}
```

### Cuti

```http
POST    /cuti
GET     /cuti
GET     /cuti/{id}
PUT     /cuti/{id}
DELETE  /cuti/{id}
```

---

## 🧩 View Pendukung

Digunakan untuk menyederhanakan query dan tampilan data di frontend.

- `view_beranda_mahasiswa`
- `view_beranda_kajur`
- `view_riwayat_admin`

---

> 🔧 Sistem ini cocok untuk simulasi pengajuan cuti mahasiswa dengan role yang berbeda (admin, kajur, mahasiswa) dan terintegrasi penuh antara backend dan frontend berbasis Docker.


## 👥 Role dan Hak Akses

### 1. Mahasiswa
- Mengajukan cuti
- Melihat status cuti
- Melihat riwayat cuti
- Update profil

### 2. Dosen
- Melihat data mahasiswa
- Validasi pengajuan cuti
- Melihat riwayat cuti mahasiswa

### 3. Admin
- Manajemen user
- Manajemen data master
- Monitoring sistem

[Detail role lainnya...]

## 📝 Alur Pengajuan Cuti

1. Mahasiswa login dan mengajukan cuti
2. Sistem validasi (cek kuota cuti)
3. Dosen pembimbing validasi
4. Kajur validasi
5. BAUP validasi
6. Status cuti diupdate
7. Notifikasi ke mahasiswa

## 📚 Dokumentasi API

### Format Response

#### Success Response
```json
{
    "status": 200,
    "error": null,
    "message": {
        "success": "Pesan sukses"
    },
    "data": {}
}
```

#### Error Response
```json
{
    "status": 400,
    "error": "Pesan error",
    "messages": {}
}
```

### Validasi
- NPM unik per mahasiswa
- Satu mahasiswa satu cuti aktif
- Cross-check data antar tabel
- Validasi role dan akses

## 🔒 Keamanan

- CORS configuration
- Input validation
- Role-based access control
- Data integrity checks
- Error handling

## 🤝 Kontribusi

Silakan berkontribusi dengan membuat pull request atau melaporkan issue.

## 📄 Lisensi

[MIT License](LICENSE)

## 👨‍💻 Developer

[Nama Developer/Tim]

## 📞 Kontak

Untuk informasi lebih lanjut, hubungi:
- Email: [email]
- Website: [website]


