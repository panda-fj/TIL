# ESModulesで使用する方法

Windowsを想定  
JestはCommonJSを前提にしているため、ESModulesで使用するにためには準備が必要

## cross-envのインストール

```
npm i -D cross-env
```

## package.jsonの編集

`scripts`,`type` を編集する  

```json
{
  "scripts": {
    "test": "cross-env NODE_OPTIONS=--experimental-vm-modules jest"
  },
  "type": "module"
}
```

## テスト実行

jestの拡張機能はうまく作動しなくなるので、コマンドで実行する

```
npm run test
```

## コード例

```js
// a.js
export const func = (a, b) => {
    return a + b
}

// a.test.js
import { func } from "./a";

test('足し算', () => {
    expect(func(1, 99)).toBe(100)
})
```

## experimental-vm-modulesの警告を消す方法

テスト実行時に必ず下の警告が表示される

```
(node:13852) ExperimentalWarning: VM Modules is an experimental feature and might change at any time
(Use `node --trace-warnings ...` to show where the warning was created)
```

消したいときは、`package.json`の`scripts`を編集する

```json
{
  "scripts": {
    "test": "cross-env NODE_OPTIONS=--experimental-vm-modules NODE_NO_WARNINGS=1 jest"
  }
}
```


## 参考URL

<https://zenn.dev/dozo/articles/0091f1a3e790d6>  

<https://stackoverflow.com/questions/55778283/how-to-disable-warnings-when-node-is-launched-via-a-global-shell-script>