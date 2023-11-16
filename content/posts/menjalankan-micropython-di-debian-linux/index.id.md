---
title: 'Menjalankan MicroPython di Debian'
date: 2023-11-04T08:35:00+07:00
draft: false
tags:
  - Debian
  - MicroPython
showtoc: true
---

MicroPython adalah implementasi Python yang ringan dan efisien, awalnya dirancang khusus untuk mikrokontroler dan perangkat _embedded_. Namun, Anda juga dapat menjalankan MicroPython di sistem Linux berbasis Debian. Dalam artikel ini, kita akan mencoba menjalankan MicroPython di Debian.

## Langkah-langkah

Berikut adalah langkah-langkah untuk menjalankan MicroPython di Debian:

### Mendapatkan Kode Sumber

Pertama, Anda perlu mengunduh kode sumber MicroPython dari repositori resmi:

```bash
git clone https://github.com/micropython/micropython
cd micropython
```

Untuk mencoba versi tertentu anda bisa menjalankan perintah:

```bash
git checkout v1.21.0
```

Jika sukses, Anda akan melihat pesan berikut:

```csharp
Note: switching to 'v1.21.0'.
```

Untuk melihat semua versi MicroPython yang ada jalankan perintah `git tag`

### Instalasi Dependensi

Sebelum membangun MicroPython, Anda perlu menginstal beberapa dependensi. Jalankan perintah berikut untuk menginstalnya:

```bash
sudo apt-get install build-essential libffi-dev git pkg-config
```

Pastikan bahwa Python sudah terpasang. Saat ini, MicroPython masih mendukung Python 2, tetapi sebaiknya Anda menggunakan Python 3. Anda dapat memeriksa ketersediaan Python di sistem Anda dengan perintah:

```bash
python --version
```

### Membangun MicroPython cross-compiler

MicroPython memerlukan `mpy-cross` untuk kompilasi kode. Anda dapat membangunnya dengan langkah-langkah berikut:

```bash
make -C mpy-cross
```

Pastikan bahwa `mpy-cross` dibangun untuk arsitektur host, bukan arsitektur target.

Jika proses kompilasi berhasil, Anda akan melihat pesan yang mirip dengan ini:

```csharp
LINK build/mpy-cross
   text    data     bss     dec     hex filename
 306103   13696     872  320671   4e49f build/mpy-cross
```

### Membangun Port Unix MicroPython

Port Unix adalah versi MicroPython yang berjalan pada sistem Linux, macOS, dan sistem berbasis Unix lainnya. Ini sangat berguna untuk mengembangkan MicroPython karena Anda tidak perlu men-_deploy_ kode ke perangkat untuk mengujinya. Proses pembangunan serupa dengan penggunaan python binary di CPython.

Untuk membangun port Unix, pastikan semua dependensi yang berkaitan dengan Linux terinstal, seperti yang dijelaskan pada bagian [instalasi dependensi](#instalasi-dependensi). Pastikan juga bahwa Anda sudah menginstal `gcc` dan GNU `make`.

```bash
gcc --version
```

```csharp
gcc (Debian 10.2.1-6) 10.2.1 20210110
Copyright (C) 2020 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

Kemudian, Anda dapat membangun port Unix sebagai berikut:

```bash
cd ports/unix
make submodules
make
```

Jika proses pembangunan MicroPython berhasil, Anda akan melihat pesan berikut:

```csharp
LINK build-standard/micropython
   text    data     bss     dec     hex filename
 690237   67872    7120  765229   bad2d build-standard/micropython
 ```

Sekarang jalankan MicroPython:

```bash
./build-standard/micropython 
```

```csharp
MicroPython v1.22.0-preview.5.gd2a9d70c0 on 2023-11-02; linux [GCC 10.2.1] version
Use Ctrl-D to exit, Ctrl-E for paste mode
>>> print("halo dunia")
halo dunia
>>>
```

Atau jika menggunakan v1.21.0 

```csharp
MicroPython v1.21.0 on 2023-11-02; linux [GCC 10.2.1] version
Use Ctrl-D to exit, Ctrl-E for paste mode
>>> print("halo dunia")
halo dunia
>>>
```

## Kesimpulan

Dengan mengikuti langkah-langkah di atas, Anda dapat membangun dan menjalankan MicroPython di sistem Debian. Ini berguna untuk mengembangkan dan menguji MicroPython tanpa melalalui proses _deploy_ kode ke perangkat. MicroPython di Debian dapat digunakan untuk berbagai keperluan, dari scripting hingga pengembangan aplikasi. Selamat mencoba!

## Referensi

- [Building the Unix port of MicroPython](https://docs.micropython.org/en/latest/develop/gettingstarted.html#building-the-unix-port-of-micropython)