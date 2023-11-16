---
title: 'Membangun Yarr di Raspberry Pi Zero W'
date: 2023-11-14T08:00:00+07:00
draft: false
cover: 
  image: featured-image.png
  caption: Tangkapan layar aplikasi Yarr
tags:
  - Debian
  - Golang
  - Linux
  - Raspberry Pi
  - RPi Zero W
  - Yarr
showtoc: true
---

Yarr (_Yet Another RSS Reader_) adalah aplikasi agregator berita berbasis web yang dapat digunakan sebagai aplikasi desktop atau server yang di-_host_ sendiri. Namun, Raspberry Pi Zero W memiliki arsitektur ARMv6 yang tidak didukung oleh binary standar Yarr. Untuk menjalankan Yarr di Raspberry Pi Zero W, Anda perlu membangunnya secara manual. Artikel ini akan memberikan panduan langkah demi langkah tentang cara membangun dan menjalankan Yarr di Raspberry Pi Zero W, serta bagaimana membuat layanan `systemd` agar Yarr tetap berjalan saat Raspberry Pi _booting_ atau _restart_.

Versi Go saat panduan ini ditulis adalah __1.21.4__. Pastikan Anda menyesuaikan dengan versi yang terbaru. Anda juga perlu memastikan bahwa Go sudah terinstal di Raspberry Pi Anda. Untuk panduan instalasi Go di Raspberry Pi, Anda dapat merujuk ke [panduan ini]({{< ref "../instalasi-go-di-raspberry-pi-zero-w/index.id.md" >}})

## Langkah 1: Unduh Repositori Yarr
Pertama, Anda perlu mengunduh kode sumber Yarr. Anda dapat melakukannya dengan menjalankan perintah berikut:

```bash
git clone https://github.com/nkanaev/yarr.git
```

## Langkah 2: Sesuaikan Makefile
Selanjutnya, buka file `makefile` dalam direktori proyek Yarr dan tambahkan `target build` untuk Raspberry Pi Zero W. Anda dapat melakukannya dengan mengedit `makefile` seperti berikut:

```make
build_linux_arm6:
        mkdir -p _output/linux
        GOOS=linux GOARCH=arm GOARM=6 go build -tags "sqlite_foreign_keys linux" -ldflags="$(GO_LDFLAGS)" -o _output/linux/yarr.arm6 ./cmd/yarr
```

Target build_linux_arm6 ini akan menghasilkan _binary_ Yarr untuk arsitektur Raspberry Pi Zero W (ARMv6). Pastikan Anda menyimpan perubahan pada `makefile`.

## Langkah 3: Bangun Yarr

Setelah Anda telah mengedit `makefile`, sekarang saatnya untuk membangun Yarr. Gunakan perintah berikut:

```bash
make build_linux_arm6
```

Ini akan menghasilkan _binary_ Yarr untuk Raspberry Pi Zero W di direktori `_output/linux`. Anda dapat menjalankan _binary_ ini di Raspberry Pi Zero W Anda.

## Langkah 4: Salin ke `.local/bin` 

Agar aplikasi dapat diakses secara global saat login, Anda perlu memindahkan _binary_ Yarr ke direktory `.local/bin` dengan menjalankan perintah:

```bash
cp _output/linux/yarr.arm6 ~/.local/bin/yarr
```

## Langkah 5: Buat Layanan systemd

Untuk menjalankan Yarr sebagai layanan pada Raspberry Pi Anda, buat berkas layanan `systemd` dengan nama `yarr.service` dalam direktori `/etc/systemd/system/`. Gantilah `user` dengan nama pengguna yang sesuai dan sesuaikan port yang digunakan sesuai kebutuhan Anda. Berikut contoh isi dari berkas `yarr.service`:

```plaintext
/etc/systemd/system/yarr.service
[Unit]
Description=Yarr Service
After=network.target

[Service]
User=user
ExecStart=/home/user/.local/bin/yarr -addr "0.0.0.0:7070"
WorkingDirectory=/home/user/.local/bin
Restart=always

[Install]
WantedBy=multi-user.target
```

## Langkah 6: Mulai Layanan dan Aktifkan Autostart

Setelah membuat berkas layanan, jalankan perintah berikut untuk memulai layanan Yarr dan mengaktifkan _autostart_ saat Raspberry Pi Zero W _booting_:

```bash
sudo systemctl start yarr
sudo systemctl enable yarr
```

## Langkah 7: Cek Status Layanan

Anda dapat memeriksa status layanan dengan perintah berikut:

```bash
sudo systemctl status yarr
```

Anda harus melihat bahwa layanan Yarr berjalan tanpa masalah dan aktif.

## Langkah 8: Akses aplikasi

Anda dapat mengakses aplikasi Yarr dengan mengakses alamat __localhost:7070__ atau __ip_komputer:7070__. 


## Kesimpulan

Dengan langkah-langkah di atas, Anda telah berhasil membangun dan menjalankan Yarr di Raspberry Pi Zero W yang memiliki arsitektur ARMv6, serta membuatnya berjalan pada saat _booting_ atau _reboot_. Yarr akan berjalan sebagai layanan dan akan secara otomatis memulai saat Raspberry Pi dinyalakan. Anda dapat mengakses Yarr melalui web browser dengan mengunjungi alamat IP Raspberry Pi di port yang Anda tentukan.