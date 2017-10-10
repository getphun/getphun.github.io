---
layout: post
title: Kontroler
categories: core
---

Semua request dari client akan diteruskan ke kontroler untuk di proses berdasarkan
route yang ada. Pada dasarnya, masing-masing kontroler bisa berdiri sendiri dan
menyelesaikan request dari user sendiri. Tapi, pada umumnya, semua kontroler
memiliki kesamaan beberapa method dan kebutuhan akan service injector. Untuk itulah
framework Phun menyediakan satu library yang bisa di-**extends** oleh kontroler
untuk mendapatkan preset method dan service injector aplikasi.

Library ini adalah class `Controller` yang dibuat di `./modules/core/library/Controller.php`.

Bentuk di bawah adalah bentuk umum cara meng-extends class Controller dari kontroler
module:

```php
<?php

namespace MyModule\Controller;

class MyController extends \Controller
{
    ...
}
```

Begitu kontroler module terextends dari class `Controller`, semua service bisa
diakses dengan perintah `$this->{service}`. Class Controller memiliki beberapa
method untuk membantu kontroler menyelesaikan tugas mereka. Method-method
tersebut adalah:

1. [ajax](#ajax)
1. [redirect](#redirect)
1. [redirectUrl](#redirecturl)
1. [respond](#respond)
1. [show404](#show404)

## ajax

```php
<?php
/**
 * @param mixed $data Data yang dikirim ke client
 */
$this->ajax($data)
```

Method untuk mengirim data dalam format json ke client. Method ini juga akan mengeset
header `Content-Type` response menjadi `application/json`.

## redirect

```php
<?php
/**
 * @param string $url Target redirect
 * @param integer $code HTTP Header status code. Default 302
 */
$this->redirect($url[, $code=302])
```

Fungsi untuk me-redirect request ke URL lain.

## redirectUrl

```php
<?php
/**
 * @param string $name Nama router
 * @param array $args Router arguments. Default []
 * @param array $query Tambahan query string. Default []
 * @param integer $code HTTP Header status code. Default 302
 */
$this->redirectUrl($name[, $args=[][, $query=[][, $code=302]]])
```

Fungsi untuk me-redirect request ke router lain berdasarkan nama router.

## respond

```php
<?php
/**
 * @param string $view Path theme file di folder theme gate.
 * @param array $params Data yang dikirim ke view. Default [].
 * @param integer $cache Total detik request di cache. Default null.
 */
$this->respond($view[, $params=[][, $cache]])
```

Fungsi untuk menggenerasi view. Nilai parameter $view adalah relative path ke 
theme file yang berlokasi di theme folder ( `./theme/[gate]` ).

## show404

```php
<?php
$this->show404();
```

Fungsi global untuk memanggil router yang menangani error 404.