# $attributes

呼び出し側で設定した属性を、コンポーネント側で受け取れる。

## merge()メソッド

Bladeコンポーネントで既存の属性に新しい属性を追加・上書きする。  
特に、`class`属性のような、複数の値を管理する場面で使える。

呼び出し元で渡した値で、完全に上書きする場合は、`merge()` は使わない。

コンポーネント

```php
<button {{ $attributes->merge(['class' => 'btn']) }}>
    {{ $slot }}
</button>
```

既に`class="btn"`が設定されているコンポーネント。  
呼び出し元からclass属性に追加がある場合、既存の`btn`に加えられる。

呼び出し元

```php
<x-button class="btn-primary" id="submit-button">
    送信
</x-button>
```

出力

```php
<button class="btn btn-primary" id="submit-button">
    送信
</button>
```

## 完全に上書きする場合

`merge()` を使わずに、`$attributes` をそのまま適用する。 

```php
<button {{ $attributes }}>
```