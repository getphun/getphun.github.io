---
layout: post
title: Service Response
categories: core
---

Adalah service yang menyediakan informasi dan aksi-aksi tentang response yang sedang
berlangsung. Service ini bisa diakses dari kontroler dengan perintah
`$this->res->{prop|method}`.

Service ini menyediakan data dan method sebagai berikut:

1. [addContent](#addcontent)
1. [addCookie](#addcookie)
1. [addHeader](#addheader)
1. [cache](#cache)
1. [getHeader](#getheader)
1. [redirect](#redirect)
1. [render](#render)
1. [send](#send)
1. [setStatus](#setstatus)

## addContent

```php
<?php
/**
 * @param string $text Tambahan teks pada output
 */
$this->res->addContent($text);
```

Fungsi untuk menambahkan teks konten ke dalam konten output. Fungsi ini tidak 
menindih konten yang sudah ada, hanya akan menambahkan diakhir konten.

## addCookie

```php
<?php
/**
 * @param string $name Nama cookie 
 * @param string $value Nilai cookie
 * @param integer $expiration Lamanya cookie disimpan, dalam satuan detik
 */
$this->res->addCookie($name, $value, $expiration)
```

Tambahkan cookie ke response. Menset cookie pada response tidak akan tercache
pada cache output. Fungsi ini hanya berlaku bagi request yang sedang berlangsung.

## addHeader

```php
<?php
/**
 * @param string $name Nama header
 * @param string $content Nilai header
 */
$this->res->addHeader($name, $content)
```

Fungsi untuk mendaftarkan tambahan header untuk request yang sedang berlangsung.
Jika request dicache, maka nilai-nilai pada header juga akan digunakan pada
response dari cache selanjutnya.

## cache

```php
<?php
/**
 * @param integer $time Total output akan dicache dalam satuan detik
 */
$this->res->cache($time)
```

Jika fungsi ini dipanggil sebelum memanggil perintah `send`, maka view, dan headers
yang sudah diset akan dicache dan akan digunakan oleh system untuk meresponse 
request selanjutnya tanpa melalui proses kontroler lagi.

## getHeader

```php
<?php
/**
 * @param string $name Nama header
 */
$this->res->getHeader($name)
```

Mengambil header yang sudah diset untuk request ini. Nilai yang dikembalikan bukan
nilai header yang dikirim client, melainkan header yang akan diset dan dikirim ke 
client.

## redirect

```php
<?php
/**
 * @param string $url Target URL kemana request akan diredirect
 * @param integer $code Redirect http kode. Default 302.
 */
$this->res->redirect($url[, $code=302])
```

Fungsi untuk me-redirect request ke url lain.

## render

```php
<?php
/**
 * @param string $gate Gate yang akan digunakan untuk menentukan folder theme
 * @param string $view Path ke file theme relative ke folder gate.
 * @param array $params Parameter variable yang dikirim ke view.
 */
$this->res->render($gate, $view, $params)
```

Fungsi untuk menggenerasi view dari file theme yang berada di folder gate theme.
Hasil render view akan langsung ditambahkan ke response output.

## send

```php
<?php
/**
 * @param string $content Response konten
 * @param array $headers Tambahan/Perubahan header
 */
$this->res->send([$content=null[, $headers=[]]])
```

Mengirim konten yang sudah terbentuk, dan keluar dari aplikasi. Semua perintah
setelah perintah ini tidak lagi tereksekusi.

Jika parameter `$content` diset, maka nilai konten yang sudah diset sebelumnya
akan ditindih. Jika parameter `$headers` diisi, maka nilai header dengan key yang
sama akan ditindih, sementara header lainnya tetap digunakan.

## setStatus

```php
<?php
/**
 * @param integer $status Response status code.
 */
$this->res->setStatus($status)
```

Mengeset response status code. Fungsi ini hanya berlaku untuk request yang sedang
berlangsung saja. Jika request menyimpan cache output, nilai http status code tidak
akan mengikuti nilai yang diset di sini.