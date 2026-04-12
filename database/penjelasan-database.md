# 1. `users`

## Fungsi

Ini adalah **tabel akun login utama**.
Semua orang yang bisa masuk ke sistem harus punya data di sini, baik:

- admin
- anggota

Jadi tabel ini tidak menyimpan data detail identitas secara penuh, melainkan lebih ke **data autentikasi dan status akun**.

## Relasi

- `users` terhubung ke `admin_profiles` untuk akun admin
- `users` terhubung ke `member_profiles` untuk akun anggota
- `users` terhubung ke `role_user` untuk menentukan level admin
- `users` juga dipakai sebagai referensi siapa admin yang menyetujui pendaftaran atau membuat transaksi

## Kolom

- `id` → identitas unik user
- `name` → nama akun, bisa dipakai sebagai nama tampilan singkat
- `email` → email login, harus unik
- `password` → password yang sudah di-hash
- `account_type` → penanda apakah akun ini `admin` atau `member`
- `email_verified_at` → kapan email diverifikasi
- `is_active` → status akun aktif atau tidak
- `remember_token` → token untuk fitur “remember me”
- `last_login_at` → kapan terakhir login
- `created_at`, `updated_at` → waktu dibuat dan diperbarui
- `deleted_at` → penanda soft delete

## Catatan

Soft delete di tabel ini penting supaya akun tidak benar-benar hilang dari sistem.
Misalnya ada anggota yang sudah tidak aktif, datanya masih bisa tetap ditelusuri tanpa merusak histori transaksi.

---

# 2. `roles`

## Fungsi

Tabel ini menyimpan **level admin**.
Karena Anda sudah menetapkan admin ada 2 tingkat, maka isi tabel ini minimal:

- `superadmin`
- `admin`

Tabel ini **tidak dipakai untuk anggota**.

## Relasi

- satu `role` bisa dipakai oleh banyak user admin melalui `role_user`

## Kolom

- `id` → identitas unik role
- `name` → nama role yang tampil, misalnya “Super Admin”
- `code` → kode sistem, misalnya `superadmin` atau `admin`
- `description` → penjelasan role
- `created_at`, `updated_at`
- `deleted_at` → soft delete

## Catatan

Soft delete berguna jika suatu saat ada perubahan struktur role.
Misalnya nanti role tertentu tidak dipakai lagi, bisa dinonaktifkan secara logis tanpa menghilangkan histori.

---

# 3. `role_user`

## Fungsi

Ini adalah tabel penghubung antara `users` dan `roles`.
Fungsinya untuk menjawab pertanyaan:

> “Akun admin ini levelnya apa?”

Karena semua akun login ada di `users`, maka level admin tidak langsung ditaruh di `users`, tetapi dipisah lewat tabel ini.

## Relasi

- `user_id` mengarah ke `users.id`
- `role_id` mengarah ke `roles.id`

## Kolom

- `id` → identitas unik relasi
- `user_id` → akun admin yang diberi role
- `role_id` → role yang dimiliki user tersebut
- `created_at`, `updated_at`

## Catatan

Tabel ini **tidak memakai soft delete**.
Alasannya karena ini tabel pivot/relasi, dan biasanya lebih sederhana jika relasi role cukup ditambah, diubah, atau dihapus langsung saja.

Kalau model bisnis Anda adalah satu admin hanya boleh punya satu level, maka `user_id` di sini bisa dibuat unik.

---

# 4. `admin_profiles`

## Fungsi

Tabel ini menyimpan **identitas detail admin/petugas perpustakaan**.

Kalau `users` itu data login, maka `admin_profiles` adalah data personal/domain admin.

## Relasi

- satu `admin_profiles` dimiliki oleh satu `users`
- akun ini nanti tetap bisa dipakai di transaksi sebagai petugas yang membuat atau menyetujui proses tertentu

## Kolom

- `id` → identitas profil admin
- `user_id` → relasi ke akun login admin
- `full_name` → nama lengkap admin
- `phone_number` → nomor HP
- `photo` → foto admin
- `position` → jabatan/posisi kerja
- `created_at`, `updated_at`
- `deleted_at` → soft delete

## Catatan

Soft delete penting agar admin yang sudah tidak bertugas tetap punya jejak di histori sistem.
Misalnya dulu dia pernah menyetujui pendaftaran atau peminjaman.

---

# 5. `member_profiles`

## Fungsi

Tabel ini menyimpan **identitas lengkap anggota perpustakaan**.

Ini adalah tabel utama untuk data anggota, terpisah dari `users` karena:

- `users` fokus login
- `member_profiles` fokus identitas keanggotaan

## Relasi

- satu `member_profiles` dimiliki oleh satu `users`
- satu anggota bisa punya banyak:
    - `loans`
    - `reservations`
    - `fines`

## Kolom

- `id` → identitas profil anggota
- `user_id` → relasi ke akun login anggota
- `member_code` → kode anggota perpustakaan, harus unik
- `identity_number` → nomor identitas, misalnya KTP/NIS/NIM
- `full_name` → nama lengkap anggota
- `gender` → jenis kelamin
- `birth_place` → tempat lahir
- `birth_date` → tanggal lahir
- `phone_number` → nomor HP
- `address` → alamat lengkap
- `photo` → foto anggota
- `registration_date` → tanggal resmi terdaftar sebagai anggota
- `membership_status` → status keanggotaan, misalnya:
    - `pending`
    - `active`
    - `suspended`
    - `inactive`

- `valid_until` → masa berlaku keanggotaan jika dibatasi waktu
- `created_at`, `updated_at`
- `deleted_at` → soft delete

## Catatan

Soft delete di sini sangat penting, karena anggota yang nonaktif atau keluar tetap mungkin punya histori pinjam, reservasi, atau denda.

---

# 6. `registrations`

## Fungsi

Tabel ini untuk **pendaftaran mandiri calon anggota**.

Jadi alurnya begini:

1. calon anggota isi form daftar
2. data masuk ke `registrations`
3. admin meninjau
4. kalau disetujui, dibuat akun di `users` dan profil di `member_profiles`

Admin **tidak masuk** lewat tabel ini.

## Relasi

- `approved_by` mengarah ke `users.id`, yaitu admin yang memutuskan
- `user_id` bisa diisi setelah pendaftaran disetujui dan akun benar-benar dibuat

## Kolom

- `id` → identitas pendaftaran
- `user_id` → akun anggota yang terbentuk setelah approved
- `full_name` → nama calon anggota
- `email` → email pendaftaran
- `password` → password awal yang akan dipakai saat akun dibuat
- `phone_number` → nomor HP
- `address` → alamat
- `identity_number` → nomor identitas
- `status` → status pendaftaran:
    - `pending`
    - `approved`
    - `rejected`

- `rejection_reason` → alasan jika ditolak
- `approved_by` → admin yang menyetujui/menolak
- `approved_at` → waktu keputusan
- `created_at`, `updated_at`
- `deleted_at` → soft delete

## Catatan

Soft delete berguna jika suatu pendaftaran ingin “disembunyikan” dari tampilan utama tetapi datanya tetap aman tersimpan.

---

# 7. `authors`

## Fungsi

Tabel master untuk **penulis buku**.

Satu penulis bisa menulis banyak buku, dan satu buku juga bisa punya lebih dari satu penulis.

## Relasi

- many-to-many dengan `books` melalui `book_authors`

## Kolom

- `id`
- `name` → nama penulis
- `slug` → versi URL-friendly dari nama
- `photo` → foto penulis
- `about` → biografi singkat
- `nationality` → kebangsaan
- `email` → email penulis jika ada
- `status` → aktif/nonaktif
- `created_at`, `updated_at`
- `deleted_at` → soft delete

## Catatan

Soft delete berguna kalau penulis tidak ingin ditampilkan lagi, tetapi relasi historis dengan buku tetap dipertahankan.

---

# 8. `publishers`

## Fungsi

Tabel master untuk **penerbit**.

## Relasi

- satu penerbit bisa menerbitkan banyak buku

## Kolom

- `id`
- `name` → nama penerbit
- `slug`
- `address` → alamat penerbit
- `phone` → nomor telepon
- `email`
- `website`
- `status`
- `created_at`, `updated_at`
- `deleted_at`

## Catatan

Soft delete membantu menyembunyikan penerbit yang sudah tidak dipakai tanpa menghapus histori buku yang pernah diterbitkan olehnya.

---

# 9. `categories`

## Fungsi

Tabel master untuk **kategori buku**.

Misalnya:

- novel
- agama
- pendidikan
- komik

## Relasi

- satu kategori bisa dimiliki banyak buku

## Kolom

- `id`
- `name` → nama kategori
- `slug`
- `description`
- `status`
- `created_at`, `updated_at`
- `deleted_at`

## Catatan

Soft delete memudahkan jika kategori lama tidak lagi digunakan, tetapi buku lama masih tetap terhubung.

---

# 10. `shelves`

## Fungsi

Tabel master untuk **rak atau lokasi penyimpanan buku**.

Ini dipakai agar admin tahu sebuah buku diletakkan di mana secara fisik.

## Relasi

- satu rak bisa menampung banyak buku

## Kolom

- `id`
- `code` → kode rak, misalnya `A-01`
- `name` → nama rak
- `location_description` → keterangan lokasi rak
- `status`
- `created_at`, `updated_at`
- `deleted_at`

## Catatan

Soft delete dipakai supaya data rak lama tidak hilang total saat tata ruang perpustakaan berubah.

---

# 11. `books`

## Fungsi

Ini adalah **master data buku**, artinya level judul buku, bukan fisik bukunya.

Contoh:

- “Laskar Pelangi” adalah satu data di `books`
- tetapi bisa punya 5 eksemplar fisik di `book_copies`

## Relasi

- `category_id` ke `categories`
- `publisher_id` ke `publishers`
- `shelf_id` ke `shelves`
- many-to-many ke `authors` lewat `book_authors`
- one-to-many ke `book_copies`

## Kolom

- `id`
- `category_id` → kategori buku
- `publisher_id` → penerbit
- `shelf_id` → rak utama/lokasi
- `title` → judul buku
- `slug` → URL-friendly judul
- `isbn` → nomor ISBN
- `year` → tahun terbit
- `edition` → edisi buku
- `language` → bahasa buku
- `page_count` → jumlah halaman
- `description` → deskripsi singkat
- `synopsis` → sinopsis
- `cover_image` → gambar cover
- `stock` → total jumlah copy
- `available_stock` → jumlah copy yang sedang tersedia
- `status` → aktif/nonaktif
- `created_at`, `updated_at`
- `deleted_at`

## Catatan

Soft delete sangat penting di sini.
Kalau sebuah buku tidak lagi ditampilkan di katalog, data buku tetap aman dan histori pinjam tidak rusak.

---

# 12. `book_authors`

## Fungsi

Tabel ini adalah penghubung antara `books` dan `authors`.

Karena relasinya many-to-many:

- satu buku bisa ditulis banyak penulis
- satu penulis bisa punya banyak buku

## Relasi

- `book_id` ke `books`
- `author_id` ke `authors`

## Kolom

- `id`
- `book_id`
- `author_id`
- `created_at`
- `updated_at`

## Catatan

Biasanya tabel ini tidak memakai soft delete karena ini murni tabel relasi.

---

# 13. `book_copies`

## Fungsi

Kalau `books` adalah judul, maka `book_copies` adalah **eksemplar fisik buku** yang benar-benar ada di rak dan bisa dipinjam.

Contoh:

- Buku “Laskar Pelangi” ada 3 copy
- berarti di `book_copies` ada 3 baris data

## Relasi

- setiap copy pasti milik satu `books`
- satu copy bisa muncul di banyak histori `loan_details` sepanjang masa pemakaiannya

## Kolom

- `id`
- `book_id` → relasi ke judul buku
- `barcode` → barcode unik per copy
- `inventory_code` → kode inventaris unik
- `acquisition_date` → tanggal buku diperoleh
- `source` → sumber buku:
    - `purchase`
    - `donation`
    - `other`

- `price` → harga beli jika ada
- `condition` → kondisi fisik umum:
    - `new`
    - `good`
    - `fair`
    - `damaged`
    - `lost`

- `copy_status` → status operasional:
    - `available`
    - `borrowed`
    - `reserved`
    - `maintenance`
    - `lost`

- `notes` → catatan tambahan
- `created_at`, `updated_at`
- `deleted_at`

## Catatan

Soft delete dipakai kalau sebuah copy sudah dikeluarkan dari koleksi aktif, misalnya rusak berat atau sudah tidak dipakai lagi.

---

# 14. `loans`

## Fungsi

Ini adalah **header transaksi peminjaman**.

Satu transaksi peminjaman bisa berisi satu atau beberapa buku, jadi tabel ini menjadi “kepala transaksi”.

## Relasi

- `member_id` ke `member_profiles`
- `created_by` ke `users` admin yang membuat transaksi
- `approved_by` ke `users` admin yang menyetujui jika prosesnya perlu persetujuan
- satu `loans` punya banyak `loan_details`

## Kolom

- `id`
- `loan_code` → kode transaksi unik
- `member_id` → anggota yang meminjam
- `loan_date` → tanggal pinjam
- `due_date` → batas tanggal kembali
- `return_date` → tanggal pengembalian total transaksi
- `status` → status transaksi:
    - `borrowed`
    - `returned`
    - `partially_returned`
    - `overdue`
    - `lost`

- `total_fine` → total denda dari transaksi
- `notes` → catatan transaksi
- `created_by` → admin yang memproses
- `approved_by` → admin yang menyetujui
- `created_at`, `updated_at`

## Catatan

Tabel ini **tidak memakai soft delete**.
Alasannya karena transaksi peminjaman adalah histori penting. Kalau dihapus logis pun, laporan bisa rancu. Untuk kasus pembatalan lebih aman pakai `status`.

---

# 15. `loan_details`

## Fungsi

Ini adalah **detail isi transaksi peminjaman**.

Kalau `loans` adalah kepala transaksi, maka `loan_details` menjelaskan buku copy mana saja yang dipinjam.

## Relasi

- `loan_id` ke `loans`
- `book_copy_id` ke `book_copies`

## Kolom

- `id`
- `loan_id` → transaksi induk
- `book_copy_id` → copy buku yang dipinjam
- `due_date` → batas kembali per item
- `returned_at` → kapan item ini benar-benar dikembalikan
- `fine_amount` → denda untuk item ini
- `condition_on_loan` → kondisi buku saat dipinjam
- `condition_on_return` → kondisi buku saat dikembalikan
- `status` → status item:
    - `borrowed`
    - `returned`
    - `overdue`
    - `lost`

- `notes`
- `created_at`, `updated_at`

## Catatan

Tabel ini juga **tidak memakai soft delete** karena menjadi detail audit transaksi.

---

# 16. `reservations`

## Fungsi

Tabel ini menyimpan **pemesanan atau booking buku** oleh anggota.

Misalnya buku sedang dipinjam orang lain, maka anggota bisa antre dengan reservasi.

## Relasi

- `member_id` ke `member_profiles`
- `book_id` ke `books`

## Kolom

- `id`
- `member_id` → anggota yang reservasi
- `book_id` → judul buku yang dipesan
- `reservation_date` → kapan reservasi dibuat
- `expire_at` → batas waktu pengambilan/pemakaian reservasi
- `status` → status reservasi:
    - `waiting`
    - `ready`
    - `cancelled`
    - `fulfilled`
    - `expired`

- `notes`
- `created_at`, `updated_at`
- `deleted_at`

## Catatan

Soft delete berguna agar reservasi lama bisa disembunyikan dari tampilan aktif tanpa menghilangkan jejak datanya.

---

# 17. `fines`

## Fungsi

Tabel ini menyimpan **data denda**.

Denda bisa muncul karena:

- telat mengembalikan
- buku rusak
- buku hilang
- alasan lain

## Relasi

- `loan_detail_id` ke `loan_details`
- `member_id` ke `member_profiles`

## Kolom

- `id`
- `loan_detail_id` → item pinjaman yang menimbulkan denda
- `member_id` → anggota yang dikenai denda
- `amount` → nominal denda
- `reason` → alasan denda:
    - `late_return`
    - `lost_book`
    - `damaged_book`
    - `other`

- `status` → status pembayaran:
    - `unpaid`
    - `paid`
    - `waived`

- `paid_at` → kapan dibayar
- `notes`
- `created_at`, `updated_at`

## Catatan

Tabel ini juga **tidak memakai soft delete** karena bagian dari histori keuangan dan audit transaksi.

---

# Gambaran relasi besar

## A. Jalur akun admin

`users` → `admin_profiles`
`users` → `role_user` → `roles`

Artinya:

- login admin ada di `users`
- identitas admin ada di `admin_profiles`
- level admin ditentukan oleh `roles`

---

## B. Jalur akun anggota

`registrations` → `users` → `member_profiles`

Artinya:

- calon anggota daftar dulu
- kalau disetujui, dibuat akun login
- lalu dibentuk profil anggota resmi

---

## C. Jalur data buku

`categories` → `books`
`publishers` → `books`
`shelves` → `books`
`authors` ↔ `book_authors` ↔ `books`
`books` → `book_copies`

Artinya:

- buku punya kategori, penerbit, dan lokasi rak
- buku bisa punya banyak penulis
- satu judul buku bisa punya banyak copy fisik

---

## D. Jalur transaksi

`member_profiles` → `loans` → `loan_details` → `book_copies` → `books`

Artinya:

- anggota melakukan peminjaman
- satu transaksi punya beberapa detail item
- item itu menunjuk ke copy buku fisik tertentu

---

## E. Jalur layanan tambahan

`member_profiles` → `reservations` → `books`
`member_profiles` → `fines` → `loan_details`

Artinya:

- anggota bisa reservasi judul buku
- anggota juga bisa punya denda dari item pinjaman tertentu

---

# Ringkasan soft delete

## Tabel yang memakai `deleted_at`

Karena datanya bisa “disembunyikan” tanpa benar-benar dihapus:

- `users`
- `roles`
- `admin_profiles`
- `member_profiles`
- `registrations`
- `authors`
- `publishers`
- `categories`
- `shelves`
- `books`
- `book_copies`
- `reservations`

## Tabel yang tidak memakai `deleted_at`

Karena sifatnya histori transaksi atau tabel relasi:

- `role_user`
- `book_authors`
- `loans`
- `loan_details`
- `fines`
