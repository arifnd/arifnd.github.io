---
title: "Menyesuaikan ukuran swap file di Raspberry Pi Zero W"
date: 2019-11-12T13:30:00+07:00
draft: false
resources:
- name: "featured-image"
  src: "featured-image.png"
tags:
  - Debian
  - Linux
  - PHP
  - Raspberry Pi
  - RPi Zero W
  - SWAP
showtoc: true
---
Berawal dari kendala ketika memasang paket PHP melalui composer.

```
PHP Fatal error: Uncaught exception 'ErrorException' with message 'proc_open(): fork failed - Cannot allocate memory' in phar
```

Selanjutnya setelah melakukan pencarian, diketahui bahwa penyebabnya adalah kurangnya memori ataupun swap yang dimiliki oleh sistem. Dikarenakan menjalankan composer pada Board Raspberry Pi Zero W yang memiliki memori 512MB dan swap bawaan 100MB. Sementara composer mensyaratkan minimal memori maupun swap yang dimiliki adalah 1GB.

Dari kondisi tersebut solusi yang memungkinkan dilakukan adalah menyesuaikan alokasi swap pada Raspberry Pi Zero W menjadi 1GB atau lebih. Berikut ini adalah langkah-langkahnya:

1. Buka berkas ```/etc/dphys-swapfile```.
2. Cari bagian ```CONF_SWAPSIZE=100```.
3. ubah nilai ```100``` sesuai ukuran swap yang diinginkan.
4. Agar perubahan terapkan oleh sistem, jalankan perintah ```/etc/init.d/dphys-swapfile restart```.

Setelah menyesuaikan ukuran swap pada Raspberry Pi Zero W. Pemasangan paket dengan composer berjalan dengan lancar.

**Referensi:**
1. [composer proc_open(): fork failed errors](https://getcomposer.org/doc/articles/troubleshooting.md#proc-open-fork-failed-errors)
2. [Raspberry Pi: How to set up swap space?](https://raspberrypi.stackexchange.com/questions/70/how-to-set-up-swap-space)