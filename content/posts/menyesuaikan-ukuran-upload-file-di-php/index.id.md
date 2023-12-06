---
title: 'Menyesuaikan Ukuran Upload File di PHP'
date: 2023-12-05T08:00:00+07:00
draft: false
tags:
  - Linux
  - NGINX
  - PHP
showtoc: false
---

Dalam pengembangan web, ada kondisi di mana aplikasi kita membutuhkan kemampuan untuk mengunggah berkas dengan ukuran yang cukup besar. Sayangnya, konfigurasi bawaan PHP terbatas pada ukuran maksimal sebesar 2MB. Artikel ini bertujuan untuk membahas secara rinci bagaimana kita dapat menyesuaikan ukuran unggah berkas di lingkungan PHP yang berjalan pada sistem operasi Linux, dan bagaimana kita dapat melakukan konfigurasi tambahan jika menggunakan web server NGINX.

## Menyesuaikan Ukuran Upload File di PHP

1. Mulailah dengan membuka file konfigurasi PHP (php.ini) menggunakan teks editor. Lokasi file ini mungkin berbeda tergantung pada distribusi Linux yang Anda gunakan. Umumnya dapat ditemukan di salah satu dari lokasi berikut:

- `/etc/php/<versi-php>/apache2/php.ini`
- `/etc/php/<versi-php>/cli/php.ini`
- `/etc/php/<versi-php>/fpm/php.ini`

2. Cari dan atur nilai dari dua opsi berikut:
  ```ini
  upload_max_filesize = 100M
  post_max_size = 100M
  ```
Pastikan untuk mengganti nilai 100M sesuai dengan ukuran maksimal yang diinginkan. Di sini, 100M menunjukkan bahwa file dengan ukuran maksimal 100 MB dapat diunggah.

3. Simpan perubahan dan _restart_ web server Anda.

## Konfigurasi Ukuran Upload File untuk NGINX

Jika Anda menggunakan NGINX sebagai web server, perhatikan bahwa NGINX juga memiliki pengaturan tambahan yang perlu diubah.

1. Buka konfigurasi situs NGINX yang relevan. Biasanya dapat ditemukan di direktori `/etc/nginx/sites-available/default`.

2. Tambahkan atau perbarui blok `server` atau `location` dengan menambahkan opsi `client_max_body_size`:
```nginx
  server {
      # ...
      client_max_body_size 100M;
      # ...
  }
  ```
Ubah nilai sesuai dengan ukuran maksimum yang diinginkan.

3. Simpan perubahan dan restart NGINX.

## Kesimpulan

Dengan menyesuaikan konfigurasi PHP dan konfigurasi webserver NGINX, Anda dapat mengelola ukuran maksimal file yang dapat diunggah oleh pengguna ke server web Anda. Pastikan untuk mengganti nilai sesuai dengan kebutuhan dan kapasitas server Anda. Dengan demikian, aplikasi web Anda dapat mengakomodasi unggahan berkas dengan ukuran yang lebih besar sesuai dengan kebutuhan proyek Anda.