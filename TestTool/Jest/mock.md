# mock

`jest.mock()`はモジュールすべてをモック化して制御する

## コード例（JS/CMS）

`a.js`

```js
class TestClass {
    constructor(num) {
        this.num = num
    }

    calc(numA, numB) {
        return numA + numB + this.num
    }
}
```