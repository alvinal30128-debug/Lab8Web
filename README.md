# Lab8Web - Praktikum 8: PHP dan Database MySQL

Mata Kuliah: Pemrograman Web

| | |
|---|---|
| Nama | Alvin |
| NIM | 312510044 |
| Kelas | I252A |

## Tujuan

1. Memahami konsep dasar database.
2. Memahami konsep dasar CRUD (Create, Read, Update, Delete) menggunakan PHP.
3. Membuat program CRUD sederhana menggunakan PHP dan MySQL.

## Struktur Proyek

```
Lab8Web/
├── database.sql      # SQL: database, tabel, dan data awal
├── koneksi.php       # koneksi ke database
├── index.php         # Read   : menampilkan data
├── tambah.php        # Create : menambah data
├── ubah.php          # Update : mengubah data
├── hapus.php         # Delete : menghapus data
├── style.css         # tampilan
├── gambar/           # folder gambar barang (juga tempat upload)
└── screenshots/      # gambar hasil praktikum
```

## Cara Menjalankan

1. Jalankan **Apache** dan **MySQL** pada XAMPP Control Panel.
2. Salin folder proyek ke `htdocs` (mis. `C:\xampp\htdocs\lab8_php_database`).
3. Buka `http://localhost/phpmyadmin/`, lalu import file `database.sql` (atau jalankan isinya pada tab SQL).
4. Buka `http://localhost/lab8_php_database/` di browser.

## 1. Persiapan: XAMPP dan phpMyAdmin

Aplikasi CRUD membutuhkan web server (Apache), PHP, dan database server (MySQL/MariaDB). Ketiganya disediakan oleh XAMPP. Pada XAMPP Control Panel, modul Apache dan MySQL dijalankan dengan menekan tombol Start sampai statusnya berwarna hijau. Setelah itu phpMyAdmin dapat dibuka melalui browser di alamat http://localhost/phpmyadmin/ untuk mengelola database.

Folder kerja bernama lab8_php_database dibuat di dalam htdocs (docroot web server), sehingga aplikasi dapat diakses lewat http://localhost/lab8_php_database/.

> Tempel screenshot XAMPP Control Panel (Apache dan MySQL berstatus Running) dan halaman phpMyAdmin Anda di sini.

## 2. Membuat Database dan Tabel

Studi kasus yang dipakai adalah Data Barang. Database bernama latihan1 dibuat terlebih dahulu, kemudian tabel data_barang dengan field id_barang (primary key, auto increment), kategori, nama, gambar, harga_beli, harga_jual, dan stok. Perintah SQL dijalankan pada tab SQL di phpMyAdmin atau melalui MySQL client.

File `database.sql`:

```sql
-- Membuat database
CREATE DATABASE IF NOT EXISTS latihan1;
USE latihan1;

-- Membuat tabel
CREATE TABLE IF NOT EXISTS data_barang (
  id_barang  int(10) auto_increment PRIMARY KEY,
  kategori   varchar(30),
  nama       varchar(50),
  gambar     varchar(100),
  harga_beli decimal(10,0),
  harga_jual decimal(10,0),
  stok       int(4)
);
```

![Gambar 1. Database latihan1 dan struktur tabel data_barang](screenshots/01_database_tabel.png)

*Gambar 1. Database latihan1 dan struktur tabel data_barang*

## 3. Menambahkan Data Awal

Tiga data barang (HP Samsung, HP Xiaomi, dan HP OPPO) dimasukkan ke tabel menggunakan perintah INSERT INTO. Perintah SELECT * digunakan untuk memastikan data sudah tersimpan.

![Gambar 2. Isi tabel data_barang setelah INSERT](screenshots/02_data_barang.png)

*Gambar 2. Isi tabel data_barang setelah INSERT*

## 4. Membuat File Koneksi Database (koneksi.php)

File koneksi.php berisi pengaturan host, user, password, dan nama database, lalu membuat koneksi dengan mysqli_connect(). Jika koneksi gagal, program menampilkan pesan kesalahan dan berhenti. File ini di-include oleh semua file CRUD lainnya, sehingga pengaturan koneksi cukup ditulis satu kali. Untuk pengujian, baris echo "Koneksi berhasil" diaktifkan sementara.

File `koneksi.php`:

```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db   = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} #else echo "Koneksi berhasil";
?>
```

![Gambar 3. Tes koneksi database berhasil](screenshots/03_tes_koneksi.png)

*Gambar 3. Tes koneksi database berhasil*

## 5. Menampilkan Data (Read) - index.php

File index.php menjalankan query SELECT * FROM data_barang, lalu menampilkan hasilnya dalam tabel HTML menggunakan perulangan while dan mysqli_fetch_array(). Setiap baris memiliki tautan Ubah dan Hapus, serta terdapat tautan Tambah Barang di atas tabel. Jika tabel kosong, ditampilkan tulisan "Belum ada data".

![Gambar 4. Halaman daftar data barang (index.php)](screenshots/04_index.png)

*Gambar 4. Halaman daftar data barang (index.php)*

## 6. Menambah Data (Create) - tambah.php

File tambah.php menampilkan form input (nama, kategori, harga jual, harga beli, stok, dan file gambar). Form memakai method POST dan enctype multipart/form-data agar dapat mengunggah file. Saat tombol Simpan ditekan, file gambar dipindahkan ke folder gambar/ dengan move_uploaded_file(), data disimpan ke tabel dengan perintah INSERT, lalu pengguna diarahkan kembali ke index.php.

Sebagai contoh, ditambahkan barang "TV Samsung" dengan stok 20.

![Gambar 5. Form Tambah Barang](screenshots/05_form_tambah.png)

*Gambar 5. Form Tambah Barang*

![Gambar 6. Data TV Samsung berhasil ditambahkan](screenshots/06_index_setelah_tambah.png)

*Gambar 6. Data TV Samsung berhasil ditambahkan*

## 7. Mengubah Data (Update) - ubah.php

File ubah.php menerima parameter id melalui URL (ubah.php?id=...), mengambil data barang tersebut, dan menampilkannya pada form yang sudah terisi. Setelah disimpan, perintah UPDATE dijalankan. Gambar hanya diperbarui jika pengguna memilih file baru; jika tidak, gambar lama tetap dipakai.

Pada contoh, kategori TV Samsung diubah menjadi Komputer dan harga jualnya menjadi 4000000.

![Gambar 7. Form Ubah Barang berisi data lama](screenshots/07_form_ubah.png)

*Gambar 7. Form Ubah Barang berisi data lama*

![Gambar 8. Data berhasil diubah](screenshots/08_index_setelah_ubah.png)

*Gambar 8. Data berhasil diubah*

## 8. Menghapus Data (Delete) - hapus.php

File hapus.php menerima id barang melalui URL, menjalankan perintah DELETE untuk baris tersebut, lalu kembali ke index.php. Pada contoh, data TV Samsung dihapus sehingga tabel kembali berisi tiga data awal.

File `hapus.php`:

```php
<?php
include_once 'koneksi.php';

$id = isset($_GET['id']) ? (int) $_GET['id'] : 0;
$sql = "DELETE FROM data_barang WHERE id_barang = {$id}";
$result = mysqli_query($conn, $sql);

header('location: index.php');
exit;
?>
```

![Gambar 9. Data TV Samsung berhasil dihapus](screenshots/09_index_setelah_hapus.png)

*Gambar 9. Data TV Samsung berhasil dihapus*

## 9. Kesimpulan

Dengan PHP dan MySQL, empat operasi dasar pengelolaan data (Create, Read, Update, Delete) dapat dibuat dengan file yang sederhana: koneksi.php untuk menghubungkan ke database, index.php untuk membaca data, tambah.php untuk menambah data, ubah.php untuk mengubah data, dan hapus.php untuk menghapus data. Praktikum ini menjadi dasar untuk membuat aplikasi web berbasis database yang lebih lengkap.

