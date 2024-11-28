# $slot

Laravelの`$slot`は、Bladeコンポーネントで使用される特別な変数で、親コンポーネントから子コンポーネントに渡される内容を表す。

## 基本的な使い方

呼び出し元は、`<x-alert>`タグの中に、文字列を挿入している。

```php
<x-alert>
    これはスロットを使ったコンテンツです。
</x-alert>
```

コンポーネント側

```
<div class="alert alert-warning">
    {{ $slot }}
</div>
```

最終的なHTML

```html
<div class="alert alert-warning">
    これはスロットを使ったコンテンツです。
</div>
```

## スロットの仕組み

### デフォルト

`{{ $slot }}`は、コンポーネントタグ内のすべてのコンテンツを受け取る。（上記の例）

### 名前付きスロット

`<x-slot>` を使って、任意の名前を設定できる。

呼び出し

```php
<x-layout>
    <x-slot:heading>
        Home Page
    </x-slot:heading>
    <h1>Home Page!!</h1>
</x-layout>
```

コンポーネント内

```php
<div>
    <header>{{ $heading }}</header>
    <main>{{ $slot }}</main>
</div>
```