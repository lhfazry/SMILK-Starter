# 🖥️ SMILK: Sistem Manajemen Inventaris Lab Komputer

Sistem ini dirancang khusus untuk membantu pengelolaan barang di Laboratorium Komputer dengan cara yang visual dan efisien. Menggunakan **Laravel** dan **Filament**, sistem ini memungkinkan pengelolaan kategori barang, detail barang (termasuk upload gambar), serta log perawatan barang.

---

## 🛠️ Persiapan Awal

Pastikan Anda sudah berada di dalam direktori proyek ini dan sudah menjalankan:

```bash
composer install
cp .env.example .env
php artisan key:generate
touch database/database.sqlite
php artisan migrate
```

---

## 🚀 Langkah-Langkah Pengerjaan

### 1. Membuat Struktur Database & Model (15 Menit)

Tahap awal adalah menyiapkan fondasi data menggunakan Migration dan Model.

- **Jalankan Perintah:**
    ```bash
    php artisan make:model Kategori -m
    php artisan make:model Barang -m
    php artisan make:model LogPerawatan -m
    ```
- **Konfigurasi Migrasi:**
    - **Kategoris:** Tambahkan `$table->string('nama_kategori');`
    - **Barangs:** Tambahkan relasi ke kategori, kode barang, nama, kondisi (enum), dan gambar.
    - **LogPerawatans:** Tambahkan relasi ke barang, tanggal, dan catatan.
- **Set Relasi di Model:**
    - Gunakan `protected $guarded = [];` untuk mempermudah _mass assignment_.
    - Hubungkan `Barang` ke `Kategori` (_BelongsTo_) dan `Kategori` ke `Barang` (_HasMany_).

### 2. "The Filament Magic" - Generate Resources (10 Menit)

Membuat antarmuka admin secara instan tanpa perlu membuat Controller atau View manual.

- **Jalankan Perintah:**
    ```bash
    php artisan make:filament-resource Kategori
    php artisan make:filament-resource Barang
    php artisan make:filament-resource LogPerawatan
    ```
- **Cek Hasilnya:** Buka browser dan refresh halaman admin Anda (biasanya di `/admin`). Tiga menu baru akan muncul di sidebar!

### 3. Konfigurasi `KategoriResource` (10 Menit)

Konfigurasi dasar untuk memahami bagaimana `Form` dan `Table` bekerja di Filament.

- **Form:** Tambahkan `TextInput` untuk `nama_kategori` dengan validasi `required()`.
- **Table:** Tambahkan `TextColumn` untuk menampilkan `nama_kategori`.

### 4. Konfigurasi `BarangResource` - Fitur Utama (25 Menit)

Memanfaatkan fitur canggih Filament seperti upload gambar dan relasi dropdown.

- **Form:**
    - `TextInput` untuk kode dan nama barang.
    - `Select` untuk relasi kategori (menggunakan `relationship`).
    - `Select` untuk kondisi barang (Baik/Rusak).
    - `FileUpload` untuk mengunggah gambar barang.
- **Table:**
    - Tampilkan semua kolom.
    - Gunakan `ImageColumn` agar foto barang langsung terlihat.
    - Tambahkan **Badge** warna pada kolom kondisi (Hijau untuk Baik, Merah untuk Rusak).
    - **Filter:** Tambahkan filter untuk mempermudah pencarian barang yang rusak saja.

### 5. Konfigurasi `LogPerawatanResource` (15 Menit)

Mengelola riwayat perawatan barang dengan input yang lebih variatif.

- **Form:**
    - `Select` untuk memilih barang yang dirawat.
    - `DatePicker` untuk mencatat tanggal perawatan (default: hari ini).
    - `RichEditor` untuk menulis catatan perawatan yang detail (format tebal, list, dll).

### 6. Dashboard Widget - Penutup (15 Menit)

Membuat dashboard admin terlihat lebih profesional dengan ringkasan data.

- **Jalankan Perintah:**
    ```bash
    php artisan make:filament-widget StatsOverview --type=stats-overview
    ```
- **Isi Widget:** Tampilkan statistik total Kategori, total Barang, dan jumlah Barang yang sedang Rusak.

---

## 📝 Catatan Penting

- Gunakan **SQLite** untuk kemudahan setup database.
- Jangan lupa menjalankan `php artisan storage:link` jika fitur upload gambar tidak muncul.

Selamat berkarya! 🚀
