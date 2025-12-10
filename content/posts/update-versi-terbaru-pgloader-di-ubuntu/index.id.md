---
title: 'Update Versi Terbaru PGLoader di Ubuntu'
date: 2025-12-10T10:00:00+07:00
draft: false
images: []
tags:
  - pgloader
  - postgresql
  - ubuntu
  - database
showtoc: true
---

[PGLoader](https://pgloader.io/) adalah *tools* *open source* yang dapat mempermudah proses migrasi data ke PostgreSQL. Selain MySQL *tools* ini mendukung SQLite dan file CSV. PGLoader memiliki kelebihan beruupa kecepatan dalam menyalin data sekaligus mengubah format data secara otomatis, sehingga menjadi pilihan banyak *administrator database* saat melakukan migrasi. Alasan penting untuk melakukan *update* PGLoader adalah rilis [di GitHub](https://github.com/dimitri/pgloader) berhenti pada versi `3.6.9`, sedangkan di *repository* resmi PostgreSQL telah tersedia versi `3.6.10`, dan terakhir diperbarui pada Oktober 2025.

## Langkah 1: Tambahkan PostgreSQL PPA Repository Key

Pertama tambahkan *repository key*  PostgreSQL agar sistem Ubuntu dapat mengenali paket PGLoader resmi. Jalankan perintah berikut:

```bash
sudo apt-get install curl gnupg2 -y
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

Perintah ini memastikan bahwa *repository key* tersimpan di sistem agar dapat memverifikasi paket dari PostgreSQL .

## Langkah 2: Tambahkan Repository PostgreSQL

Selanjutnya, tambahkan *repository* PostgreSQL sesuai dengan versi Ubuntu yang digunakan, Jalankan perintah berikut:

```bash
echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/pgdg.list
```

Repository ini berisi paket PGLoader terbaru serta versi PostgreSQL yang kompatibel.

## Langkah 3: Perbarui Daftar Paket

Setelah repository ditambahkan, perbarui daftar paket di sistem agar Ubuntu mengenali paket terbaru:

```bash
sudo apt-get update
```

Perintah ini penting agar sistem mengetahui versi PGLoader yang tersedia saat ini.

## Langkah 4: Instal atau Perbarui PGLoader

Terakhir, jalankan perintah berikut untuk menginstal PGLoader atau memperbarui ke versi terbaru:

```bash
sudo apt-get install pgloader
```

Dengan langkah ini, sistem akan men-*download* versi PGLoader terbaru dari *repository* resmi PostgreSQL dan menggantikan versi lama jika tersedia.

## Kesimpulan

PGLoader adalah alat yang sangat membantu untuk migrasi data ke PostgreSQL. Meski versi di GitHub berhenti di `3.6.9`, namun repository PostgreSQL menyediakan versi `3.6.10` (update terakhir Oktober 2025), sehingga melakukan *update* sangat disarankan untuk mendapatkan perbaikan bug dan peningkatan performa terbaru. Dengan menambahkan repository resmi PostgreSQL, penulis bisa memastikan selalu menggunakan versi terbaru PGLoader di Ubuntu tanpa harus membangun dari source code, sehingga proses migrasi data menjadi lebih efisien dan aman.