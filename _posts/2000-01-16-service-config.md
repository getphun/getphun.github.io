---
layout: post
title: Service Config
categories: core
---

Service config adalah service yang disediakan oleh module core untuk mempermudah
developer mengakses nilai config dari kontroler. Service ini bisa diakses dari
kontroler dengan perintah `$this->config->{properti}`.

Sebagai contoh, untuk mendapatkan nilai versi aplikasi yang ada di konfigurasi
aplikasi, bisa dengan menggunakan perintah `$this->config->version`.

Nilai yang diambil dari service ini adalah semua nilai konfigurasi gabungan antar
masing-masing module dengan konfigurasi aplikasi.

Sampai saat ini, service config hanya memiliki satu method, yaitu `set`:

## set

```php
<?php
/**
 * @param string $key... Nama konfigurasi.
 * @param mixed $value Nilai konfigurasi.
 */
$this->config->set($key0[, $key1...], $value);
```

Adalah method untuk mengubah nilai konfigurasi secara temporary. Perubahan nilai
tersebut hanya bisa digunakan dalam satu request, dan tidak disimpan ke konfigurasi
aplikasi.

Contoh penggunaan:

```php
<?php

// mengubah nilai konfigurasi host
$this->config->set('host', 'example.com');

// mengubah nilai host untuk gate site.
$this->config->set('_gates', 'site', 'host', 'example.com');
```