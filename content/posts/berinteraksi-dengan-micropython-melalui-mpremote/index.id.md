---
title: 'Panduan Praktis: Berinteraksi dengan MicroPython melalui mpremote'
date: 2023-10-30T10:00:00+07:00
lastmod: 2023-10-30T14:39:00+07:00
draft: false
tags:
  - MicroPython
  - ESP32 Devkit V1
  - mpremote
showtoc: true
---

MicroPython adalah implementasi Python yang dirancang khusus untuk mikrokontroler dan perangkat _embedded_. MicroPython Remote Control, atau yang disebut sebagai `mpremote`, adalah aplikasi yang sangat berguna untuk berinteraksi, mengelola _filesystem_, dan mengotomatisasi perangkat MicroPython melalui koneksi serial. Dalam artikel ini, kita akan membahas cara berinteraksi dengan perangkat yang menjalankan MicroPython menggunakan `mpremote`, dengan fokus pada berbagai fungsi dan fitur yang ditawarkannya.

## Persiapan yang Dibutuhkan

Sebelum Anda mulai, pastikan Anda telah menyiapkan hal-hal berikut:

1. **Perangkat MicroPython**: Anda memerlukan perangkat yang menjalankan MicroPython. Seperti _board_ ESP8266, ESP32, atau perangkat lain yang mendukung MicroPython. Untuk proses instalasi Anda dapat membaca [Panduan Instalasi MicroPython pada ESP32 Devkit V1]({{< ref "../install-micropython-esp32-devkit-v1/index.id.md" >}})

2. **Kabel USB**: Anda memerlukan kabel USB untuk menghubungkan perangkat MicroPython ke komputer Anda.

3. **Komputer**: Anda akan memerlukan komputer atau laptop dengan port USB.

4. **Koneksi Internet**: Pastikan komputer Anda terhubung ke jaringan internet.

## Langkah-langkah untuk Berinteraksi dengan MicroPython Menggunakan `mpremote`

Berikut adalah langkah-langkah untuk berinteraksi dengan perangkat MicroPython menggunakan `mpremote` dan berbagai fungsi yang ditawarkannya:

### Instalasi `mpremote`

Sebelum Anda dapat mulai menggunakan `mpremote`, Anda perlu menginstalnya dengan `pip`, baik menggunakan `pip` reguler atau `pipx`. Berikut adalah cara instalasinya:

Dengan `pip`:

```bash
pip install --user mpremote
```

Dengan `pipx`:

```bash
pipx install mpremote
```

Jika Anda gagal menjalankan perintah di atas. Pastikan Python sudah terinstall di komputer Anda.

### Menggunakan `mpremote`

Salah satu cara paling sederhana untuk menggunakan `mpremote` adalah dengan menjalankannya tanpa argumen apa pun:

```bash
mpremote
```

Perintah di atas secara otomatis akan mendeteksi dan terhubung ke perangkat MicroPython pertama yang tersedia melalui koneksi serial USB. Ini akan memberikan terminal interaktif di mana Anda dapat mengakses REPL (_Read Eval Print Loop_) perangkat MicroPython dan melihat keluaran program Anda.

### Perintah-perintah `mpremote`

`mpremote` mendukung serangkaian perintah yang dapat Anda gunakan untuk berinteraksi dengan perangkat MicroPython dari jarak jauh. Berikut adalah beberapa perintah penting yang bisa Anda gunakan:

#### `connect`

Berfungsi untuk menghubungkan ke perangkat yang ditentukan.

#### `disconnect`

Berfungsi untuk memutus koneksi dari perangkat saat ini.

#### `resume`

Berfungsi untuk menjaga status _interpreter_ saat ini untuk perintah-perintah berikutnya.

#### `soft_reset`

Berfungsi untuk melakukan _sort reset_ pada perangkat.

#### `repl`

Berfungsi untuk masuk ke dalam REPL pada perangkat terhubung.

#### `eval`

Berfungsi untuk mengevaluasi dan mencetak hasil dari ekspresi Python.

#### `exec`

Berfungsi untuk mengeksekusi kode Python yang diberikan.

#### `run`

Berfungsi untuk menjalankan skrip dari sistem berkas lokal.

#### `fs`

Berfungsi untuk menjalankan perintah sistem berkas pada perangkat.

#### `df`

Berfungsi untuk menampilkan ruang bebas/digunakan pada perangkat.

#### `edit`

Berfungsi untuk mengedit file pada perangkat.

#### `mip`

Berfungsi untuk menginstal paket dari micropython-lib (atau GitHub) menggunakan alat `mip`.

#### `mount`

Berfungsi untuk mengaitkan direktori lokal ke perangkat komputer.

#### `unmount`

Berfungsi untuk mencabut pengaitan direktori lokal dari perangkat komputer.

#### `rtc`

Berfungsi untuk mengatur/mengambil waktu perangkat (RTC).

#### `sleep`

Berfungsi untuk menghentikan eksekusi sebelum menjalankan perintah berikutnya.

#### `reset`

Berfungsi untuk me-`reset` keras perangkat.

#### `bootloader`

Berfungsi untuk memasuki mode bootloader.

Dengan perintah-perintah ini, Anda dapat mengendalikan dan berinteraksi dengan perangkat MicroPython sesuai kebutuhan.

## Kesimpulan

`mpremote` adalah aplikasi yang sangat berguna untuk mengendalikan perangkat MicroPython Anda melalui koneksi serial. Dengan berbagai perintah dan opsi yang tersedia, Anda dapat menjalankan kode, mengelola berkas, dan melakukan banyak hal lainnya dengan mudah. Jika Anda adalah seorang pengembang IoT atau hanya ingin mengotomatisasi perangkat MicroPython Anda, `mpremote` adalah alat yang perlu Anda pertimbangkan. Selamat mencoba!

## Referensi

[MicroPython remote control: mpremote](https://docs.micropython.org/en/latest/reference/mpremote.html)