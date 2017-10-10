---
layout: post
title: Service Router
categories: core
---

Adalah service yang menyediakan data dan method yang berhubungan dengan router.
Service ini bisa diakses dari kontroler dengan perintah `$this->router->{prop|method}`.

Service ini menyediakan data dan method sebagai berikut:

1. [$gate](#gate)
1. [$route](#route)
1. [exists](#exists)
1. [to](#to)

## $gate

Adalah properti yang menyimpan informasi gate yang sedang aktif untuk request ini.

## $route

Adalah properti yang menyimpan informasi route yang sedang aktif untuk request ini.

## exists

```php
<?php
/**
 * @param string $name Nama router
 * @return boolean true jika ada, false jika tidak ada.
 */
$this->router->exists($name)
```

Adalah fungsi untuk mengecek jika suatu router dengan nama `$name` sudah terdaftar.

## to

```php
<?php
/**
 * @param string $name Nama target router
 * @param array $args Argument untuk pengganti router placeholder 
 * @param array $query Tambahan get query string pada hasil url
 * @return string url hasil generasi route
 */
$this->router->to($name[, $args=[][, $query=[]]])
```

Adalah fungsi untuk menggenerasi url berdasarkan nama router. Jika gate
route tersebut memiliki properti host, maka fungsi ini akan menghasilkan
absolute url, tapi jika tidak, maka akan menghasilkan relative url.