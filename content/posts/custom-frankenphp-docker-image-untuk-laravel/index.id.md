---
title: 'Custom FrankenPHP Docker Image untuk Laravel'
date: 2025-12-08T11:00:00+07:00
draft: false
images: []
tags:
  - frala
  - laravel
  - docker
  - FrankenPHP
  - PHP
  - DevOps
showtoc: true
---

Saat pertama kali mencoba [FrankenPHP](https://frankenphp.dev/), penulis sangat terkesan dengan performanya. Ringan, cepat, modern, dan benar-benar membawa angin segar untuk ekosistem PHP. Namun penulis menemukan satu masalah, [Docker](https://www.docker.com/) image bawaan FrankenPHP tidak bisa langsung digunakan untuk [Laravel](https://laravel.com/).

Laravel membutuhkan beberapa ekstensi tambahan, penyesuaian direktori, dan optimasi tertentu agar berjalan dengan baik. Beberapa ekstensi penting seperti `pdo_mysql`, `intl`, hingga `zip` tidak tersedia secara _default_. Sehingga, saat mencoba menjalankan proyek Laravel, penulis diminta untuk memasang ekstensi PHP yang dibutuhkan.

Agar tidak terus membangun secara manual setiap kali memulai proyek baru, akhirnya penulis memutuskan membuat Docker _image_ untuk FrankenPHP yang sudah siap pakai dan mendukung penuh Laravel. Berangkat dari tujuannya sederhana:

> "Docker image yang cukup dibanguun sekali, dan proyek Laravel bisa langsung jalan tanpa konfigurasi tambahan."

Dari situlah lahir _image_ [arifnd/frala](https://hub.docker.com/r/arifnd/frala), sebuah FrankenPHP image yang telah dikustomisasi, dioptimalkan, dan siap digunakan untuk Laravel versi 11+ maupun [Laravel Octane](https://laravel.com/docs/12.x/octane).

## Fitur Utama

- Sudah terpasang [Composer](https://getcomposer.org/)

- Mendukung PHP `8.2`, `8.3`, `8.4`, dan `8.5`

- Optimasi penuh untuk Laravel 11+

- Dukungan lengkap untuk Laravel Octane

- Performa tinggi karena menggunakan FrankenPHP berbasis [Caddy](https://caddyserver.com/)

- Sudah terpasang Ekstensi PHP wajib untuk Laravel:

    - intl

    - pcntl

    - pdo_mysql

    - pdo_pgsql

    - redis

    - zip

_Image_ ini penulis buat agar Laravel bisa langsung berjalan tanpa _patching_, tanpa _compile_ ulang, tanpa repot uji coba.

## Cara Menggunakan

### Menggunakan `docker run`

#### Install Dependensi Composer

Menggunakan _image_ Composer:

```bash
docker run --rm -it -v $PWD:/app composer:latest install
```

Atau langsung melalui _image_ ini:

```bash
docker run --rm -it -v $PWD:/app arifnd/frala composer install
```

Jika sudah memiliki Composer lokal:

```bash
composer install
```

#### Menjalankan Proyek

```bash
docker run -p 8000:80 \
  -v $(pwd):/app \
  arifnd/frala
```

### Menggunakan `Dockerfile`

Contoh `Dockerfile` untuk Proyek Laravel

```dockerfile
FROM arifnd/frala

ENV SERVER_NAME=:80

WORKDIR /app

COPY . .

RUN composer install \
    --prefer-dist \
    --no-progress \
    --no-interaction \
    --no-dev \
    --optimize-autoloader \
    && php artisan optimize \
    && chown -R www-data:www-data /app \
    && chmod -R 755 /app/storage /app/bootstrap/cache
```

#### Lalu Build dan Jalankan Image

```bash
docker build -t my-laravel-app .
docker run -p 8000:80 my-laravel-app
```

### Menggunakan `docker compose`

Contoh `compose.yaml` untuk Laravel

```yaml
services:
  app:
    image: arifnd/frala
    ports:
      - "8000:80"
    volumes:
      - .:/app
    environment:
      - APP_ENV=local
      - APP_DEBUG=true
      - SERVER_NAME=:80
```

### Menggunakan Laravel Octane

Jika ingin performa maksimal, bisa menjalankan Laravel menggunakan Octane dengan FrankenPHP:

```yaml
services:
  frankenphp:
    image: arifnd/frala
    entrypoint: php artisan octane:frankenphp --workers=1 --max-requests=1
    ports:
      - "8000:8000"
    environment:
      - APP_ENV=local
      - APP_DEBUG=true
      - SERVER_NAME=:80
    volumes:
      - .:/app
```

Dengan konfigurasi ini, Laravel berjalan cepat dan efisien, cocok untuk API atau aplikasi _heavy load_.

Dokumentasi lengkap untuk [FrankenPHP via Docker](https://laravel.com/docs/12.x/octane#frankenphp-via-docker)⁠.

### Membangun Image Sendiri

Bagi yang ingin membuat versi kustom lainnya, berikut berkas `Dockerfile` dasar yang dipakai:

```dockerfile
FROM dunglas/frankenphp

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

RUN install-php-extensions \
    @composer \
    intl \
    pcntl \
    pdo_mysql \
    pdo_pgsql \
    redis \
    zip
```

## Kesimpulan

Penulis membuat _image_ ini karena Docker _image_ bawaan FrankenPHP belum bisa langsung dipakai untuk Laravel, terutama bagi _developer_ yang ingin proses cepat, minim konfigurasi, namun memiliki performa tinggi. Dengan _image_ [arifnd/frala](https://hub.docker.com/r/arifnd/frala), Anda bisa langsung menjalankan Laravel tanpa mengkhawatirkan ekstensi, _permission_, atau konfigurasi server. Jika Anda sedang membangun aplikasi Laravel modern, terlebih yang membutuhkan performa tinggi dengan Octane, image ini bisa menjadi alternatif untuk dicoba.