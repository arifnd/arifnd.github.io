---
title: 'Trivy: Tools Open Source untuk Memindai Kerentanan Docker Image'
date: 2025-12-11T10:30:00+07:00
draft: false
images: []
tags:
  - trivy
  - docker
  - DevSecOps
  - security
  - vulnerability-scanner
showtoc: true
---

Saat sedang mencari solusi untuk melakukan pengecekan kerentanan pada sebuah Docker *image*, penulis menemukan tools *open source* bernama [**Trivy**](https://trivy.dev/). Awalnya penulis mencoba [Docker Scout](https://docs.docker.com/scout/) sebagai pilihan utama karena merupakan produk resmi [Docker](https://www.docker.com/). Namun, sejumlah fitur penting ternyata berbayar sehingga tidak dapat digunakan secara bebas.

Usaha mencari alternatif pun dimulai. Seperti menemukan peralatan langka di kotak perkakas, penulis akhirnya bertemu dengan Trivy, sebuah tools yang ternyata sederhana tetapi kaya fitur. Kesan pertama yang muncul: *ringan, cepat, dan hasilnya jelas*.

Tanpa konfigurasi rumit, hanya satu perintah sederhana sudah cukup:

```bash
trivy image <nama-image-anda>
```

Dalam sekejap, Trivy menampilkan ringkasan CVE, tingkat keparahan, hingga paket-paket rentan yang ditemukan di dalam Docker *image* tersebut. Pengalaman ini membuat penulis merasa menemukan alat yang selama ini dicari-cari: *gratis, open source, dan sangat praktis digunakan*.


## Kelebihan

Beberapa hal yang membuat Trivy terasa spesial:

- Gratis dan open source tanpa batasan fitur

- Instalasi mudah di berbagai sistem operasi

- Perintah penggunaan sangat sederhana

- Hasil pemindaian mudah dibaca dan dianalisis

- Nyaman digunakan dalam *workflow developer* maupun *pipeline* DevOps

## Cara Instalasi

Ada berbagai cara untuk menginstal Trivy.

### Container image (Resmi)

Contoh instalasi dari Docker Hub:

```bash
docker run -v /var/run/docker.sock:/var/run/docker.sock -v $HOME/Library/Caches:/root/.cache/ aquasec/trivy:0.68.1 image python:3.4-alpine
```

### GitHub Release (Resmi)

1. Unduh file untuk sistem operasi/arsitektur Anda dari aset [GitHub Release](https://github.com/aquasecurity/trivy/releases/tag/v0.68.1).

2. Ekstrak arsip yang diunduh.

```bash
tar -xzf ./trivy.tar.gz
```

3. Pastikan bit eksekusi biner diaktifkan.

```bash
chmod +x ./trivy
```

### Script Instalasi (Resmi)

Untuk kemudahan, Anda dapat menggunakan skrip instalasi untuk mengunduh dan menginstal Trivy dari GitHub Release.

```bash
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sudo sh -s -- -b /usr/local/bin v0.68.1
```

### RHEL/CentOS (Resmi)

#### Repository

Tambahkan pengaturan *repository* di `/etc/yum.repos.d`.

```bash
cat << EOF | sudo tee -a /etc/yum.repos.d/trivy.repo
[trivy]
name=Trivy repository
baseurl=https://aquasecurity.github.io/trivy-repo/rpm/releases/\$basearch/
gpgcheck=1
enabled=1
gpgkey=https://aquasecurity.github.io/trivy-repo/rpm/public.key
EOF
sudo yum -y update
sudo yum -y install trivy
```

#### RPM

Untuk memasang langsung dari paket `.rpm` jalankan perintah berikut:

```bash
rpm -ivh https://github.com/aquasecurity/trivy/releases/download/v0.68.1/trivy_0.68.1_Linux-64bit.rpm
```

### Debian/Ubuntu (Resmi)

#### Repository

Tambahkan pengaturan *repository* di `/etc/apt/sources.list.d`.

```bash
sudo apt-get install wget gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

#### DEB

Untuk memasang langsung dari paket `.deb` jalankan perintah berikut:

```bash
wget https://github.com/aquasecurity/trivy/releases/download/v0.68.1/trivy_0.68.1_Linux-64bit.deb
sudo dpkg -i trivy_0.68.1_Linux-64bit.deb
```

### Homebrew (Resmi)

Homebrew untuk macOS dan Linux.

```bash
brew install trivy
```

### Windows (Resmi)

1. Unduh file trivy_x.xx.x_windows-64bit.zip dari [halaman rilis](https://github.com/aquasecurity/trivy/releases/).

2. Ekstrak file dan salin ke folder mana pun.

Untuk cara instalasi di Arch Linux, OpenSUSE, Nix/NixOS, FreeBSD dapat melihat pada tautan [Installing Trivy](https://trivy.dev/docs/latest/getting-started/installation/).


## Penggunaan

Untuk melakukan pemindaian sederhana, perintah berikut sudah cukup:

```bash
trivy image <nama-image-anda>
```

Trivy akan menampilkan:

- daftar CVE yang ditemukan

- tingkat keparahan (*severity*)

- paket rentan yang teridentifikasi

- rekomendasi versi yang lebih aman

Informasi tersebut sangat berguna untuk menentukan langkah perbaikan sebelum *container* digunakan dalam lingkungan produksi.

## Kesimpulan

**Trivy** menawarkan pengalaman pemindaian kerentanan yang mudah, cepat, dan informatif. Dengan sifatnya yang gratis dan *open source*, *tools* ini menjadi pilihan ideal bagi developer, DevOps, maupun praktisi keamanan yang ingin memastikan keamanan Docker *image* maupun komponen lain dalam aplikasi. Instalasi yang sederhana dan hasil analisis yang jelas membuat **Trivy** layak menjadi bagian dari alur kerja keamanan modern.
