# 🎓 Tutorial Lengkap: Membuat Sistem Perpustakaan dari Awal

Tutorial ini akan memandu Anda membuat aplikasi Sistem Perpustakaan dari NOL menggunakan Laravel 11 dan Filament 3.

## 📋 Daftar Isi

1. [Persiapan](#1-persiapan)
2. [Membuat Project Laravel](#2-membuat-project-laravel)
3. [Install Filament](#3-install-filament)
4. [Setup Database](#4-setup-database)
5. [Membuat Models & Migrations](#5-membuat-models--migrations)
6. [Membuat Enums](#6-membuat-enums)
7. [Membuat Seeders](#7-membuat-seeders)
8. [Membuat Actions & Services](#8-membuat-actions--services)
9. [Membuat Filament Resources](#9-membuat-filament-resources)
10. [Konfigurasi Filament Panel](#10-konfigurasi-filament-panel)
11. [Jalankan Migration & Seeder](#11-jalankan-migration--seeder)
12. [Setup Storage](#12-setup-storage)
13. [Install & Compile Assets](#13-install--compile-assets)
14. [Testing](#14-testing)
15. [Struktur Folder Akhir](#15-struktur-folder-akhir)
16. [Fitur-Fitur yang Sudah Dibuat](#16-fitur-fitur-yang-sudah-dibuat)

---

## 1. Persiapan

### Pastikan Terinstall:
- PHP >= 8.2
- Composer
- Node.js >= 18
- MySQL >= 8.0
- Git

### Verifikasi:
```bash
php -v
composer -V
node -v
mysql --version
```

---

## 2. Membuat Project Laravel

### Step 1: Create Project

```bash
composer create-project laravel/laravel="12.x" library-app
cd library-app
```

### Step 2: Test Project

```bash
php artisan serve
```

Buka browser: `http://localhost:8000`

---

## 3. Install Filament

### Step 1: Install Filament Package

```bash
composer require filament/filament:"^3.3 -W"
```

### Step 2: Install Filament Panel

```bash
php artisan filament:install --panels
```

Pilih:
- Panel ID: `admin`
- Create user: `No` (kita akan buat manual)

### Step 3: Test Filament

```bash
php artisan serve
```

Buka: `http://localhost:8000/admin`

---

## 4. Setup Database

### Step 1: Buat Database

```sql
CREATE DATABASE perpustakaan_usk CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Step 2: Konfigurasi .env

Edit file `.env`:

```env
APP_NAME="Perpustakaan USK"
DB_DATABASE=perpustakaan_usk
DB_USERNAME=root
DB_PASSWORD=
```

### Step 3: Test Koneksi

```bash
php artisan db:show
```

---

## 5. Membuat Models & Migrations

### 5.1. Role Model

```bash
php artisan make:model Role -m
```

**Edit: `database/migrations/xxxx_create_roles_table.php`**

📄 [Copy dari: database/migrations/2024_01_01_000001_create_roles_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000001_create_roles_table.php)

**Edit: `app/Models/Role.php`**

📄 [Copy dari: app/Models/Role.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Role.php)

### 5.2. Role User Pivot Table

```bash
php artisan make:migration create_role_user_table
```

**Edit: `database/migrations/xxxx_create_role_user_table.php`**

📄 [Copy dari: database/migrations/2024_01_01_000002_create_role_user_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000002_create_role_user_table.php)

### 5.3. Update User Model

**Edit: `app/Models/User.php`**

📄 [Copy dari: app/Models/User.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/User.php)

**Edit: `database/migrations/0001_01_01_000000_create_users_table.php`**

📄 [Copy dari: database/migrations/0001_01_01_000000_create_users_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/0001_01_01_000000_create_users_table.php)

### 5.4. Admin Profile

```bash
php artisan make:model AdminProfile -m
```

**Edit migration & model:**

📄 [Copy migration dari: database/migrations/2024_01_01_000003_create_admin_profiles_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000003_create_admin_profiles_table.php)

📄 [Copy model dari: app/Models/AdminProfile.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/AdminProfile.php)

### 5.5. Member Profile

```bash
php artisan make:model MemberProfile -m
```

**Edit migration & model:**

📄 [Copy migration dari: database/migrations/2024_01_01_000004_create_member_profiles_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000004_create_member_profiles_table.php)

📄 [Copy model dari: app/Models/MemberProfile.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/MemberProfile.php)

### 5.6. Registration

```bash
php artisan make:model Registration -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000005_create_registrations_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000005_create_registrations_table.php)

📄 [Copy model dari: app/Models/Registration.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Registration.php)

### 5.7. Loan Rule

```bash
php artisan make:model LoanRule -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000006_create_loan_rules_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000006_create_loan_rules_table.php)

📄 [Copy model dari: app/Models/LoanRule.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/LoanRule.php)

### 5.8. Genre (Baru!)

```bash
php artisan make:model Genre -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000007_create_genres_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000007_create_genres_table.php)

📄 [Copy model dari: app/Models/Genre.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Genre.php)

### 5.9. Category

```bash
php artisan make:model Category -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000007_create_categories_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000007_create_categories_table.php)

📄 [Copy model dari: app/Models/Category.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Category.php)

### 5.10. Author

```bash
php artisan make:model Author -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000008_create_authors_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000008_create_authors_table.php)

📄 [Copy model dari: app/Models/Author.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Author.php)

### 5.11. Publisher

```bash
php artisan make:model Publisher -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000009_create_publishers_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000009_create_publishers_table.php)

📄 [Copy model dari: app/Models/Publisher.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Publisher.php)

### 5.12. Shelf

```bash
php artisan make:model Shelf -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000010_create_shelves_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000010_create_shelves_table.php)

📄 [Copy model dari: app/Models/Shelf.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Shelf.php)

### 5.13. Book

```bash
php artisan make:model Book -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000011_create_books_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000011_create_books_table.php)

📄 [Copy model dari: app/Models/Book.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Book.php)

### 5.14. Book Authors Pivot

```bash
php artisan make:migration create_book_authors_table
```

📄 [Copy dari: database/migrations/2024_01_01_000012_create_book_authors_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000012_create_book_authors_table.php)

### 5.15. Book Genres Pivot (Baru!)

```bash
php artisan make:migration create_book_genres_table
```

📄 [Copy dari: database/migrations/2024_01_01_000012_create_book_genres_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000012_create_book_genres_table.php)

### 5.16. Loan

```bash
php artisan make:model Loan -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000014_create_loans_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000014_create_loans_table.php)

📄 [Copy model dari: app/Models/Loan.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Loan.php)

### 5.17. Loan Detail

```bash
php artisan make:model LoanDetail -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000015_create_loan_details_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000015_create_loan_details_table.php)

📄 [Copy model dari: app/Models/LoanDetail.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/LoanDetail.php)

### 5.17. Reservation

```bash
php artisan make:model Reservation -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000016_create_reservations_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000016_create_reservations_table.php)

📄 [Copy model dari: app/Models/Reservation.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Reservation.php)

### 5.18. Fine

```bash
php artisan make:model Fine -m
```

📄 [Copy migration dari: database/migrations/2024_01_01_000017_create_fines_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2024_01_01_000017_create_fines_table.php)

📄 [Copy model dari: app/Models/Fine.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/Fine.php)

### 5.19. Site Setting

```bash
php artisan make:model SiteSetting -m
```

📄 [Copy migration dari: database/migrations/2026_04_10_170641_create_site_settings_table.php](https://github.com/rifqi011/usk-perpus/blob/main/database/migrations/2026_04_10_170641_create_site_settings_table.php)

📄 [Copy model dari: app/Models/SiteSetting.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Models/SiteSetting.php)

---

## 6. Membuat Enums

Buat folder `app/Enums` dan buat file-file berikut:

```bash
mkdir app/Enums
```

Copy semua file dari repository folder `app/Enums/`:

📄 [Copy dari: app/Enums/AccountType.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/AccountType.php)

📄 [Copy dari: app/Enums/ActiveStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/ActiveStatus.php)

📄 [Copy dari: app/Enums/BookCondition.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/BookCondition.php)

📄 [Copy dari: app/Enums/CopyStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/CopyStatus.php)

📄 [Copy dari: app/Enums/FineStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/FineStatus.php)

📄 [Copy dari: app/Enums/FineType.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/FineType.php)

📄 [Copy dari: app/Enums/Gender.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/Gender.php)

📄 [Copy dari: app/Enums/LoanStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/LoanStatus.php)

📄 [Copy dari: app/Enums/MembershipStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/MembershipStatus.php)

📄 [Copy dari: app/Enums/RegistrationStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/RegistrationStatus.php)

📄 [Copy dari: app/Enums/ReservationStatus.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Enums/ReservationStatus.php)

---

## 7. Membuat Seeders

### 7.1. Role Seeder

```bash
php artisan make:seeder RoleSeeder
```

**Edit: `database/seeders/RoleSeeder.php`**

📄 [Copy dari: database/seeders/RoleSeeder.php](https://github.com/rifqi011/usk-perpus/blob/main/database/seeders/RoleSeeder.php)

### 7.2. Super Admin Seeder

```bash
php artisan make:seeder SuperAdminSeeder
```

**Edit: `database/seeders/SuperAdminSeeder.php`**

📄 [Copy dari: database/seeders/SuperAdminSeeder.php](https://github.com/rifqi011/usk-perpus/blob/main/database/seeders/SuperAdminSeeder.php)

### 7.3. Loan Rule Seeder

```bash
php artisan make:seeder LoanRuleSeeder
```

**Edit: `database/seeders/LoanRuleSeeder.php`**

📄 [Copy dari: database/seeders/LoanRuleSeeder.php](https://github.com/rifqi011/usk-perpus/blob/main/database/seeders/LoanRuleSeeder.php)

### 7.4. Site Setting Seeder

```bash
php artisan make:seeder SiteSettingSeeder
```

**Edit: `database/seeders/SiteSettingSeeder.php`**

📄 [Copy dari: database/seeders/SiteSettingSeeder.php](https://github.com/rifqi011/usk-perpus/blob/main/database/seeders/SiteSettingSeeder.php)

### 7.5. Update Database Seeder

**Edit: `database/seeders/DatabaseSeeder.php`**

📄 [Copy dari: database/seeders/DatabaseSeeder.php](https://github.com/rifqi011/usk-perpus/blob/main/database/seeders/DatabaseSeeder.php)

---

## 8. Membuat Actions & Services

### 8.1. Buat Folder Actions

```bash
mkdir -p app/Actions/Loan
mkdir -p app/Actions/Member
mkdir -p app/Actions/Reservation
```

### 8.2. Loan Actions

📄 [Copy dari: app/Actions/Loan/CalculateFineAction.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Actions/Loan/CalculateFineAction.php)

📄 [Copy dari: app/Actions/Loan/CreateLoanAction.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Actions/Loan/CreateLoanAction.php)

📄 [Copy dari: app/Actions/Loan/ProcessReturnAction.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Actions/Loan/ProcessReturnAction.php)

### 8.3. Member Actions

📄 [Copy dari: app/Actions/Member/ApproveRegistrationAction.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Actions/Member/ApproveRegistrationAction.php)

📄 [Copy dari: app/Actions/Member/RejectRegistrationAction.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Actions/Member/RejectRegistrationAction.php)

### 8.4. Reservation Actions

📄 [Copy dari: app/Actions/Reservation/CreateReservationAction.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Actions/Reservation/CreateReservationAction.php)

### 8.5. Services

```bash
mkdir app/Services
```

📄 [Copy dari: app/Services/LoanService.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Services/LoanService.php)

📄 [Copy dari: app/Services/FineService.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Services/FineService.php)

📄 [Copy dari: app/Services/ReservationService.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Services/ReservationService.php)

---

## 9. Membuat Filament Resources

### 9.1. Admin Resource

```bash
php artisan make:filament-resource Admin --generate
```

Pilih:
- Model: `User`
- Generate: Yes

**Edit files:**

📄 [Copy dari: app/Filament/Resources/AdminResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AdminResource.php)

📄 [Copy dari: app/Filament/Resources/AdminResource/Pages/CreateAdmin.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AdminResource/Pages/CreateAdmin.php)

📄 [Copy dari: app/Filament/Resources/AdminResource/Pages/EditAdmin.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AdminResource/Pages/EditAdmin.php)

📄 [Copy dari: app/Filament/Resources/AdminResource/Pages/ListAdmins.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AdminResource/Pages/ListAdmins.php)

### 9.2. Member Profile Resource

```bash
php artisan make:filament-resource MemberProfile --generate
```

📄 [Copy dari: app/Filament/Resources/MemberProfileResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/MemberProfileResource.php)

📄 [Copy dari: app/Filament/Resources/MemberProfileResource/Pages/CreateMemberProfile.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/MemberProfileResource/Pages/CreateMemberProfile.php)

📄 [Copy dari: app/Filament/Resources/MemberProfileResource/Pages/EditMemberProfile.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/MemberProfileResource/Pages/EditMemberProfile.php)

📄 [Copy dari: app/Filament/Resources/MemberProfileResource/Pages/ListMemberProfiles.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/MemberProfileResource/Pages/ListMemberProfiles.php)

📄 [Copy dari: app/Filament/Resources/MemberProfileResource/Pages/ViewMemberProfile.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/MemberProfileResource/Pages/ViewMemberProfile.php)

### 9.3. Registration Resource

```bash
php artisan make:filament-resource Registration --generate
```

📄 [Copy dari: app/Filament/Resources/RegistrationResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/RegistrationResource.php)

📄 [Copy dari: app/Filament/Resources/RegistrationResource/Pages/ListRegistrations.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/RegistrationResource/Pages/ListRegistrations.php)

📄 [Copy dari: app/Filament/Resources/RegistrationResource/Pages/ViewRegistration.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/RegistrationResource/Pages/ViewRegistration.php)

### 9.4. Category Resource

```bash
php artisan make:filament-resource Category --generate
```

📄 [Copy dari: app/Filament/Resources/CategoryResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/CategoryResource.php)

📄 [Copy dari: app/Filament/Resources/CategoryResource/Pages/CreateCategory.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/CategoryResource/Pages/CreateCategory.php)

📄 [Copy dari: app/Filament/Resources/CategoryResource/Pages/EditCategory.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/CategoryResource/Pages/EditCategory.php)

📄 [Copy dari: app/Filament/Resources/CategoryResource/Pages/ListCategories.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/CategoryResource/Pages/ListCategories.php)

### 9.5. Genre Resource (Baru!)

```bash
php artisan make:filament-resource Genre --generate
```

📄 [Copy dari: app/Filament/Resources/GenreResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/GenreResource.php)

📄 [Copy dari: app/Filament/Resources/GenreResource/Pages/CreateGenre.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/GenreResource/Pages/CreateGenre.php)

📄 [Copy dari: app/Filament/Resources/GenreResource/Pages/EditGenre.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/GenreResource/Pages/EditGenre.php)

📄 [Copy dari: app/Filament/Resources/GenreResource/Pages/ListGenres.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/GenreResource/Pages/ListGenres.php)

### 9.6. Author Resource

```bash
php artisan make:filament-resource Author --generate
```

📄 [Copy dari: app/Filament/Resources/AuthorResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AuthorResource.php)

📄 [Copy dari: app/Filament/Resources/AuthorResource/Pages/CreateAuthor.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AuthorResource/Pages/CreateAuthor.php)

📄 [Copy dari: app/Filament/Resources/AuthorResource/Pages/EditAuthor.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AuthorResource/Pages/EditAuthor.php)

📄 [Copy dari: app/Filament/Resources/AuthorResource/Pages/ListAuthors.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/AuthorResource/Pages/ListAuthors.php)

### 9.7. Publisher Resource

```bash
php artisan make:filament-resource Publisher --generate
```

📄 [Copy dari: app/Filament/Resources/PublisherResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/PublisherResource.php)

📄 [Copy dari: app/Filament/Resources/PublisherResource/Pages/CreatePublisher.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/PublisherResource/Pages/CreatePublisher.php)

📄 [Copy dari: app/Filament/Resources/PublisherResource/Pages/EditPublisher.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/PublisherResource/Pages/EditPublisher.php)

📄 [Copy dari: app/Filament/Resources/PublisherResource/Pages/ListPublishers.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/PublisherResource/Pages/ListPublishers.php)

### 9.8. Shelf Resource

```bash
php artisan make:filament-resource Shelf --generate
```

📄 [Copy dari: app/Filament/Resources/ShelfResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/ShelfResource.php)

📄 [Copy dari: app/Filament/Resources/ShelfResource/Pages/CreateShelf.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/ShelfResource/Pages/CreateShelf.php)

📄 [Copy dari: app/Filament/Resources/ShelfResource/Pages/EditShelf.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/ShelfResource/Pages/EditShelf.php)

📄 [Copy dari: app/Filament/Resources/ShelfResource/Pages/ListShelves.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/ShelfResource/Pages/ListShelves.php)

### 9.9. Book Resource

```bash
php artisan make:filament-resource Book --generate
```

📄 [Copy dari: app/Filament/Resources/BookResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/BookResource.php)

📄 [Copy dari: app/Filament/Resources/BookResource/Pages/CreateBook.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/BookResource/Pages/CreateBook.php)

📄 [Copy dari: app/Filament/Resources/BookResource/Pages/EditBook.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/BookResource/Pages/EditBook.php)

📄 [Copy dari: app/Filament/Resources/BookResource/Pages/ListBooks.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/BookResource/Pages/ListBooks.php)

📄 [Copy dari: app/Filament/Resources/BookResource/Pages/ViewBook.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/BookResource/Pages/ViewBook.php)

### 9.10. Loan Resource

```bash
php artisan make:filament-resource Loan --generate
```

📄 [Copy dari: app/Filament/Resources/LoanResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanResource.php)

📄 [Copy dari: app/Filament/Resources/LoanResource/Pages/CreateLoan.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanResource/Pages/CreateLoan.php)

📄 [Copy dari: app/Filament/Resources/LoanResource/Pages/EditLoan.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanResource/Pages/EditLoan.php)

📄 [Copy dari: app/Filament/Resources/LoanResource/Pages/ListLoans.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanResource/Pages/ListLoans.php)

📄 [Copy dari: app/Filament/Resources/LoanResource/Pages/ViewLoan.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanResource/Pages/ViewLoan.php)

### 9.11. Loan Rule Resource

```bash
php artisan make:filament-resource LoanRule --generate
```

📄 [Copy dari: app/Filament/Resources/LoanRuleResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanRuleResource.php)

📄 [Copy dari: app/Filament/Resources/LoanRuleResource/Pages/CreateLoanRule.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanRuleResource/Pages/CreateLoanRule.php)

📄 [Copy dari: app/Filament/Resources/LoanRuleResource/Pages/EditLoanRule.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanRuleResource/Pages/EditLoanRule.php)

📄 [Copy dari: app/Filament/Resources/LoanRuleResource/Pages/ListLoanRules.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/LoanRuleResource/Pages/ListLoanRules.php)

### 9.12. Fine Resource

```bash
php artisan make:filament-resource Fine --generate
```

📄 [Copy dari: app/Filament/Resources/FineResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/FineResource.php)

📄 [Copy dari: app/Filament/Resources/FineResource/Pages/ListFines.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/FineResource/Pages/ListFines.php)

### 9.13. Site Setting Resource

Buat manual karena ini single record.

📄 [Copy dari: app/Filament/Resources/SiteSettingResource.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/SiteSettingResource.php)

📄 [Copy dari: app/Filament/Resources/SiteSettingResource/Pages/ManageSiteSettings.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Resources/SiteSettingResource/Pages/ManageSiteSettings.php)

### 9.14. Admin Profile Page

```bash
php artisan make:filament-page AdminProfile
```

📄 [Copy dari: app/Filament/Pages/AdminProfile.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Pages/AdminProfile.php)

📄 [Copy dari: resources/views/filament/pages/admin-profile.blade.php](https://github.com/rifqi011/usk-perpus/blob/main/resources/views/filament/pages/admin-profile.blade.php)

### 9.15. Dashboard Page

📄 [Copy dari: app/Filament/Pages/Dashboard.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Pages/Dashboard.php)

### 9.16. Dashboard Widgets

Buat folder `app/Filament/Widgets/` dan copy semua file berikut:

📄 [Copy dari: app/Filament/Widgets/StatsOverview.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Widgets/StatsOverview.php)

📄 [Copy dari: app/Filament/Widgets/LatestLoans.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Widgets/LatestLoans.php)

📄 [Copy dari: app/Filament/Widgets/OverdueLoans.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Filament/Widgets/OverdueLoans.php)

---

## 10. Konfigurasi Filament Panel

### 10.1. Update AdminPanelProvider

**Edit: `app/Providers/Filament/AdminPanelProvider.php`**

📄 [Copy dari: app/Providers/Filament/AdminPanelProvider.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Providers/Filament/AdminPanelProvider.php)

### 10.2. Buat Middleware

```bash
php artisan make:middleware EnsureUserIsAdmin
```

**Edit: `app/Http/Middleware/EnsureUserIsAdmin.php`**

📄 [Copy dari: app/Http/Middleware/EnsureUserIsAdmin.php](https://github.com/rifqi011/usk-perpus/blob/main/app/Http/Middleware/EnsureUserIsAdmin.php)

### 10.3. Register Middleware

**Edit: `bootstrap/app.php`**

📄 [Copy dari: bootstrap/app.php](https://github.com/rifqi011/usk-perpus/blob/main/bootstrap/app.php)

Atau tambahkan manual:

```php
->withMiddleware(function (Middleware $middleware) {
    $middleware->alias([
        'admin' => \App\Http\Middleware\EnsureUserIsAdmin::class,
    ]);
})
```

---

## 11. Jalankan Migration & Seeder

```bash
php artisan migrate:fresh --seed
```

Output yang diharapkan:
```
Dropping all tables ........................... DONE
Creating migration table ...................... DONE
Running migrations ............................ DONE
Seeding database .............................. DONE
```

---

## 12. Setup Storage

```bash
php artisan storage:link
```

---

## 13. Install & Compile Assets

```bash
npm install
npm run build
```

---

## 14. Testing

### Start Server

```bash
php artisan serve
```

### Login

Buka: `http://localhost:8000/admin`

```
Email: superadmin@library.com
Password: password
```

### Test Fitur

1. ✅ Dashboard muncul
2. ✅ Semua menu muncul
3. ✅ Buat kategori baru
4. ✅ Buat genre baru
5. ✅ Buat penulis baru (dengan sosial media)
6. ✅ Buat penerbit baru
7. ✅ Buat buku baru (dengan SKU, stok awal, multiple genres)
8. ✅ Buat anggota baru (kode member auto-generate)
9. ✅ Buat peminjaman → select buku langsung dari stok yang tersedia
10. ✅ Proses pengembalian
11. ✅ Upload logo di Site Settings
12. ✅ Edit profil admin

---

## 15. Struktur Folder Akhir

```
library-app/
├── app/
│   ├── Actions/
│   │   ├── Loan/
│   │   │   ├── CalculateFineAction.php
│   │   │   ├── CreateLoanAction.php
│   │   │   └── ProcessReturnAction.php
│   │   ├── Member/
│   │   │   ├── ApproveRegistrationAction.php
│   │   │   └── RejectRegistrationAction.php
│   │   └── Reservation/
│   │       └── CreateReservationAction.php
│   ├── Enums/
│   │   ├── AccountType.php
│   │   ├── ActiveStatus.php
│   │   ├── BookCondition.php
│   │   ├── CopyStatus.php
│   │   ├── FineStatus.php
│   │   ├── FineType.php
│   │   ├── Gender.php
│   │   ├── LoanStatus.php
│   │   ├── MembershipStatus.php
│   │   ├── RegistrationStatus.php
│   │   └── ReservationStatus.php
│   ├── Filament/
│   │   ├── Pages/
│   │   │   ├── AdminProfile.php
│   │   │   └── Dashboard.php
│   │   ├── Resources/
│   │   │   ├── AdminResource/
│   │   │   ├── AuthorResource/
│   │   │   ├── BookResource/
│   │   │   ├── CategoryResource/
│   │   │   ├── FineResource/
│   │   │   ├── GenreResource/
│   │   │   ├── LoanResource/
│   │   │   ├── LoanRuleResource/
│   │   │   ├── MemberProfileResource/
│   │   │   ├── PublisherResource/
│   │   │   ├── RegistrationResource/
│   │   │   ├── ShelfResource/
│   │   │   └── SiteSettingResource/
│   │   └── Widgets/
│   │       ├── StatsOverview.php
│   │       ├── LatestLoans.php
│   │       └── OverdueLoans.php
│   ├── Http/
│   │   └── Middleware/
│   │       └── EnsureUserIsAdmin.php
│   ├── Models/
│   │   ├── AdminProfile.php
│   │   ├── Author.php
│   │   ├── Book.php
│   │   ├── Category.php
│   │   ├── Fine.php
│   │   ├── Genre.php
│   │   ├── Loan.php
│   │   ├── LoanDetail.php
│   │   ├── LoanRule.php
│   │   ├── MemberProfile.php
│   │   ├── Publisher.php
│   │   ├── Registration.php
│   │   ├── Reservation.php
│   │   ├── Role.php
│   │   ├── Shelf.php
│   │   ├── SiteSetting.php
│   │   └── User.php
│   ├── Providers/
│   │   └── Filament/
│   │       └── AdminPanelProvider.php
│   └── Services/
│       ├── FineService.php
│       ├── LoanService.php
│       └── ReservationService.php
├── database/
│   ├── migrations/
│   │   ├── 0001_01_01_000000_create_users_table.php
│   │   ├── 2024_01_01_000001_create_roles_table.php
│   │   ├── 2024_01_01_000002_create_role_user_table.php
│   │   ├── 2024_01_01_000003_create_admin_profiles_table.php
│   │   ├── 2024_01_01_000004_create_member_profiles_table.php
│   │   ├── 2024_01_01_000005_create_registrations_table.php
│   │   ├── 2024_01_01_000006_create_loan_rules_table.php
│   │   ├── 2024_01_01_000007_create_genres_table.php
│   │   ├── 2024_01_01_000007_create_categories_table.php
│   │   ├── 2024_01_01_000008_create_authors_table.php
│   │   ├── 2024_01_01_000009_create_publishers_table.php
│   │   ├── 2024_01_01_000010_create_shelves_table.php
│   │   ├── 2024_01_01_000011_create_books_table.php
│   │   ├── 2024_01_01_000012_create_book_authors_table.php
│   │   ├── 2024_01_01_000012_create_book_genres_table.php
│   │   ├── 2024_01_01_000013_create_loans_table.php
│   │   ├── 2024_01_01_000014_create_loan_details_table.php
│   │   ├── 2024_01_01_000015_create_reservations_table.php
│   │   ├── 2024_01_01_000016_create_fines_table.php
│   │   └── 2026_04_10_170641_create_site_settings_table.php
│   └── seeders/
│       ├── DatabaseSeeder.php
│       ├── RoleSeeder.php
│       ├── SuperAdminSeeder.php
│       ├── LoanRuleSeeder.php
│       └── SiteSettingSeeder.php
└── resources/
    └── views/
        └── filament/
            └── pages/
                └── admin-profile.blade.php
```

---

## 16. Fitur-Fitur yang Sudah Dibuat

### ✅ Manajemen User
- Admin dengan role (Admin & Super Admin)
- Member dengan kode otomatis (M-DDMMYY-XXXXXXXX)
- Profile management untuk admin
- Avatar di navbar

### ✅ Master Data
- Kategori
- Genre (many-to-many dengan Book)
- Penulis (dengan sosial media: Facebook, Twitter, Instagram, LinkedIn)
- Penerbit
- Rak
- Buku (dengan SKU, stok awal, multiple authors, multiple genres)

### ✅ Transaksi
- **Peminjaman dengan multiple books** (menggunakan Repeater, select langsung dari stok buku)
- Pengembalian dengan perhitungan denda keterlambatan
- Reservasi
- Denda

### ✅ Pengaturan
- **Multiple Loan Rules** (bisa banyak aturan)
- Site Settings (logo, nama, tagline, favicon)

### ✅ Fitur Tambahan
- Branding dinamis (logo & nama di navbar)
- Avatar admin di navbar
- Clock di navbar
- Navigation groups yang collapsible
- **Dashboard** dengan statistik, peminjaman terbaru, dan daftar terlambat

---

## 17. Checklist Development

- [ ] Install Laravel
- [ ] Install Filament
- [ ] Setup database
- [ ] Buat semua models & migrations
- [ ] Buat semua enums
- [ ] Buat seeders
- [ ] Buat actions & services
- [ ] Buat Filament resources
- [ ] Konfigurasi Filament panel
- [ ] Buat middleware
- [ ] Jalankan migration & seeder
- [ ] Setup storage link
- [ ] Compile assets
- [ ] Testing semua fitur
- [ ] Ubah password default
- [ ] Konfigurasi site settings

---

## 18. Tips Development

### Saat Membuat Model Baru:

```bash
# Buat model dengan migration
php artisan make:model NamaModel -m

# Edit migration di database/migrations/
# Edit model di app/Models/
# Tambahkan fillable, casts, relations
```

### Saat Membuat Resource Baru:

```bash
# Buat resource dengan pages
php artisan make:filament-resource NamaModel --generate

# Edit resource di app/Filament/Resources/
# Edit pages di app/Filament/Resources/NamaModelResource/Pages/
```

### Saat Update Database:

```bash
# Jalankan migration
php artisan migrate

# Atau reset database
php artisan migrate:fresh --seed
```

### Clear Cache:

```bash
php artisan config:clear
php artisan cache:clear
php artisan view:clear
php artisan route:clear
```

---

## 19. Repository GitHub

Semua source code lengkap ada di:

**https://github.com/rifqi011/usk-perpus**

Clone dan gunakan sebagai referensi!

---

## 20. Kesimpulan

Anda telah berhasil membuat Sistem Perpustakaan lengkap dengan fitur:

✅ Multiple loan rules
✅ Multiple books per transaction (dengan Repeater)
✅ Genre system (many-to-many)
✅ Author dengan sosial media
✅ Book dengan SKU & stok awal
✅ Peminjaman langsung dari stok buku
✅ Member dengan kode otomatis
✅ Admin profile management
✅ Dynamic branding
✅ Dan masih banyak lagi!

**Selamat! 🎉**

---

**Dibuat dengan ❤️ untuk Perpustakaan USK**
