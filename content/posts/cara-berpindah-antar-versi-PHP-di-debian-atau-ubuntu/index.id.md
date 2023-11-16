---
title: 'Cara Berpindah Antar Versi PHP di Ubuntu atau Debian'
date: 2023-11-02T10:00:00+07:00
draft: false
tags:
  - Debian
  - Ubuntu
  - PHP
showtoc: true
---

Repo `ondrej/php` adalah repositori pihak ketiga yang sangat populer di kalangan pengguna Ubuntu dan Debian. Repo ini memungkinkan Anda untuk menginstal beberapa versi PHP sekaligus. Keuntungan utama dari penggunaan repo ini adalah fleksibilitas dalam mengelola versi PHP yang berbeda tanpa konflik atau masalah yang rumit. Dalam artikel ini akan dibahas cara berpindah antar versi PHP di Ubuntu atau Debian.


## Kenapa Berpindah Versi?


Ada beberapa situasi di mana Anda perlu beralih antar versi PHP di server Anda, seperti:

1. __Aplikasi Khusus__: Beberapa aplikasi web memerlukan versi PHP yang lebih lama karena alasan kompatibilitas.

2. __Pengujian dan Pengembangan__: Anda perlu menguji aplikasi di berbagai versi PHP sebelum memutuskan versi mana yang paling sesuai.

3. __Pemeliharaan__: Saat melakukan pemeliharaan server, Anda dapat dengan mudah beralih ke versi PHP yang lebih baru untuk memanfaatkan fitur terbaru.

## Cara Berpindah Versi

Pertama, mari periksa versi PHP yang terpasang secara _default_ dengan menjalankan perintah berikut:

```bash
php -v
```

Hasilnya akan menunjukkan versi PHP yang terinstall, sebagai contoh:

```csharp
PHP 8.2.12 (cli) (built: Oct 27 2023 13:01:32) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.2.12, Copyright (c) Zend Technologies
    with Zend OPcache v8.2.12, Copyright (c), by Zend Technologies
```

Atur PHP versi tertentu sebagai _default_ dengan perintah:

```bash
sudo update-alternatives --set php /usr/bin/php8.1
```

Anda bisa mengganti __php8.1__ dengan versi PHP yang dimiliki

Alternatifnya, Anda dapat menjalankan perintah berikut untuk menentukan versi PHP mana yang akan digunakan secara _default_ di seluruh sistem:

```bash
sudo update-alternatives --config php
```

Hasilnya akan menunjukkan versi PHP yang dimiliki, sebagai contoh:

```csharp
There are 2 choices for the alternative php (providing /usr/bin/php).

  Selection    Path             Priority   Status
------------------------------------------------------------
  0            /usr/bin/php8.2   82        auto mode
  1            /usr/bin/php8.1   81        manual mode
* 2            /usr/bin/php8.2   82        manual mode

Press <enter> to keep the current choice[*], or type selection number: 
```

Pilih nomor yang sesuai dengan versi PHP yang ingin Anda gunakan sebagai _default_ atau cukup tekan __ENTER__ untuk mempertahankan pilihan saat ini.

## Kesimpulan

Mengelola versi PHP di Ubuntu atau Debian menjadi lebih mudah berkat repo `ondrej/php` dan perintah `update-alternatives` Anda dapat dengan cepat beralih antara versi PHP yang berbeda tanpa mengalami kesulitan. Dengan fleksibilitas ini, Anda dapat memenuhi kebutuhan aplikasi Anda dengan mudah dan efisien, menjadikan pengelolaan versi PHP lebih lancar dalam pengembangan dan pemeliharaan server Anda.