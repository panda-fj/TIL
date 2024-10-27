# keyboardイベント

## keydown, keypress, keyup の違い

* `keydown`: キーが押された瞬間に発生し、キーを押し続けている間も繰り返し発生する。Shift, Ctrl, F1などにも対応。
* `keyup`: キーが離された瞬間に発生する。キーが離れた時点の状態を取得する場合に使われる。
* `keypress`: 現在は非推奨。文字キーにのみ反応するイベント。実際には `keydown` イベントを使用することが推奨。

## 修飾キー（Shift, Ctrl, Altなど）との組み合わせ

```js
document.addEventListener('keydown', (event) => {
    if (event.ctrlKey && event.key === 'z') {
        console.log('Ctrl + Z pressed');
    }
});
```

## event.code と event.key

`event.key` は、押されたキーに応じて値が変わる。  
例えば、「A」を押すと "A"、小文字の「a」を押すと "a" になる。

（`event.keyCode` は非推奨となり、`event.key` が推奨）


```js
document.addEventListener('keydown', (event) => {
    console.log(event.key); // "A" や "a" が出力される
});
```

`event.code` は、キーボード上の物理的なキーのコード。物理的なキー位置に基づいて動作する。

<https://developer.mozilla.org/ja/docs/Web/API/KeyboardEvent/code>

## `keydown` の重複イベント防止

キーを押し続けている間にイベントは繰り返し発生するため、処理を1度だけ行いたい場合は、`event.repeat`プロパティで状態を管理する。

```js
document.addEventListener('keydown', (event) => {
    if (event.repeat) return; // 押しっぱなしなら処理しない
    console.log('Key pressed once:', event.key);
});

```

## `preventDefault()`

キーボードイベントがブラウザのデフォルト動作に干渉する場合、その動作を `event.preventDefault()` で無効化できる。

```js
document.addEventListener('keydown', (event) => {
    if (event.key === 'Enter') {
        event.preventDefault(); // デフォルトのEnterキー動作を無効化
        console.log('Enter key pressed but default action prevented');
    }
});

```

##  アクセシビリティ（A11y）対応

特定のキーボードショートカットやイベントを処理する場合は、アクセシビリティに注意する必要がある。

特に、タブキーや矢印キーのカスタム挙動を追加する場合は、スクリーンリーダーのユーザーがアクセスできなくなるリスクがあるため、標準のキーボード操作を妨げないように工夫することが重要。