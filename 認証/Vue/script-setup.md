# script setup

<https://ja.vuejs.org/api/sfc-script-setup>

Vue 3で導入されたComposition APIの糖衣構文

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="updateMessage">Click me</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// データの宣言
const message = ref('Hello, Vue 3!')

// メソッドの定義
function updateMessage() {
  message.value = 'You clicked the button!'
}
</script>

<style scoped>
/* コンポーネント固有のスタイル */
</style>

```

## 特徴

<https://tekrog.com/vue3-script-setup>

次の３つが不要になり、可読性が向上する

* export default defineComponentによるオブジェクトのラップ
* setup()関数
* return文

トップレベルに書いた変数や関数が、そのままtemplateで使える。  
さらに、Vue 3のパフォーマンス向上に合わせて設計されているため、ビルドサイズが小さくなり、速度も向上する。

## setup を使わないで書いた場合

```vue
<template>
  <div>
    <p>{{ message }}</p>
    <button @click="updateMessage">Click me</button>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const message = ref('Hello, Vue 3!')

    function updateMessage() {
      message.value = 'You clicked the button!'
    }

    return {
      message,
      updateMessage
    }
  }
}
</script>

<style scoped>
/* コンポーネント固有のスタイル */
</style>

```