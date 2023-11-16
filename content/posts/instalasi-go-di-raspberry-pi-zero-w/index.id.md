---
title: 'Panduan Langkah Demi Langkah: Instalasi Go di Raspberry Pi Zero W'
date: 2023-11-11T00:00:00+07:00
draft: false
tags:
  - Debian
  - Golang
  - Linux
  - Raspberry Pi
  - RPi Zero W
showtoc: true
---

Raspberry Pi Zero W adalah komputer mini populer dan serbaguna yang menjalankan sistem operasi Linux. Go (Golang) adalah bahasa pemrograman yang efisien dan dapat digunakan untuk pengembangan perangkat lunak. Artikel ini akan memberikan panduan langkah demi langkah tentang cara menginstal Go pada Raspberry Pi Zero W.

Versi Go terbaru saat panduan ini ditulis adalah __1.21.4__. Pastikan Anda menyesuaikan versi sesuai dengan versi yang tersedia.

## Langkah 1: Periksa Arsitektur Raspberry Pi Anda
Sebelum mengunduh versi Go yang sesuai, Anda perlu memeriksa arsitektur Raspberry Pi Anda. Anda dapat melakukannya dengan perintah `uname -m`:

```bash
uname -m
```

Jika perintah ini mengembalikan `armv6l`, maka Anda harus mengunduh versi Go yang sesuai dengan arsitektur Raspberry Pi.

## Langkah 2: Unduh dan Ekstrak Go
Pertama-tama, unduh versi Go yang sesuai dengan arsitektur Raspberry Pi Anda. Anda dapat menggunakan perintah wget untuk mengunduh versi yang sesuai dari situs resmi Go:

```bash
wget https://go.dev/dl/go1.21.4.linux-armv6l.tar.gz
```

Selanjutnya, ekstrak arsip Go ke direktori `/usr/local` menggunakan perintah:

```bash
sudo tar -C /usr/local -xvf go1.21.4.linux-armv6l.tar.gz
```

## Langkah 3: Konfigurasi Variabel Lingkungan
Untuk dapat menggunakan Go dengan mudah, Anda perlu mengatur beberapa variabel lingkungan. Gunakan perintah berikut untuk mengedit file `.bashrc`:

```bash
cat >> ~/.bashrc << 'EOF'
export GOPATH=$HOME/go
export PATH=/usr/local/go/bin:$PATH:$GOPATH/bin
EOF
```

Kemudian, reload file konfigurasi bashrc agar perubahan tersebut diterapkan:

```bash
source ~/.bashrc
```
## Langkah 4: Periksa Instalasi Go
Untuk memastikan bahwa Go telah diinstal dengan benar, Anda dapat menjalankan perintah:

```bash
go version
```

Jika berhasil akan muncul versi Go yang telah terinstal:

```csharp
go version go1.21.4 linux/arm
```

## Kesimpulan

Anda sekarang memiliki Go yang terinstal pada Raspberry Pi Zero W Anda. Go adalah bahasa pemrograman yang kuat dan efisien, yang membuatnya menjadi pilihan yang baik untuk pengembangan perangkat lunak pada perangkat Raspberry Pi Anda. Selamat mengembangkan proyek-proyek yang menarik dengan Raspberry Pi dan Go!

## Referensi

[Install Go Lang on Raspberry Pi](https://gist.github.com/pcgeek86/0206d688e6760fe4504ba405024e887c)