# vite.config.ts - javascript-obfuscator

## Typescript

```ts
import { defineConfig } from 'vite';
import javascriptObfuscator from 'javascript-obfuscator';
import { Plugin, OutputOptions, OutputBundle } from 'rollup';

export default defineConfig({
    build: {
        rollupOptions: {
            output: {
                plugins: [
                    {
                        name: 'obfuscator',
                        generateBundle(outputOptions: OutputOptions, bundle: OutputBundle) {
                            for (const fileName in bundle) {
                                if (bundle[fileName].type === 'chunk') {
                                    const chunk = bundle[fileName];
                                    if (chunk && 'code' in chunk) {
                                        const originalCode = chunk.code;
                                        const obfuscatedCode = javascriptObfuscator.obfuscate(originalCode, {
                                            compact: true,
                                            controlFlowFlattening: true
                                        }).getObfuscatedCode();
                                        chunk.code = obfuscatedCode;
                                    }
                                }
                            }
                        }
                    } as Plugin
                ]
            }
        }
    }
});

```

## 

```ts
const obfuscatedCode = javascriptObfuscator.obfuscate(originalCode, {
                                            compact: true,
                                            controlFlowFlattening: true
                                        }).getObfuscatedCode();

```

1. **`import { defineConfig } from 'vite';`**  
   **説明**: Viteの設定を定義するために`defineConfig`をインポートします。これにより、設定ファイルがViteの標準形式で書かれ、コード補完なども有効になります。

2. **`import javascriptObfuscator from 'javascript-obfuscator';`**  
   **説明**: 難読化を行うための`javascript-obfuscator`モジュールをインポートします。これは実際のコードを難読化するために使われます。

3. **`export default defineConfig({...})`**  
   **説明**: Viteの設定をエクスポートします。`defineConfig`関数内にViteの設定オブジェクトを渡します。

4. **`build: {...}`**  
   **説明**: `build`オプションは、Viteのビルドプロセスに関する設定を定義する場所です。この中で、Rollupに関連するカスタマイズやプラグインの設定が行われます。

5. **`rollupOptions: {...}`**  
   **説明**: Viteはビルド時にRollupを使っているため、`rollupOptions`を使ってカスタムの設定やプラグインを指定できます。この中で、難読化プラグインを組み込みます。

6. **`output: {...}`**  
   **説明**: 生成されたバンドルファイルに対する設定を行う場所です。ここにプラグインを指定することで、ビルドの最終段階で生成されたコードに対して処理を加えます。

7. **`plugins: [{ ... }]`**  
   **説明**: Rollupのプラグインは、この`plugins`オプションを使って追加します。ここでは、`generateBundle`というイベントをフックし、ビルドが完了した後のコードに対して難読化を行います。

8. **`generateBundle(outputOptions, bundle)`**  
   **説明**: `generateBundle`は、ビルドされたバンドルファイルが生成される直前に実行されるRollupのフックです。これにより、生成されたコードに対して最後の処理を行うことができます。  
   - **`outputOptions`**: 出力に関するオプション。  
   - **`bundle`**: バンドルされたファイル群のオブジェクト。このオブジェクトの中に生成されたJavaScriptファイルが格納されます。

9. **`for (const fileName in bundle) {...}`**  
   **説明**: `bundle`オブジェクト内には複数のファイル（chunk）が含まれていることがあるため、ループを使ってすべてのファイルに処理を適用します。  
   **`if (bundle[fileName].type === 'chunk'):`** ここでは、JavaScriptファイル（chunk）に対してのみ処理を適用するようにしています。

10. **`javascriptObfuscator.obfuscate(originalCode, {...})`**  
    **説明**: `javascript-obfuscator`の`obfuscate`メソッドを使って、生成されたJavaScriptコードを難読化します。このメソッドは、元のコードとオプションを受け取り、難読化されたコードを返します。  
    - **`compact: true`**: 難読化されたコードをさらにコンパクトにします。  
    - **`controlFlowFlattening: true`**: コードのフローをさらに難読化し、理解しづらくしますが、パフォーマンスに若干の影響を与える可能性があります。

11. **`bundle[fileName].code = obfuscatedCode;`**  
    **説明**: 難読化されたコードを元のバンドルオブジェクトに置き換えます。これにより、ビルド後の最終的なファイルに難読化されたコードが含まれることになります。


## Javascript

型アノテーションがない

```js
import { defineConfig } from 'vite';
import javascriptObfuscator from 'javascript-obfuscator';

export default defineConfig({
    build: {
        rollupOptions: {
            output: {
                plugins: [
                    {
                        name: 'obfuscator',
                        generateBundle(outputOptions, bundle) {
                            for (const fileName in bundle) {
                                if (bundle[fileName].type === 'chunk') {
                                    const originalCode = bundle[fileName].code;
                                    const obfuscatedCode = javascriptObfuscator.obfuscate(originalCode, {
                                        compact: true,
                                        controlFlowFlattening: true
                                    }).getObfuscatedCode();
                                    bundle[fileName].code = obfuscatedCode;
                                }
                            }
                        }
                    }
                ]
            }
        }
    }
});

```