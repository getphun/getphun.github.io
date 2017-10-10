---
layout: post
title: Konfigurasi Router
categories: core
---

Di sebagian besar kasus, module adalah bagian yang mendaftarkan router. Hal ini
karena module yang tahu betul tugas apa yang bisa mereka kerjakan, dan darimana
seharusnya user datang untuk mendapat pelayanan dari module.

Routes adalah koleksi rule URL yang di test untuk menentukan module mana yang
bertugas mem-proses request user. Sebelum melakukan pemilihan router, system
melakukan pemilihan gate untuk menentukan koleksi router mana yang digunakan untuk
filter routes.

Masing-masing router yang didaftarkan harus berada di bawah gate.

Script di bawah adalah contoh pendaftaran router di konfigurasi module:

```php
<?php

return [
    '__name' => 'my-module',
    ...
    '_autoload' => [
        'classes' => [
            'AdminController' => 'modules/my-module/cont~/AdminController.php',
            'SiteController' => 'modules/my-modules/cont~/SiteController.php',
            ...
        ],
        ...
    ],
    
    '_routes' => [
        'admin' => [
            '404' => [
                'handler' => 'Admin::notFound'
            ],
            'adminHome' => [
                'rule' => '/',
                'handler' => 'Admin::index'
            ],
            'adminUser' => [
                'rule' => '/user',
                'handler' => 'User::index'
        ],
        'site' => [
            '404' => [
                'handler' => 'Site::notFound'
            ],
            'siteHome' => [
                'rule' => '/',
                'handler' => 'Site::home'
            ],
            'sitePageSingle' => [
                'rule' => '/page/:slug',
                'priority' => 11,
                'handler' => 'Site::single'
            ],
            'sitePages' => [
                'rule' => '/page',
                'priority' => 10,
                'module' => 'site',
                'method' => 'GET',
                'handler' => 'Site::index'
        ]
    ]
];
```

## Nama Route

Masing-masing route harus didaftarkan di bawah gate. Dan semua route harus
punya nama, pada contoh di atas, nama-nama route adalah array key yang tepat di 
bawah nama gate, seperti `404`, `adminHome`, `adminUser`, `siteHome`, dll.

Masing-masing developer bebas membuat nama route sendiri. Tapi sangat disarankan
dengan menggunakan bentuk seperti `[gate][Module][Action][Additional]`.

Sebagai contoh, nama router untuk module `MyModule` dengan action `single` yang
didaftarkan di bawah gate `site` adalah `siteMyModuleSingle`. Sementara route
untuk module yang sama dengan action `index` adalah `siteMyModuleIndex`.

## 404

Route dengan nama '404' adalah route spesial. Route ini akan dipanggil ketika tidak
ada route yang cocok untuk request yang sedang berlangsung di gate yang aktif.

Satu-satunya properti yang dibutuhkan di route 404 adalah `handler`.

## Properti 

Masing-masing route membutuhkan setidaknya dua properti, yaitu `rule` dan `handler`.
Keterangan masing-masing properti adalah sebagai berikut:

### handler

Adalah properti yang berisi informasi kontroler dan method yang dipanggil untuk
menyelesaikan request ini. Hilangkan suffix `Controller` pada nama class, dan
`Action` pada nama method. Sebagai contoh, untuk menyerahkan request ke kontroler
`AdminController` dengan action `indexAction`, maka tuliskan `Admin::index`.

Jika kontroler memiliki namespace, pastikan menuliskan lengkap dengan namespace nya.

### method

Adalah filter untuk request method. Properti ini menerima nilai seperti `GET`, `POST`,
`PUT`, dan `DELETE`. Route akan diproses hanya jika nilai method sama dengan nilai
request method. Jika tidak diset, maka semua method diterima.

### module

Adalah properti yang berisi module yang harus ada agar route tersebut diikutkan dalam
daftar routes yang akan ditest. Jika module dengan nama nilai properti ini tidak
terinstall, maka route ini tidak digunakan.

### priority

Pengurutan route berdasarkan nilai prioritas. Nilai prioritas paling tinggi akan
dites terlebih dahulu, dan dilanjutkan dengan route dengan prioritas paling rendah.
Jika nilai ini tidak diset, maka nilai 1.000.000 akan digunakan.

### rule

Properti ini menentukan struktur path request. Boleh nilai absolut, atau dengan
variable jika diawali dengan `:`. Jika pada route terdapat bagian variable, maka
nilai dari variable tersebut bisa diakses dari kontroler dengan perintah
`$this->param->{name}` dimana name adalah placeholder yang digunakan untuk
variable. Pada contoh route dengan nama `sitePageSingle` di atas, kontroler bisa
memanggil perintah `$this->param->slug` untuk mendapatkan bagian url yang tepat
dibagian `:slug`.

Nilai `path` dari gate otomatis ditambahkan sebagai prefix masing-masing rule.
Sehingga tidak perlu lagi menambahkan prefix path. Sebagai contoh, jika nilai 
properti `path` untuk gate `admin` adalah `/admin`, maka route dengan nama
`adminUser` yang terpilih ketika user mengakses `/admin/user`.