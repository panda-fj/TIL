# vite.config.ts - distの構造

buildするファイルを指定して、`dist`内の構造を変更するときは、`rollupOptions` の `input` と `output` を編集する。

## ルート内の特定のディレクトリを、構造・ファイル名をそのまま build する

`popup` フォルダと、`src` フォルダをbuild対象にする

```ts
import { defineConfig } from 'vite';
import { resolve } from 'path';
import fs from 'fs';
import path from 'path';

/**
 * rootからの相対パス(拡張子なし)をキー、絶対パスを値にした、レコードを作成する
 * @param dirName ディレクトリ名
 * @returns 
 */
function getFilesWithoutExtension(dirName: string): Record<string, string> {
    return fs.readdirSync(resolve(__dirname, dirName)).reduce((entries, file) => {
        const filePath = resolve(__dirname, dirName, file);
        const relativePath = path.relative(__dirname, filePath);
        entries[path.join(path.dirname(relativePath), path.basename(relativePath, path.extname(relativePath)))] = filePath;
        return entries;
    }, {} as Record<string, string>);
}

// popupフォルダ内のすべてのファイルを動的に取得
const popupFiles = getFilesWithoutExtension('popup');

// srcフォルダ内のすべてのファイルを動的に取得
const srcFiles = getFilesWithoutExtension('src');


export default defineConfig({
    build: {
        rollupOptions: {
            input: {
                ...popupFiles,
                ...srcFiles
            },
            output: {
                entryFileNames: () => {
                    return '[name].js';
                },
                assetFileNames: (chunkInfo) => {
                    if (chunkInfo.originalFileNames) {
                        return `${chunkInfo.originalFileNames[0]}`;
                    }
                    return '';
                },
                
            },
            
        }
    }
});
```