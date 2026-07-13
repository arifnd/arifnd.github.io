---
title: 'Mengoptimalkan GitHub Actions: Testing FastAPI 86% Lebih Cepat'
date: 2026-07-12T10:00:00+07:00
draft: false
cover: 
  image: images/featured-image.png
tags:
  - Python
  - FastAPI
  - uv
  - CI
  - GitHub
showtoc: true
---

Salah satu keuntungan menggunakan _Continuous Integration_ (CI) adalah setiap perubahan kode dapat langsung divalidasi melalui serangkaian _automated tests_. Namun, seiring bertambahnya _unit_ dan _feature tests_, waktu eksekusi _pipeline_ juga akan meningkat.

Pada proyek FastAPI yang sedang penulis kembangkan, GitHub Actions membutuhkan sekitar 2 menit 6 detik untuk menjalankan 335 _automated tests_. Waktu tersebut memang tidak terlalu lama jika hanya dijalankan sekali, tetapi dalam aktivitas pengembangan, setiap _push_, _pull request_, atau _merge_, akumulasi waktu pengujian mulai terasa mengganggu.

Untuk memangkas waktu, penulis mencoba melakukan _profiling_ untuk mencari bagian mana yang benar-benar menjadi _bottleneck_. Hasilnya cukup menarik. Dengan beberapa optimasi sederhana namun tepat sasaran, waktu eksekusi berhasil turun menjadi sekitar 18 detik tanpa mengurangi jumlah _test_ maupun menurunkan kualitas pengujian.

Berikut proses optimasi yang penulis lakukan.

## Kondisi Awal

Lingkungan pengujian yang digunakan:

- GitHub Actions (ubuntu-latest)
- Python 3.12
- FastAPI
- SQLAlchemy 2.0
- SQLite
- Pytest
- Total 335 automated tests

Benchmark awal:

- Total waktu eksekusi: 2 menit 6 detik

## Langkah Optimasi

Langkah pertama yang penulis lakukan bukan langsung mengubah kode, tetapi mencari tahu terlebih dahulu bagian mana yang paling banyak menghabiskan waktu.

### 1. Mengganti `pip` dengan `uv`

_Bottleneck_ pertama ternyata berasal dari proses instalasi _dependency_.

Secara default, GitHub Actions akan membuat _virtual environment_ baru dan menginstal seluruh dependency setiap kali _workflow_ dijalankan. Walaupun _cache_ sudah digunakan, proses instalasi menggunakan `pip` masih membutuhkan waktu yang cukup signifikan.

Penulis mencoba beralih menggunakan `uv`, _package manager_ Python yang dikembangkan oleh Astral dan ditulis menggunakan Rust.

Workflow GitHub Actions menjadi seperti berikut.

```yaml
- name: Setup uv
  uses: astral-sh/setup-uv@v5
  with:
    enable-cache: true
    cache-dependency-glob: requirements.txt

- name: Install Python
  run: uv python install 3.12

- name: Create Virtual Environment
  run: uv venv

- name: Install Dependencies
  run: uv pip install -r requirements.txt pytest-xdist
```

Dengan memanfaatkan cache bawaan `uv`, proses instalasi dependency menjadi jauh lebih cepat. Package tidak perlu diunduh ulang pada setiap workflow sehingga startup pipeline berkurang hampir satu menit.

Untuk proyek Python yang cukup sering dijalankan di CI, menurut penulis perpindahan dari `pip` ke `uv` memberikan peningkatan performa yang cukup terasa.

### 2. Menjalankan Test Secara Paralel

Setelah waktu instalasi berkurang, _bottleneck_ berikutnya adalah proses eksekusi test itu sendiri.

Secara default, Pytest menjalankan seluruh test secara berurutan (_single process_). Padahal GitHub Actions menyediakan lebih dari satu CPU _core_ yang bisa dimanfaatkan.

Penulis menambahkan plugin `pytest-xdist` kemudian menjalankan test menggunakan:

```bash
uv run pytest tests/ -v --tb=short -n auto
```

Parameter `-n auto` akan mendeteksi jumlah CPU yang tersedia kemudian membagi seluruh _test_ ke beberapa _worker_.

Karena sebagian besar _test_ bersifat independen, waktu eksekusi langsung berkurang cukup signifikan tanpa perlu mengubah implementasi _test_.

### 3. Mengurangi Cost BCrypt Saat Testing

Langkah berikutnya adalah mencari _test_ yang paling lambat.

Pytest menyediakan fitur profiling sederhana melalui:

```bash
pytest --durations=5
```

Dari hasil _profiling_ tersebut penulis menemukan bahwa proses _hashing password_ menjadi salah satu penyumbang waktu terbesar.

Hal ini sebenarnya normal. `bcrypt` memang dirancang agar proses _hashing_ berjalan lambat demi meningkatkan keamanan terhadap serangan _brute force_.

Di lingkungan _production_ penulis menggunakan:

```python
rounds = 12
```

Namun untuk _automated testing_, tingkat keamanan tersebut sebenarnya kurang diperlukan karena tujuan pengujian hanyalah memastikan fungsi berjalan dengan benar.

Solusinya adalah menggunakan konfigurasi yang berbeda saat testing.

```python
import os
import bcrypt

IS_TESTING = os.getenv("SECRET_KEY") == "test-key"
BCRYPT_ROUNDS = 4 if IS_TESTING else 12

def get_password_hash(password: str) -> str:
    salt = bcrypt.gensalt(rounds=BCRYPT_ROUNDS)
    return bcrypt.hashpw(
        password.encode(),
        salt
    ).decode()
```

Dengan pendekatan ini, algoritma yang digunakan tetap `bcrypt` sehingga perilaku aplikasi tidak berubah. Yang berbeda hanyalah tingkat kompleksitas hashing ketika berada di lingkungan testing.

Waktu hashing turun dari sekitar 0,96 detik menjadi hanya beberapa milidetik.

Pada tahap ini penulis juga menghapus penggunaan `passlib` karena sudah mulai memunculkan beberapa _deprecation warning_ pada Python 3.12.

### 4. Mengoptimalkan Pembuatan Data Testing

_Bottleneck_ terakhir berasal dari _fixture database_.

Beberapa _test pagination_ sebelumnya membuat data menggunakan:

```python
db.add(...)
```

di dalam sebuah _loop_.

Cara tersebut todak salah, tetapi setiap pemanggilan ORM akan menambah _overhead_ transaksi database.

Penulis menggantinya dengan _bulk insert_ menggunakan SQLAlchemy 2.0.

```python
from sqlalchemy import insert

db.execute(
    insert(Device),
    [
        {
            "name": f"Device {i}",
            "status": "active"
        }
        for i in range(15)
    ]
)

db.commit()
```

Selain itu penulis juga mengevaluasi jumlah data _dummy_ yang dibuat.

Sebagai contoh, jika _pagination_ hanya membutuhkan 11 data untuk menguji perpindahan halaman, maka tidak ada alasan membuat ratusan _record_. _Fixture_ yang lebih kecil membuat test lebih cepat sekaligus lebih mudah dipahami.

## Hasil Akhir

Setelah seluruh optimasi diterapkan, hasil _benchmark_ menjadi sebagai berikut.

| Tahap | Optimasi                                                                         |                           Waktu | Total Peningkatan |
| ----- | -------------------------------------------------------------------------------- | ------------------------------: | ----------------: |
| 0     | Baseline                                                                         | **2 menit 6 detik<br>(126 Detik)** |                 - |
| 1     | Mengaktifkan `pip` cache + `pytest-xdist`                                        | **1 menit 16 detik<br>(76 Detik)** |         **39,7%** |
| 2     | Migrasi dari `pip` ke `uv`                                                       |                    **48 detik** |         **61,9%** |
| 3     | Mengganti Passlib dengan `bcrypt` dan menurunkan `rounds` menjadi 4 saat testing |                    **25 detik** |         **80,2%** |
| 4     | Bulk insert SQLAlchemy dan mengurangi jumlah data fixture                        |                    **18 detik** |         **85,7%** |


Tidak ada satu optimasi yang secara sendirian mampu memangkas waktu hingga lebih dari satu menit. Sebaliknya, peningkatan performa diperoleh dari menghilangkan _beberapa_ bottleneck kecil yang jika digabungkan memberikan dampak yang cukup besar.

## Kesimpulan

Dari pengalaman ini penulis belajar bahwa optimasi CI tidak selalu diselesaikan dengan membeli _runner_ yang lebih cepat atau mengurangi jumlah _test_. Bisa jadi masalahnya justru berada pada hal-hal kecil seperti proses instalasi _dependency_, konfigurasi _library_, strategi pembuatan fixture_, atau pemanfaatan CPU yang belum optimal.

Jika _pipeline_ GitHub Actions di proyek mulai terasa lambat, penulis menyarankan untuk tidak langsung melakukan optimasi secara acak. Mulailah dengan melakukan _profiling_, identifikasi _bottleneck_ terbesar, kemudian optimalkan satu per satu. Dengan pendekatan tersebut, setiap perubahan yang dilakukan memiliki alasan yang jelas dan hasilnya pun dapat diukur.