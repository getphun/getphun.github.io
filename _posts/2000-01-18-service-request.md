---
layout: post
title: Service Request
categories: core
---

Service yang menyediakan informasi-informasi dan aksi yang berhubungan dengan 
request yang sedang berlangsung. Service ini bisa diakses dari kontroler dengan
perintah `$this->req->{method|prop}`.

Service ini menyediakan data dan method sebagai berikut:

1. [$method](#method)
1. [$uri](#uri)
1. [$url](#url)
1. [get](#get)
1. [getCookie](#getcookie)
1. [getFile](#getfile)
1. [getIP](#getip)
1. [getPost](#getpost)
1. [getQuery](#getquery)
1. [getServer](#getserver)
1. [isAjax](#isajax)

## $method

Properti yang menyimpan informasi request method yang sedang terjadi. Nilai ini
sama persis dengan yang ada di `$_SERVER['REQUEST_METHOD']`.

## $uri

Properti yang menyimpan informasi path URI yang sedang terjadi. Akan mengembalikan
path tanpa host, dan tanpa query string.

## $url

Properti yang menyimpan informasi URL yang sedang terjadi. Akan mengembalikan
full-url dengan host, dan tanpa query string.

## get

```php
<?php
/**
 * @param string $name Nama parameter
 * @param mixed $def Nilai default 
 * @return mixed nilai parameter, atau $def.
 */
$this->req->get($name[, $def=null]);
```

Mengambil nilai request parameter dari `$_GET`, `$_POST`, atau `$_FILES` secara
berurutan. Jika semuanya `null`, maka nilai dari argument `$def` yang akan
dikembalikan.

## getCookie

```php
<?php
/**
 * @param string $name Nama cookie
 * @param mixed $def Nilai default 
 * @return mixed nilai cookie, atau $def.
 */
$this->req->getCookie($name[, $def=null]);
```

Mengambil nilai cookie, atau sama dengan `$_COOKIE[$name]`. Jika cookie tidak
ditemukan, maka nilai dari argument `$def` yang akan dikembalikan.

## getFile

```php
<?php
/**
 * @param string $name Nama field file.
 * @return mixed object file atau null.
 */
$this->req->getFile($name);
```

Mengambil object file yang diupload user. Fungsi ini sama dengan perintah `$_FILES[$name]`.

## getIP

```php
<?php
/**
 * @return string user ip
 */
$this->req->getIP();
```

Fungsi yang mencoba menggembalikan client ip yang diambil dari header berikut:

1. HTTP_CLIENT_IP
1. HTTP_X_FORWARDED_FOR
1. HTTP_X_FORWARDED
1. HTTP_FORWARDED_FOR
1. HTTP_FORWARDED
1. REMOTE_ADDR

## getPost

```php
<?php
/**
 * @param string $name Nama field
 * @param mixed $def Nilai default 
 * @return mixed nilai post field, atau $def.
 */
$this->req->getPost($name[, $def=null]);
```

Mengambil nilai post, atau sama dengan `$_POST[$name]`. Jika post field tidak
ditemukan, maka nilai dari argument `$def` yang akan dikembalikan.

## getQuery

```php
<?php
/**
 * @param string $name Nama field
 * @param mixed $def Nilai default 
 * @return mixed nilai get query, atau $def.
 */
$this->req->getQuery($name[, $def=null]);
```

Mengambil nilai get query, atau sama dengan `$_GET[$name]`. Jika get query tidak
ditemukan, maka nilai dari argument `$def` yang akan dikembalikan.

## getServer

```php
<?php
/**
 * @param string $name Nama field
 * @param mixed $def Nilai default 
 * @return mixed nilai properti server, atau $def.
 */
$this->req->getServer($name[, $def=null]);
```

Mengambil nilai dari variable global `$_SERVER`, atau sama dengan `$_SERVER[$name]`.
Jika nilai tidak ditemukan, maka nilai dari argument `$def` yang akan dikembalikan.

## isAjax

```php
<?php
/**
 * @return boolean true jika menggunakan ajax, false jika bukan
 */
$this->req->isAjax();
```

Mengecek jika request yang sedang berlangsung berasal dari ajax.