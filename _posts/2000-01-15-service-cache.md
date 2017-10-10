---
layout: post
title: Service Cache
categories: core
---

Adalah service yang disediakan oleh module core yang bertugas menangani semua
urusan yang berhubungan dengan cache. Service ini didaftarkan dengan nama `cache`
sehingga bisa diakses dari kontroler dengan perintah `$this->cache->{metohd}`.

Untuk sekarang, cache yang didukung baru cache dengan basis data file. Sangat
besar kemungkinan service ini akan mendukung basis data lainnya seperti redis,
mongodb, dan lain-lain.

Untuk basis data file, semua data cache disimpan di folder `./etc/cache`.

Service `cache` memiliki beberapa method yaitu:

1. [get](#get)
1. [getOutputName](#getoutputname)
1. [remove](#remove)
1. [removeOutput](#removeoutput)
1. [save](#save)
1. [saveOutput](#saveoutput)
1. [total](#total)
1. [truncate](#truncate)

## get

```php
<?php
/**
 * @param string $name Nama cache
 * @return mixed data cache atau null.
 */
$this->cache->get($name)
```

Fungsi untuk mengambil nilai cache. Jika cache tidak ada, maka nilai `null` akan
dikembalikan.

## getOutputName

```php
<?php
/**
 * @return string nama cache output.
 */
$this->cache->getOutputName()
```

Fungsi yang akan menggenerasi nama cache output untuk request yang sedang berlangsung.
Fungsi ini akan memperhitungkan semua rules output cache yang ada di konfigurasi
aplikasi. Nilai yang dikembalikan adalah string nama cache.

## remove

```php
<?php
/**
 * @param string $name Nama cache
 * @return boolean true jika berhasil, false jika gagal.
 */
$this->cache->remove($name)
```

Fungsi untuk menghapus cache dari basis data. Fungsi ini akan mengembalikan nilai
`true` jika berhasil, atau `false` jika gagal.

## removeOutput

```php
<?php
/**
 * @param string $url
 */
$this->cache->removeOutput($url)
```

Fungsi untuk menghapus cache output. Tidak ada nilai yang akan dikembalikan oleh
fungsi ini.

## save

```php
<?php
/**
 * @param string $name Nama cache.
 * @param mixed $content Data yang ingin disimpan.
 * @param $expiration Total lamanya cache disimpan dalam satuan detik.
 */
$this->cache->save($name, $content, $expiration)
```

Fungsi untuk menyimpan suatu data kedalam cache. Fungsi ini akan selalu mengembalikan
nilai `true`.

## saveOutput

```php
<?php
/**
 * @param $res Object response yang memiliki properti header dan content.
 * @param $expiration Total lamanya cache disimpan dalam satuan detik.
 */
$this->cache->saveOutput($res, $expiration)
```

Fungsi untuk membuatkan cache output dari request yang sedang berlangsung. Fungsi
ini akan menggunakan fungsi `getOutputName` untuk menggenerasi nama cache. Fungsi
ini akan selalu mengembalikan nilai `true`.

## total

```php
<?php
/**
 * @return integer total cache
 */
$this->cache->total()
```

Mengambil jumlah cache yang tersimpan di basis data. Fungsi ini mengembalikan nilai
integer jumlah cache.

## truncate

```php
<?php
/**
 */
$this->cache->truncate()
```

Menghapus semua data cache. Fungsi ini tidak memiliki parameter, dan juga tidak
memiliki nilai pengembalian.