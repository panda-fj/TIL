#  js-obfuscator

<https://github.com/javascript-obfuscator/javascript-obfuscator>

Javascript用のフリー難読化ツール

## インストール

npxで実行してもいい

```
npm i -D javascript-obfuscator
```

## CLI

### 基本

```
// input-obfuscated.js が自動生成
npx javascript-obfuscator input.js
```

```
// 出力ファイル名をカスタム
npx javascript-obfuscator input.js -o output.js
```

### オプション

日本語説明  
<https://qiita.com/u83unlimited/items/970f819d1fafa325bfbf>


```
    -v, --version
    -h, --help

    -o, --output

    --compact <boolean>
    --config <string>
    --control-flow-flattening <boolean>
    --control-flow-flattening-threshold <number>
    --dead-code-injection <boolean>
    --dead-code-injection-threshold <number>
    --debug-protection <boolean>
    --debug-protection-interval <number>
    --disable-console-output <boolean>
    --domain-lock '<list>' (comma separated)
    --domain-lock-redirect-url <string>
    --exclude '<list>' (comma separated)
    --force-transform-strings '<list>' (comma separated)
    --identifier-names-cache-path <string>
    --identifier-names-generator <string> [dictionary, hexadecimal, mangled, mangled-shuffled]
    --identifiers-dictionary '<list>' (comma separated)
    --identifiers-prefix <string>
    --ignore-imports <boolean>
    --log <boolean>
    --numbers-to-expressions <boolean>
    --options-preset <string> [default, low-obfuscation, medium-obfuscation, high-obfuscation]
    --rename-globals <boolean>
    --rename-properties <boolean>
    --rename-properties-mode <string> [safe, unsafe]
    --reserved-names '<list>' (comma separated)
    --reserved-strings '<list>' (comma separated)
    --seed <string|number>
    --self-defending <boolean>
    --simplify <boolean>
    --source-map <boolean>
    --source-map-base-url <string>
    --source-map-file-name <string>
    --source-map-mode <string> [inline, separate]
    --source-map-sources-mode <string> [sources, sources-content]
    --split-strings <boolean>
    --split-strings-chunk-length <number>
    --string-array <boolean>
    --string-array-calls-transform <boolean>
    --string-array-calls-transform-threshold <number>
    --string-array-encoding '<list>' (comma separated) [none, base64, rc4]
    --string-array-indexes-type '<list>' (comma separated) [hexadecimal-number, hexadecimal-numeric-string]
    --string-array-index-shift <boolean>
    --string-array-rotate <boolean>
    --string-array-shuffle <boolean>
    --string-array-wrappers-count <number>
    --string-array-wrappers-chained-calls <boolean>
    --string-array-wrappers-parameters-max-count <number>
    --string-array-wrappers-type <string> [variable, function]
    --string-array-threshold <number>
    --target <string> [browser, browser-no-eval, node]
    --transform-object-keys <boolean>
    --unicode-escape-sequence <boolean>
```

### コードストック

```
npx javascript-obfuscator content.js  --source-map true --dead-code-injection true --dead-code-injection-threshold 0.7 --self-defending true
```