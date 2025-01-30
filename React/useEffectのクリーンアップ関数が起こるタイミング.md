# useEffectのクリーンアップ関数が起こるタイミング

```tsx
useEffect(() => {
    const interval = setInterval(() => {
        if (!isDragging) {
            setCurrentIndex((prevIndex) => (prevIndex + 1));
        }
    }, 2000);

    return () => clearInterval(interval); // クリーンアップ関数
}, [isDragging]);
```

## クリーンアップ関数が発生するタイミング

* 次の useEffect 実行時
* コンポーネントのアンマウント時

## 上記のコードでの動作の流れ

### 1, 初回実行時

1. コンポーネントがマウントされると `useEffect` が実行される。
2. `setInterval` が実行され、新しいタイマー（例：ID 1）が開始される。
3. `useEffect` 内のクリーンアップ関数 `clearInterval` はまだ実行されない（次回実行時かアンマウント時まで待機）。

### 2, 依存値（isDragging）が変化した場合

1. `isDragging` の変更により、`useEffect` が再実行される。
2. 再実行の前に、Reactは前回の `setInterval` をクリーンアップ（削除） する。
    `clearInterval(interval)` により、前回のタイマー（ID 1）がクリアされる。
3. その後、新しい `setInterval` が実行され、新しいタイマー（例：ID 2）が作成される。

つまり、古いタイマーが確実にクリアされ、新しいものが作り直される という流れになる。

### 3, コンポーネントのアンマウント時

コンポーネントがアンマウントされる（画面から削除される）と、Reactはクリーンアップ関数を実行する。

`clearInterval(interval)` が呼ばれ、最後にセットされたタイマーを削除。