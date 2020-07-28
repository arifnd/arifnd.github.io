# Riwayat



<!-- **2020-07-13**

---
- 
-  -->

<!-- **2020-07-18** -->

**2020-07-17**

---
- [getterms.io](https://getterms.io/): alternative generator untuk Privacy Policy dan TOS.
- Menampilkan informasi arsitektur di [Alpine Linux](https://alpinelinux.org/) dengan perintah `dpkg --print-architecture|awk -F'-' '{print $NF}'`, [link](https://github.com/tianon/gosu/issues/24).
- Menampilkan informasi arsitektur di Debian/Ubuntu dengan perintah `dpkg --print-architecture`.
- Tutorial membuat image doker minimalis untuk aplikasi dengan bahasa pemrograman golang, [link](https://medium.com/@chemidy/create-the-smallest-and-secured-golang-docker-image-based-on-scratch-4752223b7324).
- `ldd` perintah untuk mengetahui *shared library* dari sebuah aplikasi linux.

**2020-07-16**

---
- Mengenal Trusted Web Activities, [link](https://codelabs.developers.google.com/codelabs/getting-started-with-twas/index.html#0).
- Solusi agar perangkat Android terdeteksi di linux saat *debug* program, [link](https://stackoverflow.com/questions/43771918/how-do-i-set-up-udev-rules-for-debugging-a-physical-android-device-with-android)
- [hookbin](https://hookbin.com/): Online Tool untuk inspeksi HTTP Resquest.

**2020-07-14**

---
- Tambahkan repositori manual di `/etc/apt/sources.list` untuk mengatasi gagal *install* docker di RPi OS Buster.

  `deb [arch=armhf] https://download.docker.com/linux/debian buster stable`


**2020-07-13**

---
![RPi4](/images/rpi-4-aluminium-case.jpg)
- Beli Raspberry Pi 4 2GB.
- *Install* Raspberry Pi OS (32-bit) Lite.
- Suhu RPi 4 47-49'C meskipun menggunakan pendingin aluminium dan dalam kondisi *fresh install*.
- Gagal *install* docker dari tutorial [*Install Docker Engine on Debian*](https://docs.docker.com/engine/install/debian/)
