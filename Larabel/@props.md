# @props

Bladeコンポーネントのプロパティを定義するもの。  
配列形式で、プロパティ名とデフォルト値を設定できる。  
このプロパティは、Bladeテンプレート内で、変数として利用できる。

## 例

```php
@props(['active' => false])

<div class="{{ $active ? 'bg-blue-500' : 'bg-gray-500' }}">
    コンポーネントの内容
</div>
```

* @props で active プロパティを定義し、デフォルト値を false に設定。
* 親コンポーネントから値を渡さない場合、$active の値は、デフォルトの false になる。
* 親コンポーネントから値を渡した場合、その値が $active に代入される。

## 変数としての利用

`@props(['active' => false])` は、同じファイル内で `$active` という変数として利用できるようになる。

```php
@props(['active' => false])

<div class="{{ $active ? 'active-class' : 'inactive-class' }}">
    {{ $active ? 'アクティブ状態です' : '非アクティブ状態です' }}
</div>
```

## 複数の値の設定

```php
@props(['active' => false, 'type' => 'a'])
```