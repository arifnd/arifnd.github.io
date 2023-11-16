---
title: 'Mengarahkan apt-get Menggunakan IPv4 atau IPv6 pada Ubuntu atau Debian'
date: 2023-10-31T11:45:00+07:00
draft: false
tags:
  - Debian
  - Ubuntu
  - apt
  - apt-get
showtoc: true
---

Pada komputer yang mengaktifkan IPv6, beberapa program dan perintah akan lebih memilih salah satu jenis koneksi, yaitu IPv4 atau IPv6. Dalam panduan ini, kita akan belajar bagaimana mengarahkan perintah `apt-get` agar hanya menggunakan IPv4 atau IPv6 pada Ubuntu dan Debian.

## Opsi Cepat dengan Baris Perintah

Jika ingin mengarahkan `apt-get` ke IPv4 atau IPv6 tanpa melakukan perubahan pengaturan, pada perintah `apt-get` dapat ditambahkan opsi `Acquire::ForceIPv4` atau `Acquire::ForceIPv6`. Opsi ini tersedia setelah versi `0.9.7.9~exp1`, untuk memastikan versi `apt-get` lebih baru dari `0.9.7.9~exp1` dengan menjalankan perintah berikut:

```bash
apt-get --version
```

Hasilnya akan seperti di bawah ini:

```csharp
apt 2.2.4 (amd64)
```

Jika versi sudah sesuai, Anda dapat memaksa penggunaan IPv4 dengan perintah:

```bash
sudo apt-get -o Acquire::ForceIPv4=true update
```

atau IPv6:

```bash
sudo apt-get -o Acquire::ForceIPv6=true update
```

Ini akan mengupdate repositori dan mengarahkan URL di `sources.list` ke IPv4 atau IPv6 saja.

## Opsi yang Persisten

Untuk membuat pengaturan ini persisten, buat berkas `99force-ipv4` di direktori `/etc/apt/apt.conf.d/`:

```bash
sudo vi /etc/apt/apt.conf.d/99force-ipv4
```

Kemudian, masukkan tambahkan teks berikut:

```csharp
Acquire::ForceIPv4 "true";
```

Simpan berkas, dan selesai. Jika ingin memaksa penggunaan IPv6, ganti `Acquire::ForceIPv4` menjadi `Acquire::ForceIPv6` dalam berkas tersebut. Untuk memilih yang lebih baik diantara keduanya, Anda dapat mengujinya dengan menggunakan opsi cepat dan melihat mana yang berfungsi lebih baik.

## Opsi yang Persisten dengan bash shell aliases

Anda juga dapat menggunakan bash shell aliases agar pengaturan ini tetap persisten. Tambahkan baris berikut ke berkas` ~/.bashrc`:

```bash
## Selalu gunakan IPv6 ##
alias apt-get='sudo apt-get -o Acquire::ForceIPv6=true'
```

Untuk mengatur penggunaan IPv4, cukup ubah baris tersebut menjadi:

```bash
alias apt-get='sudo apt-get -o Acquire::ForceIPv4=true'
```

## Pengujian

Sekarang, Anda bisa mengarahkan `apt-get` untuk menggunakan IPv4 atau IPv6 dan mengujinya. Gunakan perintah `apt-get` berikut:

```bash
sudo apt-get update
```

Anda dapat menggantinya dengan perintah `apt-get` lain sesuai kebutuhan. Hasilnya akan mengonfirmasi bahwa `apt-get` hanya akan menggunakan protokol IPv4 atau IPv6 yang telah Anda atur.

## Kesimpulan

Anda telah belajar cara mengarahkan perintah `apt-get` pada Ubuntu atau Debian untuk menggunakan IPv4 atau IPv6 sesuai kebutuhan. Ini akan membantu menghindari masalah _timeout_ yang mungkin terjadi saat terhubung ke alamat IPv6. Pastikan server Anda dikonfigurasi dengan benar untuk mendukung kedua jenis jaringan IPv4 dan IPv6 dalam mode _dual-stack_. Jika Anda mengalami masalah dengan IPv6, disarankan untuk mencari penyebab masalahnya. Anda juga dapat merujuk ke manual `apt-get` dengan menggunakan perintah `man apt-get` atau `man apt` untuk informasi lebih lanjut.

## Referensi

1. [Force Apt-Get to IPv4 or IPv6 on Ubuntu or Debian](https://www.vultr.com/docs/force-apt-get-to-ipv4-or-ipv6-on-ubuntu-or-debian/)

2. [How To Use apt-get with IPv4 or IPv6 Transport (address) on a Ubuntu or Debian or Mint Linux](https://www.cyberciti.biz/faq/howto-use-apt-get-with-ipv6-or-ipv4-transport-on-ubuntu-debian/)