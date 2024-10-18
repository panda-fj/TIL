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

* `generateBundle(outputOptions, bundle)`  
ビルドされたバンドルファイルが生成される直前に実行されるRollupのフック。生成されたコードに対して最後の処理を行うことができる。  
`outputOptions`: 出力に関するオプション。  
`bundle`: バンドルされたファイル群のオブジェクト。このオブジェクトの中に生成されたJavaScriptファイルが格納される。

* `for (const fileName in bundle) {...}`  
`bundle`オブジェクト内には複数のJavaScriptファイル（chunk）が含まれていることがあるため、ループを使ってすべてのファイルに処理を適用する。  
`if (bundle[fileName].type === 'chunk'):` JavaScriptファイル（chunk）に対してのみ処理を適用するようにしている。

* `javascriptObfuscator.obfuscate(originalCode, {...})`  
`javascript-obfuscator`の`obfuscate`メソッドを使って、生成されたJavaScriptコードを難読化する。元のコードとオプションを受け取り、難読化されたコードを返す。 
  
* `bundle[fileName].code = obfuscatedCode;`   
難読化されたコードを元のバンドルオブジェクトに置き換える。これにより、ビルド後の最終的なファイルに難読化されたコードが含まれることになる。



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