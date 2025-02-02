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

Next.jsの `<Link>` コンポーネント特有の機能（`prefetch`など）により、アプリ内のページ遷移を最適化する。

## \<a>

`<a>` は HTML の標準のアンカータグ で、主にリンクを作成する。

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

`href`に外部リンクを指定する場合、`<Link>`を使用しても内部ルーティングの最適化のような処理は行われない。   
最終的に`<a>`タグをそのまま出力するだけなので、そのまま`<a>`タグを使用したほうがシンプルになる。

## 具体的な分岐

```js
// node_modules\next\dist\client\link.js
function prefetch(router, href, as, options) {
    if (!(0, _islocalurl.isLocalURL)(href)) {
        return;
    }
}
```

外部リンク `(isLocalURL(href) === false)` であれば `prefetch` は実行されない。

```js
if ((0, _utils.isAbsoluteUrl)(as)) {
    childProps.href = as;
} else if (!legacyBehavior || passHref || child.type === 'a' && !('href' in child.props)) {
    const curLocale = typeof locale !== 'undefined' ? locale : router == null ? void 0 : router.locale;
    const localeDomain = (router == null ? void 0 : router.isLocaleDomain) && (0, _getdomainlocale.getDomainLocale)(as, curLocale, router == null ? void 0 : router.locales, router == null ? void 0 : router.domainLocales);
    childProps.href = localeDomain || (0, _addbasepath.addBasePath)((0, _addlocale.addLocale)(as, curLocale, router == null ? void 0 : router.defaultLocale));
}
```

`href` が外部リンク (`isAbsoluteUrl(as) === true`) の場合、Next.js のルーターは使用されず、通常の `<a>` タグとして処理される

```js
return legacyBehavior ? /*#__PURE__*/ _react.default.cloneElement(child, childProps) : /*#__PURE__*/ (0, _jsxruntime.jsx)("a", {
    ...restProps,
    ...childProps,
    children: children
});
```

`legacyBehavior` が `false` の場合、 `children` を `<a>` タグでラップし、 `childProps` などのプロパティを適用してレンダリングする。