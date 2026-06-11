# Tugas Praktikum 03 - MSIM4401

## Aplikasi Crypto Market

### Identitas Mahasiswa

**Nama:** Andre Bintang Pramastyo
**NIM:** 050600902
**UPBJJ:** UT Surabaya

---

## Deskripsi Tugas

Proyek ini dibuat untuk memenuhi Tugas Praktikum 03 mata kuliah **MSIM4401**.

Aplikasi menampilkan data cryptocurrency yang diambil secara online menggunakan **Coinlore API** dengan endpoint:

```text
https://api.coinlore.net/api/tickers/
```

Data yang ditampilkan sesuai dengan ketentuan soal, yaitu:

* Rank
* Name
* Symbol
* Price USD

---

## Screenshot Aplikasi

### Tampilan Utama

> Tambahkan screenshot hasil aplikasi pada folder `Asset/`

![Crypto Market](Asset/screenshot.png)

---

## Fitur Aplikasi

* Mengambil data cryptocurrency dari Coinlore API
* Menampilkan daftar cryptocurrency secara real-time
* Menampilkan informasi:

  * Rank
  * Nama Coin
  * Simbol Coin
  * Harga dalam USD
* Fitur Refresh Data
* Tampilan mobile menggunakan Ionic Vue

---

## Teknologi yang Digunakan

* Ionic Framework
* Vue 3
* TypeScript
* Vite
* Coinlore API

---

## Struktur Proyek

```text
src/
├── App.vue
├── main.ts
├── router/
├── theme/
└── views/
    └── HomePage.vue

Asset/
└── screenshot.png
```

---

## Penjelasan File Utama

### App.vue

Berfungsi sebagai komponen utama aplikasi yang memuat halaman dan navigasi aplikasi.

### HomePage.vue

Berisi:

* Pemanggilan API Coinlore
* Pengolahan data cryptocurrency
* Tampilan daftar cryptocurrency
* Fungsi refresh data

### main.ts

Berfungsi untuk melakukan inisialisasi aplikasi Vue dan Ionic.

### router/

Mengatur navigasi antar halaman aplikasi.

### theme/

Berisi pengaturan tema dan styling aplikasi.

---

## Cara Menjalankan Program

### Install Dependency

```bash
npm install
```

### Menjalankan Aplikasi

```bash
ionic serve
```

### Build Production

```bash
ionic build
```

---

## API yang Digunakan

### Endpoint

```text
https://api.coinlore.net/api/tickers/
```

### Data yang Ditampilkan

| Field     | Keterangan                     |
| --------- | ------------------------------ |
| rank      | Peringkat cryptocurrency       |
| name      | Nama cryptocurrency            |
| symbol    | Simbol cryptocurrency          |
| price_usd | Harga cryptocurrency dalam USD |

---

## Hasil Implementasi

Aplikasi berhasil menampilkan daftar cryptocurrency dari Coinlore API secara dinamis sesuai ketentuan tugas dengan menampilkan atribut:

* Rank
* Name
* Symbol
* Price USD

serta menyediakan fitur refresh untuk memperbarui data terbaru dari server.

---

## Author

**Andre Bintang Pramastyo**
NIM 050600902
UPBJJ UT Surabaya
