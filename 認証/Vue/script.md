# script

コンポーネントのデータやロジック（JavaScriptのコード）を記述する。

コンポーネントで使うデータ（data）、メソッド（methods）、ライフサイクルフック（createdやmountedなど）、そしてpropsやcomputedなどを定義する。

```vue
<script>
export default {
  data() {
    return {
      message: 'こんにちは、Vue.js！'
    };
  }
};
</script>
```