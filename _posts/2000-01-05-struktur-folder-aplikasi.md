---
layout: post
title: Struktur Folder Aplikasi
categories: core
---

Begitu module `core` terinstall, maka struktur folder di bawah yang akan kita lihat
pada folder dimana aplikasi diinstall.

```
.htaccess
index.php
etc/
    .env
    config.php
    cache/
    log/
modules/
    core/
        config.php
        Phun.php
        controller/
            HomeController.php
        helper/
            devel.php
            page.php
        library/
            Controller.php
            Router.php
            Server.php
            View.php
        service/
            Cache.php
            Config.php
            Param.php
            Request.php
            Response.php
            Router.php
theme/
    site/
        404.phtml
        index.phtml
        static/
```

Folder `etc` adalah folder yang menyimpan informasi-informasi khusus aplikasi.
Folder ini berisi cache, file konfigurasi aplikasi, dan file `.env` yang 
menyimpan informasi environment aplikasi.

Folder `modules` adalah tempat dimana semua folder module harus disimpan.

Folder `theme` adalah folder yang menyimpan semua template site. Masing-masing
gate memiliki satu theme.