# Sistem Manajemen Cuti Akademik

Sistem manajemen cuti akademik berbasis REST API menggunakan CodeIgniter 4 Framework. Sistem ini dirancang untuk mengelola proses pengajuan dan manajemen cuti akademik mahasiswa dengan berbagai role pengguna.

## 📋 Daftar Isi
- [Fitur Utama](#fitur-utama)
- [Teknologi](#teknologi)
- [Persyaratan Sistem](#persyaratan-sistem)
- [Instalasi](#instalasi)
  - [Instalasi dengan Docker](#instalasi-dengan-docker)
  - [Instalasi Manual](#instalasi-manual)
- [Struktur Database](#struktur-database)
- [API Endpoints](#api-endpoints)
- [Role dan Hak Akses](#role-dan-hak-akses)
- [Alur Pengajuan Cuti](#alur-pengajuan-cuti)
- [Dokumentasi API](#dokumentasi-api)

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
- Dashboard Dosen
- Dashboard Admin
- Dashboard BAUP
- Dashboard Kajur
- Dashboard Koordinator Perpustakaan

### 4. Manajemen Data
- Pengelolaan data mahasiswa
- Pengelolaan data dosen
- Pengelolaan data kajur
- Pengelolaan data admin
- Pengelolaan data BAUP

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
- PHP >= 8.1
- MySQL/MariaDB
- Composer
- Apache/Nginx
- Extensions PHP:
  - intl
  - json
  - mbstring
  - mysqlnd
  - xml

## 📥 Instalasi

### Instalasi dengan Docker

1. Clone repository
```bash
git clone [repository-url]
cd backend
```

2. Setup Environment
```bash
cp env .env
ganti .env , hostname jadi db
ganti app, config, database.php jadi db 
(yang tadinya itu localhost atau 127.0.0.1 buat testing)
```
Edit file .env dan sesuaikan konfigurasi database untuk Docker:
```env
database.default.hostname = db    # Nama service MySQL di docker-compose
database.default.database = cuti
database.default.username = root
database.default.password = root123
database.default.port = 3306
```

3. Build dan Jalankan Docker Container
```bash
# Build dan jalankan container
docker-compose build --no-cache
docker-compose up -d

# Cek status container
docker ps
```

4. Import Database (jika ada)
```bash
# Copy file SQL ke container
docker cp ./cuti.sql scuti-db:/cuti.sql
docker cp cuti.sql scuti-db:/cuti.sql 
(sama kaya diatas)

# Import database
docker exec -it scuti-db mysql -uroot -proot123 cuti < cuti.sql
docker exec -i scuti-db mysql -uroot cuti < cuti.sql (yang ga pake passwd)
docker exec -i scuti-db mysql -h localhost -uroot cuti < cuti.sql

#jalanin
docker compose up -d
------
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

5. Verifikasi Instalasi
- Web Server: http://localhost:8080 (Default CodeIgniter welcome page)
- API Endpoint: http://localhost:8080/mahasiswa (atau endpoint lainnya)

#### Struktur Docker
Project ini menggunakan 3 container Docker:
1. **PHP-FPM (scuti-app)**
   - Base image: php:8.3-fpm
   - Extensions: mysqli, pdo_mysql, mbstring, exif, pcntl, bcmath, gd, intl
   - Composer untuk dependency management

2. **Nginx (scuti-nginx)**
   - Base image: nginx:alpine
   - Port: 8080:80
   - Serves sebagai web server

3. **MySQL (scuti-db)**
   - Base image: mysql:8.0
   - Port: 3307:3306 (diubah untuk menghindari konflik)
   - Credentials default:
     - Database: cuti
     - Username: root
     - Password: 

#### Docker Commands yang Sering Digunakan
```bash
# Start containers
docker-compose up -d

# Stop containers
docker-compose down

# Lihat logs
docker logs scuti-app    # PHP logs
docker logs scuti-nginx  # Nginx logs
docker logs scuti-db     # MySQL logs

# Masuk ke container
docker exec -it scuti-app bash
docker exec -it scuti-db bash

# Rebuild container (setelah mengubah Dockerfile)
docker-compose build --no-cache
docker-compose up -d

# Hapus semua container dan volume
docker-compose down -v
```

#### Troubleshooting Docker
1. **Port Conflicts**
   - Error: "port is already allocated"
   - Solusi: Ubah port mapping di docker-compose.yml

2. **MySQL Connection Issues**
   - Error: "MySQL extension not loaded"
   - Solusi: Pastikan extension mysqli sudah terinstall di container PHP

3. **Permission Issues**
   - Error: "Permission denied"
   - Solusi: Periksa ownership files di container (www-data:www-data)

### Instalasi Manual

1. Clone repository
```bash
git clone [repository-url]
cd backend
```

2. Install dependencies
```bash
composer install
```

3. Konfigurasi environment
```bash
cp env .env
```
Edit file .env dan sesuaikan konfigurasi database:
```env
database.default.hostname = localhost
database.default.database = [nama_database]
database.default.username = [username]
database.default.password = [password]
```

4. Migrasi database
```bash
php spark migrate
php spark serve (dipake)
```

5. Jalankan seeder (opsional)
```bash
php spark db:seed InitialSeeder
```

## 🗄️ Struktur Database

### Tabel Utama
1. users
   - id_user (PK)
   - username
   - role
   - status

2. mahasiswa
   - npm (PK)
   - id_user (FK)
   - nama_mahasiswa
   - id_dosen (FK)
   - id_kajur (FK)

3. cuti
   - id_cuti (PK)
   - npm (FK)
   - tanggal_pengajuan
   - status_cuti

[Detail tabel lainnya...]

## 🔌 API Endpoints

### Autentikasi
- POST /user - Register user baru
- GET /user - Get semua user
- GET /user/{id} - Get user by ID
- PUT /user/{id} - Update user
- DELETE /user/{id} - Delete user

### Mahasiswa
- POST /mahasiswa - Tambah mahasiswa
- GET /mahasiswa - Get semua mahasiswa
- GET /mahasiswa/{npm} - Get mahasiswa by NPM
- PUT /mahasiswa/{npm} - Update mahasiswa
- DELETE /mahasiswa/{npm} - Delete mahasiswa

### Cuti
- POST /cuti - Pengajuan cuti
- GET /cuti - Get semua data cuti
- GET /cuti/{id} - Get cuti by ID
- PUT /cuti/{id} - Update status cuti
- DELETE /cuti/{id} - Delete cuti

[Detail endpoint lainnya...]

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
