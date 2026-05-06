````markdown
# 🛒 SISTEM KASIR DESKTOP (POINT OF SALE)

## 📘 Deskripsi Proyek

Sistem Kasir Desktop merupakan aplikasi **Point of Sale (POS)** yang dirancang untuk membantu proses transaksi penjualan pada toko secara cepat, aman, dan terstruktur.

Sistem ini dibuat menggunakan konsep database relasional modern sehingga mampu mendukung proses transaksi layaknya sistem POS minimarket profesional.

---

# 👨‍💻 Kelompok: LoopLab

## 👥 Anggota Kelompok

| No | Nama |
|---|---|
| 1 | Arzizah Dwiyanti Dasopang |
| 2 | Muhammad Wildan Zulfahmi |
| 3 | Naufal Imania |

---

# 🎯 Tujuan Sistem

Sistem dibuat untuk:

- Mempermudah proses transaksi penjualan
- Mengelola data produk dan stok
- Mengurangi kesalahan perhitungan manual
- Menyimpan histori transaksi secara aman
- Mendukung sistem diskon dan member
- Mempermudah monitoring penjualan

---

# ✨ Fitur Utama Sistem

## 🔐 Login Multi User

Sistem mendukung beberapa role pengguna:

- Admin
- Manager
- Kasir

---

## 📦 Manajemen Produk

Fitur untuk:

- Menambah produk
- Mengedit produk
- Menghapus produk
- Mengatur kategori produk
- Mengatur stok barang

---

## 🏷️ Sistem Diskon

Mendukung:

- Diskon produk
- Diskon berdasarkan hari
- Banyak diskon dalam satu produk
- Pemilihan diskon terbesar otomatis
- Histori penggunaan diskon

---

## 👤 Sistem Member

Member mendapatkan:

- Diskon tambahan
- Histori transaksi
- Poin member

---

## 💳 Transaksi Penjualan

Kasir dapat:

- Scan barcode
- Input barang manual
- Mengatur jumlah barang
- Menghitung total otomatis
- Menghitung kembalian otomatis

---

## 📊 Histori Stok

Sistem mencatat:

- Stok masuk
- Stok keluar
- Perubahan stok otomatis
- Histori aktivitas stok

---

# 🖥️ Alur Kerja Sistem

---

# 1️⃣ Login User

Pengguna melakukan login menggunakan akun yang tersedia.

Role pengguna:

| Role | Hak Akses |
|---|---|
| Admin | Mengelola seluruh sistem |
| Manager | Monitoring dan laporan |
| Kasir | Melakukan transaksi |

Data login disimpan pada tabel:

```text
users
````

---

# 2️⃣ Input Barang

Kasir dapat:

* Scan barcode
* Input manual

Sistem akan:

1. Mencari produk
2. Mengecek stok
3. Mengecek diskon aktif
4. Menghitung subtotal
5. Menghitung potongan
6. Menampilkan barang ke keranjang

---

# 3️⃣ Sistem Diskon

Sistem mendukung banyak jenis diskon dalam satu produk.

## 📌 Contoh

Produk:

```text
Indomie
```

Memiliki diskon:

* Promo Senin 10%
* Promo Brand 5%
* Promo Akhir Bulan 7%

Maka sistem otomatis memilih:

```text
10%
```

karena merupakan diskon terbesar.

---

# 4️⃣ Sistem Member

Member mendapatkan potongan tambahan setelah seluruh diskon produk dihitung.

## 📌 Contoh Perhitungan

```text
Total setelah diskon item = 100000
Diskon member = 5%
Potongan = 5000
Final = 95000
```

---

# 5️⃣ Pembayaran

Kasir menginput:

```text
Jumlah uang customer
```

Sistem akan menghitung:

* Total akhir
* Kembalian
* Status pembayaran

---

# 6️⃣ Histori Stok

Semua perubahan stok dicatat ke tabel:

```text
stock_movements
```

Tujuan:

* Audit stok
* Histori barang
* Monitoring stok
* Mencegah manipulasi data

---

# 7️⃣ Cancel Transaksi

Jika transaksi dibatalkan:

```text
Customer batal belanja
```

Maka sistem akan:

* Mengubah status transaksi menjadi `canceled`
* Mengembalikan stok
* Menyimpan histori transaksi

---

# 🗄️ Struktur Database

Database terdiri dari tabel:

| No | Nama Tabel                 |
| -- | -------------------------- |
| 1  | categories                 |
| 2  | products                   |
| 3  | discounts                  |
| 4  | discount_days              |
| 5  | discount_products          |
| 6  | members                    |
| 7  | users                      |
| 8  | transactions               |
| 9  | transaction_items          |
| 10 | transaction_item_discounts |
| 11 | stock_movements            |

---

# 📂 1. Tabel Categories

## Fungsi

Menyimpan kategori produk.

## Contoh

* Makanan
* Minuman
* Snack
* Elektronik

---

## Struktur Kolom

| Kolom      | Tipe Data    | Fungsi        |
| ---------- | ------------ | ------------- |
| id         | int          | Primary key   |
| name       | varchar(100) | Nama kategori |
| created_at | datetime2    | Waktu dibuat  |

---

## Relasi

```text
categories (1)
→ products (many)
```

---

# 📂 2. Tabel Products

## Fungsi

Menyimpan data produk yang dijual.

---

## Struktur Kolom

| Kolom       | Tipe Data | Fungsi          |
| ----------- | --------- | --------------- |
| id          | int       | Primary key     |
| category_id | int       | Relasi kategori |
| name        | varchar   | Nama produk     |
| barcode     | varchar   | Barcode produk  |
| price       | decimal   | Harga produk    |
| stock       | int       | Jumlah stok     |
| is_active   | bit       | Status aktif    |
| created_at  | datetime  | Waktu dibuat    |

---

## Relasi

### Ke Categories

```text
products many → 1 categories
```

### Ke Transaction Items

```text
products (1)
→ transaction_items (many)
```

### Ke Stock Movements

```text
products (1)
→ stock_movements (many)
```

### Ke Discount Products

```text
products (1)
→ discount_products (many)
```

---

## Fungsi `is_active`

Digunakan untuk:

* Menonaktifkan produk
* Menyembunyikan produk dari kasir
* Menjaga histori transaksi

---

# 📂 3. Tabel Discounts

## Fungsi

Menyimpan master data diskon.

---

## Struktur Kolom

| Kolom          | Fungsi            |
| -------------- | ----------------- |
| id             | Primary key       |
| name           | Nama diskon       |
| discount_type  | percent / nominal |
| discount_value | Nilai diskon      |
| start_date     | Tanggal mulai     |
| end_date       | Tanggal selesai   |
| is_active      | Status aktif      |
| created_at     | Waktu dibuat      |

---

## Relasi

### Ke Discount Days

```text
discounts (1)
→ discount_days (many)
```

### Ke Discount Products

```text
discounts (1)
→ discount_products (many)
```

### Ke Transaction Item Discounts

```text
discounts (1)
→ transaction_item_discounts (many)
```

---

# 📂 4. Tabel Discount Days

## Fungsi

Menentukan hari aktif suatu diskon.

---

## Struktur Kolom

| Kolom       | Fungsi          |
| ----------- | --------------- |
| id          | Primary key     |
| discount_id | Relasi discount |
| day_of_week | Hari aktif      |

---

## Relasi

```text
discount_days many → 1 discounts
```

---

# 📂 5. Tabel Discount Products

## Fungsi

Tabel penghubung:

```text
products ↔ discounts
```

---

## Kenapa Dibutuhkan?

Karena:

* Satu produk bisa memiliki banyak diskon
* Satu diskon bisa digunakan banyak produk

---

## Struktur Kolom

| Kolom       | Fungsi          |
| ----------- | --------------- |
| id          | Primary key     |
| discount_id | Relasi discount |
| product_id  | Relasi product  |

---

# 📂 6. Tabel Members

## Fungsi

Menyimpan data member pelanggan.

---

## Struktur Kolom

| Kolom      | Fungsi        |
| ---------- | ------------- |
| id         | Primary key   |
| name       | Nama member   |
| phone      | Nomor telepon |
| points     | Poin member   |
| created_at | Waktu daftar  |

---

## Relasi

```text
members (1)
→ transactions (many)
```

---

# 📂 7. Tabel Users

## Fungsi

Menyimpan akun pengguna sistem.

---

## Struktur Kolom

| Kolom      | Fungsi         |
| ---------- | -------------- |
| id         | Primary key    |
| name       | Nama user      |
| username   | Username login |
| password   | Password       |
| role       | Role user      |
| is_active  | Status aktif   |
| created_at | Waktu dibuat   |

---

## Relasi

```text
users (1)
→ transactions (many)
```

---

# 📂 8. Tabel Transactions

## Fungsi

Menyimpan header transaksi.

---

## Struktur Kolom

| Kolom          | Fungsi               |
| -------------- | -------------------- |
| id             | Primary key          |
| invoice_code   | Nomor invoice        |
| user_id        | Kasir                |
| total_price    | Total sebelum diskon |
| total_discount | Total potongan       |
| final_price    | Total akhir          |
| paid_amount    | Jumlah bayar         |
| change_amount  | Kembalian            |
| status         | Status transaksi     |
| created_at     | Waktu transaksi      |
| member_id      | Relasi member        |

---

## Relasi

### Ke Users

```text
transactions many → 1 users
```

### Ke Members

```text
transactions many → 1 members
```

### Ke Transaction Items

```text
transactions (1)
→ transaction_items (many)
```

---

## Status Transaksi

Contoh:

* pending
* paid
* canceled

---

# 📂 9. Tabel Transaction Items

## Fungsi

Menyimpan detail barang transaksi.

---

## Struktur Kolom

| Kolom          | Fungsi               |
| -------------- | -------------------- |
| id             | Primary key          |
| transaction_id | Relasi transaksi     |
| product_id     | Relasi produk        |
| product_name   | Snapshot nama produk |
| price          | Harga produk         |
| qty            | Jumlah               |
| subtotal       | Harga × qty          |
| discount_total | Total diskon item    |
| final_price    | Harga akhir item     |

---

## Relasi

### Ke Transactions

```text
transaction_items many → 1 transactions
```

### Ke Products

```text
transaction_items many → 1 products
```

### Ke Transaction Item Discounts

```text
transaction_items (1)
→ transaction_item_discounts (many)
```

---

# 📂 10. Tabel Transaction Item Discounts

## Fungsi

Menyimpan histori diskon saat transaksi.

---

## Struktur Kolom

| Kolom               | Fungsi                |
| ------------------- | --------------------- |
| id                  | Primary key           |
| transaction_item_id | Relasi item transaksi |
| discount_id         | Relasi discount       |
| discount_name       | Snapshot nama diskon  |
| discount_type       | Snapshot tipe         |
| discount_value      | Snapshot nilai        |
| amount_cut          | Total potongan        |

---

## Relasi

### Ke Transaction Items

```text
transaction_item_discounts many → 1 transaction_items
```

### Ke Discounts

```text
transaction_item_discounts many → 1 discounts
```

---

# 📂 11. Tabel Stock Movements

## Fungsi

Menyimpan histori perubahan stok.

---

## Struktur Kolom

| Kolom          | Fungsi           |
| -------------- | ---------------- |
| id             | Primary key      |
| product_id     | Relasi produk    |
| type           | IN / OUT         |
| qty            | Jumlah perubahan |
| reference_type | Sumber perubahan |
| reference_id   | ID sumber        |
| created_at     | Waktu perubahan  |

---

## Relasi

```text
stock_movements many → 1 products
```

---

# ⚙️ Trigger Database

## 📌 Pengertian Trigger

Trigger adalah prosedur otomatis pada database yang berjalan ketika terjadi:

* INSERT
* UPDATE
* DELETE

pada tabel tertentu.

---

# 🎯 Fungsi Trigger Pada Sistem

Digunakan untuk:

* Mengurangi stok otomatis
* Menyimpan histori stok
* Menjaga sinkronisasi transaksi

---

# ⏱️ Kapan Trigger Berjalan?

Trigger berjalan setelah:

```text
transaksi berhasil disimpan
```

biasanya saat:

```text
Klik Bayar
```

---

# 🛒 Sebelum Pembayaran

Barang masih berada di:

```text
keranjang sementara
```

Kasir masih bisa:

* Menambah barang
* Mengedit jumlah
* Menghapus barang

Belum ada:

* Insert transaksi
* Pengurangan stok
* Histori stok

---

# ✅ Setelah Pembayaran

Sistem akan:

1. INSERT transactions
2. INSERT transaction_items
3. Trigger berjalan
4. Stok berkurang otomatis
5. Histori stok dibuat otomatis

---

# 🔥 Trigger AFTER INSERT Transaction Items

## Fungsi

Digunakan untuk:

* Mengurangi stok produk
* Menyimpan histori stok keluar

---

# 💻 SQL Trigger

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

# 🧠 Penjelasan Trigger

## inserted

```text
inserted
```

merupakan tabel sementara bawaan SQL Server yang berisi data baru hasil INSERT.

---

## UPDATE Products

```sql
UPDATE p
SET p.stock = p.stock - i.qty
```

Digunakan untuk:

```text
mengurangi stok produk
```

---

## INSERT Stock Movements

```sql
INSERT INTO stock_movements
```

Digunakan untuk:

```text
menyimpan histori stok keluar
```

---

# ✅ Keuntungan Menggunakan Trigger

* Otomatis
* Konsisten
* Aman
* Histori stok lengkap
* Mengurangi human error
* Mirip sistem POS profesional

---
# 🔥 Trigger Cancel Transaksi

## 📌 Fungsi

Trigger ini digunakan untuk:

- Mengembalikan stok barang saat transaksi dibatalkan
- Menyimpan histori pengembalian stok
- Menjaga konsistensi data stok

---

# 🎯 Kapan Trigger Berjalan?

Trigger berjalan ketika:

```text
status transaksi berubah menjadi canceled
```

Contoh:

```text
paid → canceled
```

---

# 🧠 Cara Kerja Trigger

Saat transaksi dibatalkan:

1. Sistem mengambil seluruh item pada transaksi
2. Sistem mengembalikan stok produk
3. Sistem menyimpan histori stok masuk

---

# 📦 Contoh Kasus

## Sebelum Cancel

| Produk | Qty Dibeli |
|---|---|
| Indomie | 3 |
| Teh Botol | 2 |

Stok setelah pembayaran:

| Produk | Stok |
|---|---|
| Indomie | 7 |
| Teh Botol | 5 |

---

## Setelah Cancel

Status transaksi:

```text
canceled
```

Maka stok otomatis kembali:

| Produk | Stok Setelah Cancel |
|---|---|
| Indomie | 10 |
| Teh Botol | 7 |

---

# 💻 SQL Trigger

```sql
CREATE TRIGGER trg_transaction_cancel
ON transactions
AFTER UPDATE
AS
BEGIN
    SET NOCOUNT ON;

    IF UPDATE(status)
    BEGIN
        UPDATE p
        SET p.stock = p.stock + ti.qty
        FROM products p
        JOIN transaction_items ti
            ON p.id = ti.product_id
        JOIN inserted i
            ON ti.transaction_id = i.id
        JOIN deleted d
            ON d.id = i.id
        WHERE i.status = 'canceled'
          AND d.status <> 'canceled';

        INSERT INTO stock_movements
        (
            product_id,
            type,
            qty,
            reference_type,
            reference_id
        )
        SELECT
            ti.product_id,
            'in',
            ti.qty,
            'transaction_cancel',
            ti.transaction_id
        FROM transaction_items ti
        JOIN inserted i
            ON ti.transaction_id = i.id
        JOIN deleted d
            ON d.id = i.id
        WHERE i.status = 'canceled'
          AND d.status <> 'canceled';
    END
END;
```

---

# 🔍 Penjelasan Trigger

## 📌 `IF UPDATE(status)`

Digunakan untuk mengecek apakah kolom:

```text
status
```

mengalami perubahan.

---

## 📌 `inserted`

Berisi data:

```text
setelah update
```

---

## 📌 `deleted`

Berisi data:

```text
sebelum update
```

---

## 📌 Kondisi Trigger

```sql
i.status = 'canceled'
AND d.status <> 'canceled'
```

Artinya:

```text
transaksi baru saja berubah menjadi canceled
```

Tujuannya agar stok tidak kembali berkali-kali.

---

# 📦 UPDATE Produk

```sql
SET p.stock = p.stock + ti.qty
```

Digunakan untuk:

```text
mengembalikan stok barang
```

---

# 📊 INSERT Stock Movements

```sql
INSERT INTO stock_movements
```

Digunakan untuk:

```text
menyimpan histori stok masuk
```

karena transaksi dibatalkan.

---

# ✅ Keuntungan Trigger Cancel

- Pengembalian stok otomatis
- Histori stok tetap tercatat
- Mengurangi human error
- Data stok lebih konsisten
- Cocok untuk sistem POS modern

---

# 📌 Kesimpulan

Sistem Kasir Desktop (Point of Sale / POS) ini dirancang menggunakan konsep database relasional modern untuk membantu proses transaksi penjualan menjadi lebih cepat, aman, dan terstruktur.

Sistem mendukung berbagai fitur penting seperti:

- Login multi user
- Manajemen produk dan kategori
- Sistem diskon fleksibel
- Sistem member
- Histori transaksi
- Histori pergerakan stok
- Cancel transaksi
- Otomatisasi menggunakan trigger database

Struktur database dibuat dengan relasi yang jelas antar tabel sehingga data menjadi lebih konsisten dan mudah dikembangkan.

Penggunaan trigger database membantu sistem bekerja secara otomatis, seperti:

- Mengurangi stok saat transaksi berhasil
- Mengembalikan stok saat transaksi dibatalkan
- Menyimpan histori stok masuk dan keluar

Dengan adanya sistem histori stok pada tabel:

```text
stock_movements
```

setiap perubahan stok dapat dilacak sehingga membantu proses audit dan monitoring barang.

Sistem diskon juga dirancang lebih fleksibel karena satu produk dapat memiliki banyak diskon dan sistem otomatis memilih diskon terbesar yang aktif.

Selain itu, fitur cancel transaksi membuat sistem lebih realistis seperti POS minimarket profesional karena stok dapat kembali otomatis ketika transaksi dibatalkan.

Secara keseluruhan, sistem ini memiliki beberapa keunggulan:

- Otomatis
- Aman
- Konsisten
- Mengurangi human error
- Mudah dikembangkan
- Memiliki histori data lengkap
- Cocok untuk sistem kasir modern

Sistem Kasir Desktop ini cocok digunakan untuk:

- Projek sekolah
- PKL
- Capstone project
- Sistem kasir toko kecil dan menengah
- Simulasi POS minimarket

Dengan rancangan database dan trigger yang baik, sistem dapat menjadi dasar yang kuat untuk pengembangan aplikasi kasir desktop yang lebih kompleks di masa depan.

---

# 🚀 Teknologi yang Digunakan

| Teknologi             | Fungsi           |
| --------------------- | ---------------- |
| SQL Server            | Database         |
| Desktop App           | Aplikasi kasir   |
| Trigger SQL           | Otomatisasi stok |
| Relational Database   | Relasi data      |
| Point of Sale Concept | Sistem transaksi |

---

# 📖 Dokumentasi

Dokumentasi ini dibuat sebagai rancangan sistem dan struktur database untuk mendukung pengembangan aplikasi kasir desktop modern.

```
<img width="2552" height="1365" alt="diagramkolab-ERDpertransaksi drawio" src="https://github.com/user-attachments/assets/c1de136b-e802-4f8f-8574-f03b7b8a1989" />

```
