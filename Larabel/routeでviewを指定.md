# routeでviewを指定

デフォルトで、`resources\routes\**.php` などで、viewを設定する。

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('home');
});

Route::get('/about', function () {
    return view('about');
});

Route::get('/contact', function () {
    return view('contact');
});
```

## 第二引数でプロパティを設定する

```php
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('home', [
        'greeting' => 'Yo',
        'name' => 'John'
    ]);
});
```

view 側で `$greeting`, `$name` として利用できる。