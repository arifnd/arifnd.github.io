---
title: 'Mengatasi hostname Ubuntu 18.04 yang berubah ketika di-restart'
date: 2019-12-13T23:00:34+07:00
draft: false
tags:
  - Server
  - Ubuntu
showtoc: true
---
Hostname atau *host name* atau nama komputer adalah nama yang diberikan untuk mengenali perangkat dalam jaringan. Umumnya hostname memiliki nama yang berbeda ataupun unik. 

Pada ubuntu hostname dapat diubah menggunakan perintah ```hostnamectl``` ataupun dengan menyunting berkas ```/etc/hostname```. Namun, pada ubuntu versi 18.04 terdapat kendala yang mengakibatkan hostname yang sudah disesuaikan kembali pada konfigurasi awal. 

Untuk mengatasi kendala ini perlu dilakukan penyesuai pada berkas ``` /etc/cloud/cloud.cfg``` dengan langkah sebagai berikut:
1. buka terminal, jika ubuntu tidak memiliki GUI silahkan lewati langkah ini.
2. Pastikan memiliki hak aksess **root** atapun dapat menjalankan perintah ```sudo```.
3. jalankan perintah ```sudo nano /etc/cloud/cloud.cfg``` atau ```nano /etc/cloud/cloud.cfg``` jika login sebagai **root**. 
4. cari teks ```preserve_hostname``` dan ubah parameternya yang sebelumya ```false``` menjadi ```true``` lalu simpan.
5. ubah kembali hostname dengan menjalankan perintah ```sudo hostnamectl set-hostname nama_hostname``` atau ```hostnamectl set-hostname nama_hostname```.
6. Atau bisa juga dengan cara menyunting berkas ```/etc/hostname``` lalu simpan.

Untuk mengetahui hasilnya silahkan me-*restart* komputer ataupun server ubuntu.

sumber: [askubuntu.com](https://askubuntu.com/questions/1028633/host-name-reverts-to-old-name-after-reboot-in-18-04-lts)