# Linkタグとaタグ

<https://nextjs.org/docs/app/api-reference/components/link>

## \<Link>

`<Link>` は HTML `<a>` 要素を拡張した React コンポーネントで、プリフェッチとルート間のクライアントサイドナビゲーションを提供する。

```tsx
import Link from 'next/link'
 
export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

`prefetch`などにより、アプリ内のページ遷移を最適化する。

## \<a>

```tsx
export default function Page() {
  return (
    <a
        href="https://example.com"
        target="_blank"
        className="underline font-semibold"
    >
        Example
    </a>
  )
}
```

## 具体的な分岐

```js
function prefetch(router, href, as, options) {
    if (!(0, _islocalurl.isLocalURL)(href)) {
        return;
    }
}
```

外部リンク `(isLocalURL(href) === false)` であれば `prefetch` は実行されない。