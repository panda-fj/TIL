# npx

<https://github.com/npm/npx>  
<https://docs.npmjs.com/cli/v9/commands/npx>  
<https://blog.npmjs.org/post/162869356040/introducing-npx-an-npm-package-runner>

## npxの仕組み

1. プロジェクトのローカル、$PATHを探して、パッケージがあれば実行する
2. ローカルにない場合は、`_npx`にインストールして実行する

## インストール先

windows   

```
C:\Users\<USERNAME>\AppData\Local\npm-cache\_npx
```
