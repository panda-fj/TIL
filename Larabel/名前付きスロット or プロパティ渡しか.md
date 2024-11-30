# x-slot か プロパティを渡すか

## x-slot が適している場面

layout.blade.php

```php
<div class="layout">
    <header>
        <h1>{{ $header }}</h1>
    </header>
    <nav>
        {{ $navigation }}
    </nav>
    <main>
        {{ $slot }}
    </main>
    <footer>
        {{ $footer }}
    </footer>
</div>
```

名前付きスロットの場合

```php
<x-layout>
    <x-slot:header>
        Home Page
    </x-slot:header>
    
    <x-slot:navigation>
        <ul>
            <li><a href="/">Home</a></li>
            <li><a href="/about">About</a></li>
            <li><a href="/contact">Contact</a></li>
        </ul>
    </x-slot:navigation>
    
    <h1>Welcome to the Home Page!</h1>
    
    <x-slot:footer>
        &copy; 2024 My Website
    </x-slot:footer>
</x-layout>
```

プロパティ渡しの場合

```php
<x-layout 
    :header="'Home Page'"
    :navigation="'<ul><li><a href=\"/\">Home</a></li><li><a href=\"/about\">About</a></li><li><a href=\"/contact\">Contact</a></li></ul>'"
    :footer="'&copy; 2024 My Website'"
>
    <h1>Welcome to the Home Page!</h1>
</x-layout>
```

* 複数のスロットが必要な場合に、どの部分に何を渡しているかを理解できやすくなる。
* Bladeディレクティブを使った動的なリストや、HTMLを含む構造を簡単に挿入できる。（HTMLをプロパティで渡す場合は、HTMLを文字列として扱う必要があるため、エスケープやインデントが壊れやすくなる。）