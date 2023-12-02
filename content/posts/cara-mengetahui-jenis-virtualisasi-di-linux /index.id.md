---
title: 'Cara Mengetahui Jenis Virtualisasi di Linux '
date: 2023-12-02T07:00:00+07:00
draft: false
tags:
  - CentOS
  - Debian
  - hostnamectl
  - KVM
  - Redhat
  - OpenVZ
  - Ubuntu
showtoc: false
---

Dalam pengelolaan _Virtual Private Server_ (VPS), ada kalanya kita perlu mengetahui jenis virtualisasi yang digunakan. Salah satu cara untuk mendapatkan informasi ini adalah menggunakan aplikasi `hostnamectl`. Artikel ini akan membahas langkah-langkah untuk mengetahui jenis virtualisasi pada VPS menggunakan perintah `hostnamectl`.

## Cara Install `hostnamectl`

Sebelum kita dapat menggunakan `hostnamectl`, pastikan bahwa aplikasi ini sudah terinstal di sistem Anda. Untuk instalasi pada distro berbasis Debian (seperti Ubuntu), gunakan perintah:

```bash
sudo apt-get install systemd
```

Sedangkan untuk distro berbasis Redhat (seperti CentOS), gunakan perintah:

```bash
sudo yum install systemd
```

Setelah instalasi selesai, Anda dapat menggunakan perintah `hostnamectl status` untuk mengetahui informasi tentang sistem operasi dan jenis virtualisasi yang digunakan.

## Contoh Output

### OpenVZ Virtualization:
```bash
$ hostnamectl status
   Static hostname: dev.fnd.my.id
         Icon name: computer-container
           Chassis: container
        Machine ID: ef5dbcf4ea2845aa92c30e2a00b33ef6
           Boot ID: 778522b91c6a4931976a8fee56d1df06
    Virtualization: openvz
  Operating System: Debian GNU/Linux 11 (bullseye)
            Kernel: Linux 4.19.0
      Architecture: x86-64
```

### KVM Virtualization
```bash
$ hostnamectl status
   Static hostname: dev.fnd.my.id
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 5ceb8f5c910b765bdb9da53f975ab6fb
           Boot ID: e8c16d17cd7146e7adbe6698536a0289
    Virtualization: kvm
  Operating System: Debian GNU/Linux 11 (bullseye)
            Kernel: Linux 5.10.0-23-amd64
      Architecture: x86-64
```

## Kesimpulan

Dengan menggunakan perintah `hostnamectl`, Anda dapat mengidentifikasi jenis virtualisasi yang digunakan VPS Anda.

## Referensi

- [How to find out the virtualization type of an linux VPS?](https://serverfault.com/questions/595471/how-to-find-out-the-virtualization-type-of-an-linux-vps)