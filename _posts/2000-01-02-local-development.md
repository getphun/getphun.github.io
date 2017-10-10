---
layout: post
title: Development Lokal
categories: global
---

Untuk mempermudah proses development aplikasi dengan framework Phun, sangat
disarankan mengikuti beberapa hal di bawah:

## CLI

Install Phun CLI. Tools ini banyak membantu dalam proses pembuatan dan development
module. Mulai dari pembuatan module, model, sync dari module development ke 
aplikasi, dan lain-lain.

## Reusable Module

Sangat disarankan memegang prinsip reusable module bagi developer yang akan
membuat module. Satu module yang sedang didevelop mungkin akan digunakan di 
aplikasi lain. Mengetahui hal ini, module harus distruktur sebaik mungkin agar
setiap kali ada perubahan pada module, aplikasi tidak perlu melakukan perubahan
yang memberatkan.

Ada beberapa bagian pada module yang diperbolehkan diubah begitu sudah digabungkan
dengan aplikasi, dan ada juga beberapa bagian yang berbahaya kalau diubah.

Beberapa bagian module yang pada umumnya boleh diubah setelah digabungkan dengan
aplikasi adalah:

1. view/template
1. site kontroler
1. events listener

Dan beberapa hal yang tidak boleh diubah walaupun module tersebut sudah digabungkan
dengan aplikasi adalah:

1. konfigurasi module
1. meta generator
1. feed/sitemap kontroler

Dengan mengatur dengan baik bagian-bagian yang boleh, dan yang tidak boleh diupdate,
aplikasi jadi lebih mudah diperbaharui ketika ada update pada module.

## Struktur Folder

Karena developer harus mempertimbangkan reusable module, sebaiknya folder
development module dipisahkan dari aplikasi. Struktur di bawah bisa digunakan
untuk pemisahan folder aplikasi dan module:

```
/var/www/html/
    project1/
        _source/
            module1/
            module2/
            module3/
        site/
            index.php
            etc/
                config.php
                cache/
            modules/
                module1/
                module2/
                module3/
```

Dengan struktur folder seperti ini, semua module didevelop di folder `_source`
dan di-*sync* menggunakan CLI setiap kali ada perubahan pada module.

Sementara di folder `site` hanya melakukan development jika memang harus melakukan
perubahan pada file-file module yang boleh diubah.

Pastikan vhost untuk domain development dipointing ke folder `site`.