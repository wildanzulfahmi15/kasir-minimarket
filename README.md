# Sistem Kasir Desktop

## Deskripsi Sistem

Sistem Kasir Desktop adalah aplikasi Point of Sale (POS) yang digunakan untuk membantu proses transaksi penjualan pada sebuah toko.

Sistem ini mendukung fitur:

- Login multi user
- Role admin, manager, dan kasir
- Manajemen produk
- Manajemen kategori
- Sistem member
- Sistem diskon
- Diskon berdasarkan hari
- Diskon produk
- Transaksi penjualan
- Histori stok
- Monitoring aktivitas penjualan
- Perhitungan otomatis total transaksi
- Pengurangan stok otomatis

Sistem menggunakan database relasional SQL Server dengan struktur yang dirancang agar:

- Mudah dikembangkan
- Aman untuk histori transaksi
- Fleksibel untuk pengembangan fitur
- Mirip dengan sistem POS minimarket modern
- Mendukung histori diskon dan stok

---

# Gambaran Sistem

## 1. Login User

Pengguna login menggunakan akun:

- Admin
- Manager
- Kasir

Data login disimpan pada tabel:

```text
users
```

---

# 2. Input Barang

Kasir melakukan scan barcode atau input barang manual.

Sistem akan:

1. Mencari produk
2. Mengecek stok
3. Mengecek diskon aktif
4. Menghitung subtotal
5. Menghitung diskon
6. Menampilkan barang ke keranjang sementara

---

# 3. Sistem Diskon

Sistem mendukung:

- Diskon produk
- Diskon berdasarkan hari
- Banyak diskon pada 1 produk
- Pengambilan diskon terbesar
- Histori diskon transaksi

Contoh:

Produk:

```text
Indomie
```

Memiliki diskon:

- Promo Senin 10%
- Promo Brand 5%
- Promo Akhir Bulan 7%

Maka sistem memilih:

```text
10%
```

karena merupakan diskon terbesar.

---

# 4. Sistem Member

Member mendapatkan diskon tambahan.

Perhitungan:

1. Diskon produk dihitung dulu
2. Semua item dijumlahkan
3. Diskon member diterapkan ke total transaksi

Contoh:

```text
Total setelah diskon item = 100000
Diskon member = 5%
Potongan = 5000
Final = 95000
```

---

# 5. Pembayaran

Kasir menginput:

```text
Jumlah uang customer
```

Sistem menghitung:

- Total akhir
- Kembalian
- Status pembayaran

---

# 6. Histori Stok

Setiap perubahan stok dicatat ke tabel:

```text
stock_movements
```

Tujuan:

- Audit stok
- Histori pergerakan barang
- Laporan stok masuk dan keluar
- Mencegah manipulasi stok

---

# 7. Cancel Transaksi

Seluruh transaksi dapat dibatalkan.

Contoh:

```text
Customer batal semua belanja
```

Maka:

- Status transaksi menjadi canceled
- Semua stok dikembalikan
- Histori tetap tersimpan

---

# Struktur Database

Database terdiri dari tabel:

1. categories
2. products
3. discounts
4. discount_days
5. discount_products
6. members
7. users
8. transactions
9. transaction_items
10. transaction_item_discounts
11. stock_movements

---

# 1. Tabel Categories

## Fungsi

Menyimpan kategori produk.

Contoh:

- Makanan
- Minuman
- Snack
- Elektronik

---

## Struktur Kolom

| Kolom | Tipe Data | Fungsi |
|---|---|---|
| id | int | Primary key |
| name | varchar(100) | Nama kategori |
| created_at | datetime2 | Waktu dibuat |

---

## Relasi

```text
categories (1)
→ products (many)
```

Karena satu kategori memiliki banyak produk.

---

# 2. Tabel Products

## Fungsi

Menyimpan data barang yang dijual.

---

## Struktur Kolom

| Kolom | Tipe Data | Fungsi |
|---|---|---|
| id | int | Primary key |
| category_id | int | Relasi kategori |
| name | varchar | Nama produk |
| barcode | varchar | Barcode produk |
| price | decimal | Harga |
| stock | int | Jumlah stok |
| is_active | bit | Status aktif |
| created_at | datetime | Waktu dibuat |

---

## Relasi

### Ke Categories

```text
products many → 1 categories
```

---

### Ke Transaction Items

```text
products (1)
→ transaction_items (many)
```

---

### Ke Stock Movements

```text
products (1)
→ stock_movements (many)
```

---

### Ke Discount Products

```text
products (1)
→ discount_products (many)
```

---

## Fungsi is_active

Digunakan untuk:

- Menonaktifkan produk
- Menyembunyikan produk dari kasir
- Menjaga histori transaksi

---

# 3. Tabel Discounts

## Fungsi

Menyimpan master data diskon.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| name | Nama diskon |
| discount_type | percent / nominal |
| discount_value | Nilai diskon |
| start_date | Tanggal mulai |
| end_date | Tanggal selesai |
| is_active | Status aktif |
| created_at | Waktu dibuat |

---

## Relasi

### Ke Discount Days

```text
discounts (1)
→ discount_days (many)
```

---

### Ke Discount Products

```text
discounts (1)
→ discount_products (many)
```

---

### Ke Transaction Item Discounts

```text
discounts (1)
→ transaction_item_discounts (many)
```

---

# 4. Tabel Discount Days

## Fungsi

Menentukan hari aktif suatu diskon.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| discount_id | Relasi discount |
| day_of_week | Hari aktif |

---

## Relasi

```text
discount_days many → 1 discounts
```

---

# 5. Tabel Discount Products

## Fungsi

Tabel penghubung:

```text
products ↔ discounts
```

---

## Kenapa Dibutuhkan?

Karena:

- Satu produk bisa memiliki banyak diskon
- Satu diskon bisa digunakan banyak produk

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| discount_id | Relasi discount |
| product_id | Relasi product |

---

# 6. Tabel Members

## Fungsi

Menyimpan data member pelanggan.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| name | Nama member |
| phone | Nomor telepon |
| points | Poin member |
| created_at | Waktu daftar |

---

## Relasi

```text
members (1)
→ transactions (many)
```

---

# 7. Tabel Users

## Fungsi

Menyimpan akun pengguna sistem.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| name | Nama user |
| username | Username login |
| password | Password |
| role | Role user |
| is_active | Status aktif |
| created_at | Waktu dibuat |

---

## Relasi

```text
users (1)
→ transactions (many)
```

---

# 8. Tabel Transactions

## Fungsi

Menyimpan header transaksi.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| invoice_code | Nomor invoice |
| user_id | Kasir |
| total_price | Total sebelum diskon |
| total_discount | Total potongan |
| final_price | Total akhir |
| paid_amount | Jumlah bayar |
| change_amount | Kembalian |
| status | Status transaksi |
| created_at | Waktu transaksi |
| member_id | Relasi member |

---

## Relasi

### Ke Users

```text
transactions many → 1 users
```

---

### Ke Members

```text
transactions many → 1 members
```

---

### Ke Transaction Items

```text
transactions (1)
→ transaction_items (many)
```

---

## Status Transaksi

Contoh:

- pending
- paid
- canceled

---

# 9. Tabel Transaction Items

## Fungsi

Menyimpan detail barang transaksi.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| transaction_id | Relasi transaksi |
| product_id | Relasi produk |
| product_name | Snapshot nama produk |
| price | Harga produk |
| qty | Jumlah |
| subtotal | Harga x qty |
| discount_total | Total diskon item |
| final_price | Harga akhir item |

---

## Relasi

### Ke Transactions

```text
transaction_items many → 1 transactions
```

---

### Ke Products

```text
transaction_items many → 1 products
```

---

### Ke Transaction Item Discounts

```text
transaction_items (1)
→ transaction_item_discounts (many)
```

---

# 10. Tabel Transaction Item Discounts

## Fungsi

Menyimpan histori diskon yang digunakan saat transaksi.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| transaction_item_id | Relasi item transaksi |
| discount_id | Relasi discount |
| discount_name | Snapshot nama diskon |
| discount_type | Snapshot tipe |
| discount_value | Snapshot nilai |
| amount_cut | Total potongan |

---

## Relasi

### Ke Transaction Items

```text
transaction_item_discounts many → 1 transaction_items
```

---

### Ke Discounts

```text
transaction_item_discounts many → 1 discounts
```

---

# 11. Tabel Stock Movements

## Fungsi

Menyimpan histori perubahan stok.

---

## Struktur Kolom

| Kolom | Fungsi |
|---|---|
| id | Primary key |
| product_id | Relasi produk |
| type | IN / OUT |
| qty | Jumlah perubahan |
| reference_type | Sumber perubahan |
| reference_id | ID sumber |
| created_at | Waktu perubahan |

---

## Relasi

```text
stock_movements many → 1 products
```

---

# Trigger Database

## Pengertian Trigger

Trigger adalah prosedur otomatis di database yang berjalan ketika terjadi:

- INSERT
- UPDATE
- DELETE

pada tabel tertentu.

---

# Fungsi Trigger Pada Sistem Ini

Trigger digunakan untuk:

- Mengurangi stok otomatis
- Menyimpan histori stok otomatis
- Menjaga sinkronisasi stok dan transaksi

---

# Kapan Trigger Berjalan?

Trigger berjalan setelah:

```text
transaksi selesai disimpan
```

biasanya saat:

```text
Klik Bayar
```

---

# Sebelum Pembayaran

Sebelum transaksi final:

barang masih berada di:

```text
keranjang sementara
```

Kasir masih bisa:

- tambah barang
- edit qty
- hapus barang

menggunakan CRUD biasa.

Belum ada:

- insert transaksi
- pengurangan stok
- histori stok

---

# Setelah Pembayaran

Saat transaksi berhasil dibayar:

Sistem akan:

1. INSERT transactions
2. INSERT transaction_items
3. Trigger berjalan
4. Stok otomatis berkurang
5. Histori stok otomatis dibuat

---

# Trigger AFTER INSERT Transaction Items

## Fungsi

Digunakan untuk:

- Mengurangi stok produk
- Menyimpan histori stok keluar

---

# SQL Trigger

```sql
CREATE TRIGGER trg_transaction_item_insert
ON transaction_items
AFTER INSERT
AS
BEGIN
    SET NOCOUNT ON;

    UPDATE p
    SET p.stock = p.stock - i.qty
    FROM products p
    JOIN inserted i
        ON p.id = i.product_id;

    INSERT INTO stock_movements
    (
        product_id,
        type,
        qty,
        reference_type,
        reference_id
    )
    SELECT
        i.product_id,
        'out',
        i.qty,
        'transaction',
        i.transaction_id
    FROM inserted i;
END;
```

---

# Penjelasan Trigger

## inserted

```text
inserted
```

adalah tabel sementara bawaan SQL Server yang berisi data yang baru dimasukkan.

---

## UPDATE products

Bagian:

```sql
UPDATE p
SET p.stock = p.stock - i.qty
```

digunakan untuk:

```text
mengurangi stok produk
```

---

## INSERT stock_movements

Bagian:

```sql
INSERT INTO stock_movements
```

digunakan untuk:

```text
menyimpan histori stok keluar
```

---

# Keuntungan Menggunakan Trigger

- Otomatis
- Konsisten
- Aman
- Histori stok lengkap
- Mengurangi human error
- Mirip sistem POS profesional

---

# Kesimpulan

Struktur database ini dirancang untuk sistem kasir modern dengan fokus pada:

- Konsistensi data
- Histori transaksi aman
- Histori stok lengkap
- Fleksibilitas sistem diskon
- Pengembangan sistem di masa depan

Database ini cocok untuk:

- Projek sekolah
- PKL
- Capstone project
- Sistem kasir desktop
- POS toko kecil dan menengah
