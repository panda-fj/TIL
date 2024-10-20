# vite.config.ts - pluginsの位置

## outputの中

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

## rollupOptionsの中（outputの下）

```ts
import { defineConfig } from 'vite';
import javascriptObfuscator from 'javascript-obfuscator';
import { Plugin, OutputOptions, OutputBundle } from 'rollup';

export default defineConfig({
    build: {
        rollupOptions: {
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
});
```

## 違い

Rollupでは `plugins` は `rollupOptions` のトップレベルに置くことが推奨される。  
しかし、Viteでは `output.plugins` もサポートされており、Viteでは `output` の中に `plugins` を配置しても動く。

プラグインがすべての出力に適用される場合、RollupやViteの設定ファイルのトップレベル（`rollupOptions` の直下）に `plugins` を配置するのが一般的らしい。