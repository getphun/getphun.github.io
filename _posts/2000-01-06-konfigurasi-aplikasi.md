---
layout: post
title: Konfigurasi Aplikasi
categories: core
---

Masing-masing module memiliki file konfigurasi sendiri yang disimpan di file
`config.php` yang ada di folder module. Ada beberapa konfigurasi yang tidak bisa
dituliskan di module karena konfigurasi tersebut terikat dengan aplikasi. Untuk
itulah aplikasi memiliki konfigurasi sendiri.

Perlu diketahui bahwa konfigurasi aplikasi adalah bagian yang paling akhir
digabungkan dengan kumpulan konfigurasi module. Sehingga kesamaan nama properti
pada konfigurasi aplikasi akan menindih konfigurasi yang sama di konfigurasi
module. Dengan begitu, developer aplikasi bisa mendindih nilai konfigurasi module
dari konfigurasi aplikasi jika dibutuhkan.

File konfigurasi aplikasi disimpan di `./etc/config.php`. Sebelum menggunakan
file ini, system mencari file konfigurasi berdasarkan nilai `env` terlebih dahulu.
Jika nilai `env` sekarang adalah `production`, maka system mencari file 
`./etc/config.production.php` terlebih dahulu. Jika file tersebut tidak ada,
system kembali ke file konfigurasi standar, yaitu `./etc/config.php`.

Nilai konfigurasi bisa diakses dari aplikasi dengan perintah `$this->config->{property}`.

Isi nilai konfigurasi aplikasi kurang lebih seperti di bawah:

```php
<?php
/**
 * Application level config
 * @package core
 * @version 0.0.1
 * @upgrade false
 */

return [
    'name' => 'Phun',
    'version' => '0.0.1',
    'host' => 'cms.phu',
    'secure' => false,
    'timezone' => 'Asia/Jakarta',
    'install' => '2017-05-31 01:05:00',
    
    '_gates' => [
        'site' => [
            'host' => 'HOST',
            'path' => '/'
        ]
    ],
    '_routes' => [
        'site' => [
            'rule' => '/',
            'handler' => 'Site::index'
        ]
    ],
    
    'query_cache' => [
        'page', 'library'
    ],
    
    'bakso' => 'Jakarta'
];
```

Keterangan masing-masing properti adalah sebagai berikut:

## name

Adalah nama aplikasi. Tidak ada spesifikasi khusus untuk nilai ini.

## version

Versi aplikasi yang sekarang. Tidak ada spesifikasi khusus untuk nilai ini. Tapi
untuk menjaga kesamaan antar development, sebaiknya gunakan bentuk seperti di atas.
Yaitu pemisahan menjadi tiga bagian, dan dimulai dari versi `0.0.1`.

## host

Adalah hostname aplikasi. Tanpa `http(s)` dan tanpa akhiran `/`. Nilai ini yang
akan digunakan pada properti `_gates` jika nilai properti `host` adalah `HOST`.
Jika aplikasi menggunakan `www`, maka pastikan nilai properti ini juga dengan
www. Properti ini juga yang akan digunakan ketika router akan menggenerasi
absolute url.

## secure

**Untuk sementara nilai ini belum digunakan**

Adalah properti yang menentukan
apakah absolute URL akan menggunakan `https` atau `http`.

## timezone

Adalah timezone yang akan digunakan aplikasi dalam proses tanggal.

## install

Adalah konfigurasi yang berisi tanggal pertama aplikasi di deploy ke server.
Nilai ini mungkin dibutuhkan beberapa module untuk tujuan tertentu. Isikan
dengan format `Y-m-d H:i:s`.

## _gates

Adalah daftar gates aplikasi. Masing-masing gate harus memiliki properti `path`
dan optional properti `host`. Jika properti `host` tidak diisi, maka semua request
host dianggap cocok. Lebih lanjut tentang gate bisa dilihat di bagian gates.

## _routes

Adalah daftar tambahan routes selain dari yang sudah didaftarkan di module. Pada
sebagian besar aplikasi, nilai `_routes` selalu kosong. Hal ini karena pada umumnya
routes ke suatu module didaftarkan di module itu sendiri.

Nilai konfigurasi ini adalah array dimana key array adalah nama gate, dan nilai
nya adalah daftar routes.

## query_cache

Adalah daftar request query string ( `$_GET` ) yang ikut diperhitungkan pada seleksi
output cache.

Jika nilai properti ini adalah `['page']`, maka kedua request ini akan disimpan
di output cache yang berbeda:

```
http://example.com/post
http://example.com/post?page=2
```

Sementara kedua request di bawah akan menggunakan output cache yang sama:

```
http://example.com/post
http://example.com/post?utm_source=phun
```

## tambahan

Tambahan konfigurasi selain yang dijelaskan di atas tetap disimpan dan tetap bisa
digunakan dari aplikasi atau oleh module.