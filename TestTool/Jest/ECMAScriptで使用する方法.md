# ECMAScriptで使用する方法

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