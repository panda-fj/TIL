# vite.config.ts - distの構造

buildするファイルを指定して、`dist`内の構造を変更するときは、`rollupOptions` の `input` と `output` を編集する。

## ルート内の特定のディレクトリ（サブディレクトリは含まない）を、構造・ファイル名をそのまま build する

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

// root内の特定のファイルを追加
const additionalFiles = {
    '特定のファイル名': resolve(__dirname, '特定のファイル名.js'), // 例: 'main': resolve(__dirname, 'main.js')
};


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

## サブディレクトリを含める場合

インポートパスの問題が残るが、アセットを移動させるのには使えるかも

```ts
function getFilesWithoutExtension(dirName: string): Record<string, string> {
    const result: Record<string, string> = {};

    function traverseDirectory(currentDir: string) {
        const files = fs.readdirSync(currentDir);

        files.forEach(file => {
            const filePath = resolve(currentDir, file);
            const relativePath = path.relative(__dirname, filePath);

            if (fs.statSync(filePath).isDirectory()) {
                // サブディレクトリの場合は再帰処理を行う
                traverseDirectory(filePath);
            } else {
                // ファイルの場合はRecordに追加
                const key = path.join(path.dirname(relativePath), path.basename(relativePath, path.extname(relativePath)));
                result[key] = filePath;
            }
        });
    }

    // 最初のディレクトリから走査を開始
    traverseDirectory(resolve(__dirname, dirName));

    return result;
}
```