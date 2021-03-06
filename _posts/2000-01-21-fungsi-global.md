---
layout: post
title: Fungsi Global
categories: core
---

Module core membawa beberapa fungsi global yang terload setiap kali aplikasi 
dijalankan. Di bawah ini adalah fungsi-fungsi tersebut:

1. [alt](#alt)
1. [autoload_class_exists](#autoload_class_exists)
1. [calculate_pagination](#calculate_pagination)
1. [deb](#deb)
1. [group_by_prop](#group_by_prop)
1. [group_per_column](#group_per_column)
1. [hs](#hs)
1. [is_dev](#is_dev)
1. [is_indexed_array](#is_indexed_array)
1. [module_exists](#module_exists)
1. [object_replace](#object_replace)
1. [prop_as_key](#prop_as_key)
1. [strtoslug](#strtoslug)

## alt

```php
<?php
/**
 * @param mixed $var Variabel yang akan dites
 * @return mixed Nilai yang bukan falsy
 */
alt($arg0, $arg1[, $arg2])
```

Fungsi untuk mendapatkan nilai yang bukan *falsy* dari deretan argument, dan 
mengembalikan nilai paling awal yang bukan *falsy*.

## autoload_class_exists

```php
<?php
/**
 * @param string $name Nama class untuk dicek
 */
autoload_class_exists($name)
```

Fungsi untuk mencari jika suatu class sudah didaftarkan di autoload class. Fungsi
ini tidak meload file class, pun tidak mencari tahu jika file target ada atau tidak.

## calculate_pagination

```php
<?php
/**
 *
 */
calculate_pagination()
```

## deb

```php
<?php
/**
 * @param mixed $arg Daftar argument yang akan di debug.
 */
deb([$arg0[, $arg1,...]])
```

Fungsi untuk mendebug variable dan mengakhiri eksekusi aplikasi. Fungsi `deb` cocok
digunakan sebagai pengganti `var_dump` untuk menghasilkan informasi yang lebih mudah
dibaca.

## group_by_prop

```php
<?php
/**
 * @param array $arr Array yang akan diproses
 * @param string $prop Properti yang dijadikan group
 * @return array kelompok array
 */
group_by_prop($arr, $prop)
```

Fungsi yang bertugas mengelompokkan anggota array ke dalam propertinya. Fungsi ini
akan mengembalikan array dimana key nya adalah properti pengelompokan, dan nilai
nya adalah array anggota kelompok yang tergabung ke kelopong tersebut.

## group_per_column

```php
<?php
/**
 * @param array $list Array sumber data
 * @param integer $column Total kolom/item per group
 * @return array multi-level array
 */
group_per_column($list[, $column=3]);
```

Fungsi yang mengubah list array menjadi array dengan pengelompokan, dimana
masing-masing kelompok terdiri dari sebanyak `$column` item.

## hs

```php
<?php
/**
 * @param string $str String yang akan diencode
 * @return string encoded $str
 */
hs($str)
```

Fungsi *short-hand* untuk `htmlspecialchars($str, ENT_QUOTES)`.

## is_dev

```php
<?php
/**
 * @return boolean true jika development
 */
is_dev()
```

Fungsi untuk mengetahui jika aplikasi berjalan di environment `development`.

## is_indexed_array

```php
<?php
/**
 * @param array $arr Array yang akan dites
 * @return boolean true jika indexed array
 */
is_indexed_array($arr)
```

Fungsi untuk mengecek jika suat array adalah indexed array atau bukan.

## module_exists

```php
<?php
/**
 * @param string $name Nama module
 * @return boolean true jika ada
 */
module_exists($name)
```

Fungsi untuk mengecek jika suatu module terinstal di aplikasi. Fungsi ini tidak
hanya mengecek jika folder module ada, tanpa melakukan pengecekan lebih lanjut.

## object_replace

```php
<?php
/**
 * @param object $origin Object lama
 * @param object $new Object sumber data baru
 * @return gabungan object $origin dengan $new
 */
object_replace($origin, $new)
```

Menggabungkan dua object menjadi satu, dengan ketentuan jika suatu properti ada
dikedua object, maka nilai dari properti object `$new` akan digunakan. Tugas 
fungsi ini hampir sama dengan `array_replace` kecuali target yang diproses adalah
object.

## prop_as_key

```php
<?php
/**
 * @param array $arr Sumber data array
 * @param string $prop Properti array yang akan dijadikan key.
 * @return key-value array
 */
prop_as_key($arr, $prop)
```

Fungsi yang menjadikan properti array menjadi array key, dan nilainya array itu
sendiri. Salah satu penggunaan fungsi ini adalah untuk menjadikan id row table
menjadi array key, dan nilainya row table itu sendiri. Jika dibutuhkan mengubah
properti array menjadi array key, dan salam satu properti lainnya menjadi nilai
array, maka gunakan fungsi `array_column`.

## strtoslug

```php
<?php
/**
 * @param string $str String yang akan di proses
 * @return string Versi slug dari $str
 */
strtoslug($str)
```

Fungsi untuk mengubah suatu string menjadi berbentuk slug, yaitu string yang hanya
terdiri dari karakter `a-z`, `0-9`, dan `-`. Semua karakter selain kararakter
yang diterima akan diubah menjadi karakter `-`, dan karakter `-` yang beradar di 
awal string atau di akhir string akan dihapus. Karakter `-` yang berdampingan
juga akan diubah menjadi satu saja.