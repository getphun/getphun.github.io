---
layout: post
title: Gate Aplikasi
categories: core
---

Gate adalah *gerbang* darimana user masuk ke aplikasi. Pada umunya, satu aplikasi
terdiri dari minimal dua gate, yaitu gate site, dan gate admin. Di kondisi tertentu,
aplikasi mungkin juga memiliki gate site mobile, api, dashboard, partner, dan
lain-lain.

Ketika kontroler menggenerasi template, template yang akan digunakan oleh view
adalah template file yang ada di foler `theme/[gate]`. Sehingga bisa dijadikan
pegangan bahwa template suatu gate adalah folder gate itu sendiri di bawah
folder `./theme`.

Masing-masing gate biasanya dibedakan berdasarkan hostname, dan atau path uri.
Contohnya, gate admin menggunakan prefix path `/admin` sehingga semua request
yang awalan uri nya adalah `./admin` akan menggunakan gate `admin`, dan akan
menggunakan template dari folder `./theme/admin`. Contoh kedua adalah gate
`microsite` yang ditentukan dari hostname nya dengan rule `:slug.HOST` sehingga
semua request ke `[apasaja].example.com` akan dianggap gate `microsite`, dan
akan menggunakan template dari folder `./theme/microsite`.

Semua gate ditentukan di file konfigurasi aplikasi ( `./etc/config.php` ), dan
sebaiknya hanya ditentukan di level konfigurasi aplikasi. Sangat tidak disarankan
menentukan konfigurasi gate di konfigurasi module.

Masing-masing gate akan dites dengan urutan yang paling pertama didaftarkan di 
file konfigurasi. Jika suatu tes cocok dengan suatu request, maka gate seterusnya
tidak lagi di proses.

Contoh di bawah adalah cara-cara mendaftarkan gate di file `./etc/config.php`:

```php
<?php

return [
    'host' => 'example.com',
    ...
    '_gates' => [
        'video' => [
            'host' => 'video.HOST',
            'path' => '/'
        ],
        'audio' => [
            'host' => 'audio.website.com',
            'path' => '/'
        ],
        'micro' => [
            'host' => ':slug.HOST',
            'path' => '/'
        ],
        'admin' => [
            'path' => '/admin'
        ],
        'site' => [
            'host' => 'HOST',
            'path' => '/'
        ],
        'etc' => [
            'path' => '/'
        ]
    ]
    ...
];
```

Properti `path` harus ada, sementara properti `host` bersifat optional. Tapi, ketika
properti `host` tidak diset, maka URL pada gate tersebut akan menggunakan uri saja
tanpa hostname. Oleh karena itu, disarankan men-set properti `host` agar URL di 
sitemap/rss menjadi absolute path.

Jika properti `host` berisi test `HOST`, maka nilai tersebut akan diambil dari 
properti `host` konfigurasi aplikasi. Melihat contoh di atas, maka teks `HOST`
akan diubah menjadi `example.com`.

Melihat cara pendaftaran host di atas, maka bisa diambil simpulan bahwa:

1. Semua request dari domain `video.example.com` akan dianggap gate `video`.
1. Semua request dari domain `audio.website.com` akan dianggap gate `audio`.
1. Semua request dari domain `[apasaja].example.com` akan dianggap gate `micro`.
   Karena host selector disini menggunakan `:` maka nilai dari selector ini
   bisa diakses dari kontroler dengan perintah `$this->param->slug`.
1. Semua request dari domain apa saja dengan uri `/admin` akan dianggap gate `admin`.
1. Semua request dari domain `example.com` dengan uri apa saja akan dianggap gate `site`.
1. Semua request dari domain apa saja dengan uri apa saja akan dianggap gate `etc`.