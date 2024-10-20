# build

## vanilla / index.htmlは無視 / srcの中のtsファイルを個別にbuild

```ts
// vite.comfig.ts

import { defineConfig } from 'vite';
import { resolve } from 'path';
import fs from 'fs';

// srcディレクトリ内のすべてのtsファイルを取得
const tsFiles = fs.readdirSync(resolve(__dirname, 'src'))
  .filter(file => file.endsWith('.ts'))
  .map(file => resolve(__dirname, 'src', file));

export default defineConfig({
  build: {
    rollupOptions: {
      input: tsFiles, // 全てのtsファイルをinputに設定
      output: {
        // 任意で出力の設定（ファイル名、フォルダなど）を調整可能
        entryFileNames: '[name].js',
        dir: resolve(__dirname, 'dist'), // distフォルダに出力
      },
    },
  },
});

```

## vanilla / index.htmlを含む / srcの中のtsファイルを個別にbuild

```ts
import { defineConfig } from 'vite';
import { resolve } from 'path';
import fs from 'fs';

// srcディレクトリ内の全てのtsファイルを取得
const tsFiles = fs.readdirSync(resolve(__dirname, 'src'))
  .filter(file => file.endsWith('.ts'))
  .map(file => resolve(__dirname, 'src', file));

// ルートディレクトリ内のhtmlファイルを取得
const htmlFiles = fs.readdirSync(resolve(__dirname))
  .filter(file => file.endsWith('.html'))
  .map(file => resolve(__dirname, file));

export default defineConfig({
  build: {
    rollupOptions: {
      input: [
        ...tsFiles, // src内のtsファイル
        ...htmlFiles, // ルートディレクトリ内のhtmlファイル
      ],
      output: {
        // 任意で出力設定を調整
        entryFileNames: '[name].js',
        dir: resolve(__dirname, 'dist'), // distフォルダに出力
      },
    },
  },
});
```