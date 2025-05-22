# 📦 Penilaian Sumatif Akhir Tahun

## 🛠️ Mapil: DevOps - Penilaian Praktek

### 🡩‍🏫 Kelas: XI TJKT 1 – SMKN 1 Banyumas

### 📅 Tahun Ajaran: 2024/2025

---

## 📁 Cara Deploy Aplikasi Otomatis Menggunakan `.env` dan `otomatis.sh`

### Apa Itu File `.env`?

File `.env` (environment) digunakan untuk menyimpan konfigurasi yang bersifat rahasia dan tidak boleh dituliskan langsung di dalam kode program, seperti:

* Nama database
* Username & password database
* Alamat host (server)

**Contoh isi file **\`\`**:**

```
DB_USER=admin
DB_PASS=P4ssw0rd123
DB_NAME=psat2425
DB_HOST=rdsku.czt6n8ylfvyb.us-east-1.rds.amazonaws.com
```

File ini mirip seperti `config.php`, tapi lebih terstandarisasi dan aman.

---

## 🔧 PERSIAPAN

### 1. Buat Repository GitHub

Buat repositori baru untuk menyimpan source code aplikasi.

### 2. Buka Folder Proyek

Gunakan **VS Code** atau editor lainnya dan masuk ke folder proyek kamu.

### 3. Tambahkan File `.env` Kosong

### 4. Buat File `.gitignore`

File `.gitignore` berfungsi untuk memberitahu Git agar tidak menyertakan file tertentu saat di-*commit*.

**Contoh isi **\`\`**:**

```
.env                  # Abaikan file .env agar tidak ikut ter-push
uploads/*             # Abaikan semua isi folder uploads
!uploads/             # Kecuali folder uploads-nya
```

### 5. Push Proyek ke GitHub

```bash
git init
git remote add origin https://github.com/username/repo
 contoh : git remote add origin https://github.com/thoriq-max/psat2425
git add .
git commit -m "first commit"
git branch -M main
git push -u origin main
```

### 6. Buat Database di AWS (Aurora & RDS)

Langkah-langkah:

1. Buka **RDS > Create database**
2. Pilih **Engine: MySQL**
3. Template: **Free Tier**
4. Isi DB name, username, password
5. Public access: **No**
6. Tambahkan **security group** dengan port 3306 terbuka

---

## 🚀 CARA 1: Otomatis Lewat User Data AWS

### 1. Launch EC2 Instance

* OS: **Ubuntu**
* Type: **t2.micro**
* Key Pair: `vockey`
* Security Group:

  * Allow SSH (22)
  * HTTP (80)
  * HTTPS (443)
  * IP: `0.0.0.0/0`

### 2. Tambahkan Script Otomatis di "User Data"

```bash
#!/bin/bash
echo '#!/bin/bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
sudo rm -rf /var/www/html/{,.}
sudo git clone [repository githubmu] /var/www/html
sudo chmod -R 777 /var/www/html
echo DB_USER=[username rds] > /var/www/html/.env
echo DB_PASS=[password rds]  >> /var/www/html/.env
echo DB_NAME=[nama database]  >> /var/www/html/.env
echo DB_HOST=[endpoint rds] >> /var/www/html/.env
sudo apt install -y openssl
sudo a2enmod ssl
sudo a2ensite default-ssl.conf
sudo systemctl reload apache2'

chmod +x /home/ubuntu/otomatis.sh  
```

### 3. Akses EC2 dan Jalankan Script

```bash
./otomatis.sh
```

---

## ⚙️ CARA 2: Manual Lewat Terminal EC2

### 1. Buka Terminal EC2

* Klik instance
* Pilih **Connect**
* Klik tombol **Connect** di bawahnya

### 2. Buat Script Manual

```bash
nano otomatis.sh
```

Lalu tempel script yang sama seperti Cara 1. Simpan dan jalankan:

```bash
chmod +x otomatis.sh
./otomatis.sh
```

---

## ✅ UJI COBA AKSES

Buka browser dan akses menggunakan Public IP EC2 instance kamu. Jika berhasil menampilkan aplikasi, maka deploy otomatis berhasil!

### Default Login (Jika Aplikasi Ada Login):

* **Username:** admin
* **Password:** 123

---

## 📚 Penjelasan Perintah Shell Script

| Perintah                         | Fungsi                               |
| -------------------------------- | ------------------------------------ |
| `sudo apt update -y`             | Memperbarui daftar paket             |
| `sudo apt install -y ...`        | Menginstal Apache, PHP, MySQL client |
| `sudo rm -rf /var/www/html/{,.}` | Menghapus semua file di folder web   |
| `sudo git clone ...`             | Menyalin kode aplikasi dari GitHub   |
| `sudo chmod -R 777 ...`          | Mengatur izin akses penuh            |
| `echo DB_... > .env`             | Membuat file konfigurasi environment |
| `sudo apt install -y openssl`    | Instalasi OpenSSL                    |
| `sudo a2enmod ssl`               | Mengaktifkan SSL                     |
| `sudo a2ensite default-ssl.conf` | Mengaktifkan konfigurasi HTTPS       |
| `sudo systemctl reload apache2`  | Memuat ulang Apache                  |
| `chmod +x otomatis.sh`           | Memberikan izin eksekusi ke script   |

---

## 🎉 PENUTUP

Selamat! Kamu telah berhasil melakukan deploy aplikasi secara otomatis menggunakan file `.env` dan shell script di AWS.
Jangan lupa untuk selalu **menjaga kerahasiaan data** dan **memastikan keamanan server**.

✨ Terimakasih dan Selamat Mencoba! 💻🔥
