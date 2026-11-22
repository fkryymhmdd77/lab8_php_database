# lab8_php_database
# Lab8Web — Praktikum 8: PHP dan Database MySQL

**Mata kuliah:** Pemrograman Web OBE
**Nama:** _(Muhamad Fikry Sarbhakty)_
**NIM:** _(312510340)_
**Kelas:** _(I253C)_

Aplikasi CRUD (Create, Read, Update, Delete) sederhana untuk mengelola **Data Barang** menggunakan PHP dan HeidiSQL di Laragon.

## Struktur Folder

```
lab8_php_database/
├── database.sql   # skrip pembuatan database, tabel, dan data awal
├── koneksi.php    # koneksi ke database
├── index.php      # Read   : menampilkan data
├── tambah.php     # Create : menambah data
├── ubah.php       # Update : mengubah data
├── hapus.php      # Delete : menghapus data
├── style.css      # tampilan
└── gambar/        # folder penyimpanan gambar upload
```

## 1. Persiapan: Menjalankan MySQL Server

Buka **Laragon**, lalu klik **Start** pada modul **Apache** dan **MySQL** sampai statusnya berwarna hijau.



## 2. Mengakses phpMyAdmin

Buka `http://localhost/phpmyadmin/` di browser. phpMyAdmin dipakai untuk membuat dan mengelola database lewat antarmuka web.

## 3. Membuat Database dan Tabel

Pada tab **SQL** di phpMyAdmin, jalankan isi file `database.sql`:

- `CREATE DATABASE latihan1;` membuat database.
- `CREATE TABLE data_barang (...)` membuat tabel dengan kolom `id_barang` (primary key, auto increment), `kategori`, `nama`, `gambar`, `harga_beli`, `harga_jual`, dan `stok`.



## 4. Menambahkan Data Awal

Perintah `INSERT INTO data_barang ...` memasukkan tiga data barang (HP Samsung, Xiaomi, dan OPPO). Hasilnya dicek dengan `SELECT * FROM data_barang;`.



## 5. Membuat Folder Proyek

Buat folder `lab8_php_database` di `www` (contoh: `D:\xampp\www\lab8_php_database`), lalu akses lewat `http://localhost/lab8_php_database/`.



## 6. File Koneksi Database (`koneksi.php`)

File ini memanggil `mysqli_connect()` dengan host `localhost`, user `root`, password kosong, dan database `latihan1`. Jika koneksi gagal, program menampilkan pesan dan berhenti (`die()`). Untuk menguji, hilangkan tanda `#` pada `#else echo "Koneksi berhasil";`.


## 7. Menampilkan Data — Read (`index.php`)

- `include("koneksi.php")` memuat koneksi.
- `SELECT * FROM data_barang` mengambil seluruh data.
- `while (mysqli_fetch_array($result))` mengulang baris data menjadi baris tabel HTML.
- Tersedia tombol **Tambah Barang** serta tautan **Ubah** dan **Hapus** di tiap baris.


## 8. Menambah Data — Create (`tambah.php`)

Form dikirim dengan `method="post"` dan `enctype="multipart/form-data"` supaya bisa mengunggah gambar. Setelah tombol **Simpan** ditekan, PHP membaca `$_POST` dan `$_FILES`, memindahkan gambar ke folder `gambar/` dengan `move_uploaded_file()`, lalu menyimpan data dengan `INSERT`, dan kembali ke `index.php`.

## 9. Mengubah Data — Update (`ubah.php`)

Halaman dibuka dengan `ubah.php?id=<id_barang>`. Data lama diambil dengan `SELECT ... WHERE id_barang`, lalu ditampilkan di form. Saat disimpan, data diperbarui dengan `UPDATE ... WHERE id_barang`. Gambar lama dipertahankan jika tidak ada gambar baru yang diunggah.

## 10. Menghapus Data — Delete (`hapus.php`)

Tautan **Hapus** membuka `hapus.php?id=<id_barang>` (dengan konfirmasi JavaScript), lalu menjalankan `DELETE FROM data_barang WHERE id_barang`, dan kembali ke `index.php`.

## Perbaikan terhadap Kode di Modul

Beberapa bagian kode pada modul saya perbaiki agar aplikasi berjalan benar:

| Masalah di modul | Perbaikan |
|---|---|
| `index.php` tidak punya tombol Tambah dan tautan Ubah/Hapus, padahal tampil di gambar | Ditambahkan |
| Urutan header Harga Jual/Beli tidak sama dengan isi kolom, dan kolom Aksi menampilkan id | Disamakan; kolom Aksi berisi Ubah/Hapus |
| Path gambar jadi `gambar/gambar/...` | Path disimpan sekali sebagai `gambar/nama_file` |
| `enctype="multipart/formdata"` (typo) di `ubah.php` | Menjadi `multipart/form-data` |
| `is_select('Komputer', ...)` dipakai untuk semua opsi kategori | Tiap opsi memakai nilainya sendiri |
| Query disusun dari input langsung (rawan SQL Injection) | Memakai prepared statement |
| Output tidak di-escape | Memakai `htmlspecialchars()` |
| Panjang `nama` di tabel 30 vs 50 | Disamakan menjadi 50 |

## Cara Menjalankan

1. Salin folder ini ke `www` dengan nama `lab8_php_database`.
2. Start Apache dan MySQL di Laragon.
3. Jalankan `database.sql` di HeidiSQL.
4. Buka `http://localhost/lab8_php_database/`.
