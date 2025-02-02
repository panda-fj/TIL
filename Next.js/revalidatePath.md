# revalidatePath

<https://nextjs.org/docs/app/api-reference/functions/revalidatePath>

<https://zenn.dev/417/scraps/908a1cad6b537e>

指定したパスのキャッシュを無効化し、再生成するための仕組み。

> revalidatePathは、含まれるパスが次に訪問されたときにのみキャッシュを無効にします。つまり、動的なルートセグメントで revalidatePath を呼び出しても、一度に多くの再検証が行われることはありません。無効化は、パスが次に訪問されたときにのみ行われます。

```tsx
revalidatePath(path: string, type?: 'page' | 'layout'): void;
```

`path`が動的セグメントを含む場合（例: `/product/[slug]/page`）、`type`パラメータは必須です。

`path` がリテラルルートセグメントを参照する場合（例: `/product/1` ）、 `type` を指定するべきではありません。


## 使用例

* Server Action

```tsx
// app/actions.ts
'use server'
 
import { revalidatePath } from 'next/cache'
 
export default async function submit() {
  await submitForm()
  revalidatePath('/')
}
```

* Route Handler

```tsx
// app/api/revalidate/route.ts

import { revalidatePath } from 'next/cache'
import type { NextRequest } from 'next/server'
 
export async function GET(request: NextRequest) {
  const path = request.nextUrl.searchParams.get('path')
 
  if (path) {
    revalidatePath(path)
    return Response.json({ revalidated: true, now: Date.now() })
  }
 
  return Response.json({
    revalidated: false,
    now: Date.now(),
    message: 'Missing path to revalidate',
  })
}
```

