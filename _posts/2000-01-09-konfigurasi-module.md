---
layout: post
title: Konfigurasi Module
categories: core
---

Masing-masing module harus punya file konfigurasi yang disimpan di folder nya
sendiri. Konfigurasi ini berisi informasi nama, versi, repository, daftar file
module, daftar dependency, daftar service, autoloader dan lain-lain.

Bentuk di bawah adalah bentuk umum sebuah module:

```php
<?php
/**
 * my-module config file
 * @package my-module
 * @version 0.0.1
 * @upgrade true
 */

return [
    '__name' => 'my-module',
    '__version' => '0.0.1',
    '__git' => 'https://github.com/getphun/my-module',
    '__files' => [
        'modules/my-module/config.php'       => ['install', 'remove', 'update'],
        'modules/my-module/library/Mine.php' => ['install', 'remove'],
        'etc/trash/my-module'                => ['remove']
    ],
    '__dependencies' => [
        'admin',
        '/site-meta',
        'db-mysql/db-pgsql'
    ],
    '_services' => [
        'serv' => 'MyModule\\Service\\Serv'
    ],
    '_autoload' => [
        'classes' => [
            'MyModule\\Service\\Serv' => 'modules/my-module/service/Serv.php',
            'MyModule\\Controller\\HomeController' => 'modules/my-module/controller/HomeController.php'
        ],
        'files' => [
            'modules/my-modules/func' => 'modules/my-modules/func.js'
        ]
    ],
    
    '_routes' => [
        'site' => [
            'siteMyModuleIndex' => [
                'rule' => '/module',
                'handler' => 'MyModule\\Controller\\Home::index'
            ]
        ]
    ],
    
    'bakso' => 'Bandung'
];
```

Tidak semua properti harus terisi. Hanya ada beberapa konfigurasi yang harus ada,
seperti `__name`, `__version`, `__git`, dan `__files`.

Masing-masing nilai properti bisa diakses dari kontroller dengan perintah
`$this->config->{property}`. Sebagai contoh, untuk mendapatkan nilai properti
`bakso`, panggil `$this->config->bakso` dari kontroller.

Keterangan dari masing-masing properti adalah sebagai berikut:

## __name

Adalah nama module, harus sama dengan nama folder dimana file konfigurasi disimpan.
Silahkan mengacu ke penamaan module untuk informasi lebih jelas tentang bagaimana
cara penentuan nama module yang tepat.

## __version

Adalah versi module yang dimulai dari `0.0.1`. Metode penambahan versi diserahkan
sepenuhnya kepada developer ( untuk saat ini dan entah sampai kapan ). Nilai
properti ini harus berubah setiap kali module di-*push* ke git, dengan tujuan agar
aplikasi lain tahu kalau ada perubahan pada module.

## __git

Adalah git repository di mana module disimpan. Repository inilah yang akan digunakan
untuk mendownload versi terbaru module ini.

## __files

Adalah daftar file atau folder module. Nilai dari properti ini adalah array dimana
array key nya adalah path ke file module yang relative ke `BASEPATH` aplikasi
tanpa prefix `/`. Nilai dari masing-masing file path adalah array yang berisi
rule action file. Nilai rule yang dikenal sampai saat ini adalah `install`, `update`,
dan `remove`.

1. `install`  
   File atau folder ini akan ikut diproses pada saat module diinstall dari repository
   ke aplikasi.
1. `update`  
   File atau folder ini akan ikut diproses pada saat module diupdate dari repository
   ke aplikasi.
1. `remove`  
   File atau folder ini akan ikut diproses pada saat module dihapus dari aplikasi.

Ketika suatu path suatu file bernilai `['install', 'update']`, maka file/folder
tersebut akan ikut di-*copy* pada saat module diinstall, atau diupdate. Tapi akan
tertinggal di aplikasi pada saat module dihapus.

Begitu juga kalau nilai path file adalah `['install', 'remove']`, maka file/folder
tersebut akan ikut di-*copy* pada saat module diinstall, tapi tidak ikut terupdate
pada saat module diupdate, dan akan dihapus pada saat module dihapus dari aplikasi.

Lihat contoh konfigurasi di atas, file `modules/my-module/config.php` akan ikut
terbawa ke aplikasi pada saat module diinstall, dan diupdate, dan akan dihapus
ketika module dihapus. Ini adalah kondisi paling sering terjadi, dimana semua
file module ikut serta dalam semua aktifitas module. Kondisi seperti ini mungkin
terjadi pada module `admin-post`, karena pada umumnya halaman editor post di
masing-masing aplikasi selalu sama.

Sementara file `modules/my-module/library/Mine.php` akan ikut terbawa ke aplikasi
pada saat module diinstall, tapi tidak akan diupdate pada saat module diupdate,
dan terhapus ketika module dihapus. Kondisi seperti ini mungkin terjadi pada
module `site-post` dimana file kontroller mungkin berbeda antar satu aplikasi
dengan aplikasi yang lain, sehingga lebih baik menyerahkan isi file kontroller
tersebut ke developer pengguna module.

Yang terakhir adalah file `etc/trash/my-module` yang tidak ikut terinstall, tidak
juga ikut terupdate, tapi akan dihapus ketika module dihapus. Kondisi seperti ini
mungkin terjadi karena konten folder ini digenerasi pada saat aplikasi dijalankan
yang berisi file-file temporary yang sebenarnya bukan bagian dari module, tapi
menjadi tanggungjawab module.

## __dependencies

Adalah array yang berisi daftar module lain yang dibutuhkan oleh module ini agar
bisa berjalan dengan baik. Cara penulisan dependency yang benar adalah sebagai
berikut:

1. `module1`  
   Module mengharuskan module `module1` terinstall agar bisa berjalan dengan baik.
1. `module1/module2/module3`  
   Module membutuhkan salah satu module `module1` atau `module2` atau `module3`
   terinstall agar module bisa berjalan dengan baik. Nilai ini akan menerima salah
   satu atau semua module terinstall.
1. `/module1`  
   Module bisa bekerja dengan adanya `module1`, tapi juga tetap bisa bekerja
   walaupun module tersebut tidak terinstall. Jika module optional lebih dari satu,
   maka penulisannya bisa dipisahkan oleh karakter `/` sehingga penulisannya menjadi
   `/module1/module2/module3`.

Jika mengacu pada contoh konfigurasi di atas, maka kebutuhan akan module menjadi
seperti ini:

1. `admin`  
   Module `admin` harus terinstall.
1. `/site-meta`  
   Module `site-meta` boleh terinstall, tapi walaupun tidak ada, module tetap
   berjalan dengan baik.
1. `db-mysql/db-pgsql`  
   Salah satu atau keduanya dari module `db-mysql` dan `db-pgsql` harus terinstall.

## _services

Properti ini berisi daftar service yang didaftarkan pada aplikasi. Array key
adalah nama service yang didaftarkan, sementara nilainya adalah nama class
yang di-*construct* ketika aplikasi memanggil service. Service sendiri adalah
method yang ditambahkan ke object `$this` pada kontroller. Melihat contoh di atas,
maka kontroller memiliki service dengan nama `serv` yang bisa dipanggil dari
kontroller dengan perintah `$this->serv`, dimana method ini dibuat dengan perintah:

```php
<?php
// $this adalah kontroller yang sedang aktif berdasarkan request
$this->serv = new MyModule\Service\Serv();
```

## _autoload

Adalah konfigurasi yang berisi daftar class dan file yang diload ketika aplikasi
dijalankan. File class diload ketika dibutuhkan, sementara file langsung diload.

Pendaftaran class didaftarkan dengan ketentuan bahwa array key adalah nama class
lengkap dengan namespace nya, dan nilanya adalah relative path ke file tersebut.

Sementara pendaftaran file didaftarkan dengan ketentuan bahwa array key adalah
nama file beserta path nya, dan nilainya adalah relative path ke file tersebut.

## _routes

Adalah daftar routes dari gates yang didaftarkan oleh module sebagai cara
agar aplikasi tahu request mana yang bisa ditangani oleh module. Penjelasan
lebih lengkap tentang konfigurasi ini ada di pembahasan selanjutnya.

## bakso

Konfigurasi ini adalah konfigurasi tembahan yang bisa diakses dari kontroller
dengan perintah `$this->config->bakso`. Nilai dari konfigurasi ini bisa diakses
dari kontroler module manapun, dan mungkin bisa tertindih oleh konfigurasi lain
jika memiliki nama yang sama.