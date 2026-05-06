# 🛒 SISTEM KASIR DESKTOP (POINT OF SALE)

## 📘 Deskripsi Proyek

Sistem Kasir Desktop merupakan aplikasi **Point of Sale (POS)** yang dirancang untuk membantu proses transaksi penjualan pada toko secara cepat, aman, dan terstruktur.

Sistem ini dibuat menggunakan konsep database relasional modern sehingga mampu mendukung proses transaksi layaknya sistem POS minimarket profesional.

---

## 👥 Nama Siswa

Muhammad Wildan Zulfahmi

---

## 👨‍💻 Kelompok: LoopLab

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

# ⚙️ Trigger Database

## 📌 Pengertian Trigger

Trigger adalah prosedur otomatis pada database yang berjalan ketika terjadi:

* INSERT
* UPDATE
* DELETE

pada tabel tertentu.

---

# 🎯 Fungsi Trigger Pada Sistem POS

Trigger digunakan untuk:

* Mengurangi stok otomatis
* Mengembalikan stok otomatis
* Menyimpan histori stok
* Menjaga sinkronisasi transaksi
* Mengurangi human error
* Membantu audit stok

---

# ⏱️ Kapan Trigger Berjalan?

Trigger berjalan setelah transaksi berhasil diproses oleh sistem.

Contoh:

```text
Klik tombol bayar
```

atau ketika:

```text
status transaksi berubah menjadi canceled
```

---

# 🛒 Alur Sebelum Pembayaran

Barang masih berada di:

```text
keranjang sementara
```

Kasir masih bisa:

* Menambah barang
* Mengedit jumlah barang
* Menghapus barang

Pada tahap ini:

* Belum ada pengurangan stok
* Belum ada histori stok
* Belum ada transaksi permanen

---

# ✅ Alur Setelah Pembayaran

Sistem akan:

1. Menyimpan transaksi
2. Menyimpan detail barang transaksi
3. Trigger berjalan otomatis
4. Stok berkurang otomatis
5. Histori stok dibuat otomatis

---

# 🔥 Trigger AFTER INSERT Transaction Items

## 📌 Fungsi Trigger

Trigger ini digunakan untuk:

* Mengurangi stok produk otomatis
* Menyimpan histori stok keluar
* Menjaga konsistensi stok barang

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

## 📌 inserted

```text
inserted
```

merupakan tabel sementara bawaan SQL Server yang berisi data baru hasil INSERT.

---

## 📌 Pengurangan Stok

```sql
SET p.stock = p.stock - i.qty
```

Digunakan untuk:

```text
mengurangi stok produk otomatis
```

---

## 📌 Histori Stok Keluar

```sql
INSERT INTO stock_movements
```

Digunakan untuk:

```text
menyimpan histori stok keluar
```

---

# 📦 Contoh Proses

## Sebelum Pembayaran

| Produk  | Stok |
| ------- | ---- |
| Indomie | 10   |

Customer membeli:

```text
3 Indomie
```

---

## Setelah Pembayaran

Trigger berjalan otomatis:

```text
10 - 3 = 7
```

Maka stok menjadi:

| Produk  | Stok |
| ------- | ---- |
| Indomie | 7    |

Dan histori stok keluar otomatis tersimpan.

---

# 🔥 Trigger Cancel Transaksi

## 📌 Fungsi Trigger

Trigger ini digunakan untuk:

* Mengembalikan stok barang saat transaksi dibatalkan
* Menyimpan histori pengembalian stok
* Menjaga konsistensi data stok

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

1. Sistem mengambil seluruh item transaksi
2. Sistem mengembalikan stok produk
3. Sistem menyimpan histori stok masuk

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

# 🔍 Penjelasan Trigger Cancel

## 📌 `IF UPDATE(status)`

Digunakan untuk mengecek apakah kolom:

```text
status
```

mengalami perubahan.

---

## 📌 inserted

Berisi data:

```text
setelah update
```

---

## 📌 deleted

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

# 📦 Pengembalian Stok

```sql
SET p.stock = p.stock + ti.qty
```

Digunkan untuk:

```text
mengembalikan stok barang otomatis
```

---

# 📊 Histori Stok Masuk

```sql
INSERT INTO stock_movements
```

Digunakan untuk:

```text
menyimpan histori stok masuk
```

karena transaksi dibatalkan.

---

# 📦 Contoh Cancel Transaksi

## Sebelum Cancel

| Produk    | Qty Dibeli |
| --------- | ---------- |
| Indomie   | 3          |
| Teh Botol | 2          |

Stok setelah pembayaran:

| Produk    | Stok |
| --------- | ---- |
| Indomie   | 7    |
| Teh Botol | 5    |

---

## Setelah Cancel

Status transaksi:

```text
canceled
```

Maka stok otomatis kembali:

| Produk    | Stok Setelah Cancel |
| --------- | ------------------- |
| Indomie   | 10                  |
| Teh Botol | 7                   |

---

# ✅ Keuntungan Menggunakan Trigger

* Otomatis
* Konsisten
* Aman
* Mengurangi human error
* Histori stok lengkap
* Membantu audit stok
* Cocok untuk POS modern

---

# 📌 Kesimpulan

Trigger pada sistem kasir desktop digunakan untuk membantu otomatisasi proses pengelolaan stok dan histori transaksi.

Dengan trigger:

* Stok dapat berkurang otomatis saat transaksi berhasil
* Stok dapat kembali otomatis saat transaksi dibatalkan
* Histori stok masuk dan keluar dapat tercatat otomatis
* Data menjadi lebih konsisten dan aman

Penggunaan trigger juga membantu mengurangi kesalahan manual serta membuat sistem bekerja lebih profesional seperti aplikasi POS modern pada minimarket dan toko retail.

---

# 🖼️ Diagram Sistem


<img width="2552" height="1365" alt="diagramkolab-ERDpertransaksi drawio" src="https://github.com/user-attachments/assets/c1de136b-e802-4f8f-8574-f03b7b8a1989" />

