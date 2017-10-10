---
layout: post
title: Service Param
categories: core
---

Adalah service yang menyediakan data yang diambil dari variable router. Service
param bisa dipanggil dari kontroler dengan perintah `$this->param->{placeholder}`.

Sebagai contoh, jika aplikasi memiliki daftar router seperti di bawah:

```php
<?php

return [
    'name' => 'my-app',
    'host' => 'example.com',
    
    '_gates' => [
        'vendor' => [
            'host' => ':vend.HOST',
            'path' => '/'
        ]
    ],
    
    '_routes' => [
        'vendor' => [
            'vendorPage' => [
                'rule' => '/page/:slug',
                'handler' => '...'
            ]
        ]
    ]
];
```

Ketika user akses url `http://mince.example.com/page/about-us`, maka kontroler
bisa mengakses nilai-nilai `vend`, dan `slug` dengan cara seperti di bawah:

```php
<?php

...
function indexAction(){
    $vend = $this->param->vend; // mince
    $slug = $this->param->slug; // about-us
    $none = $this->param->nonexists; // null
}
...
```