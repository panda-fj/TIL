# mock

`jest.mock()`はモジュールすべてをモック化して制御する

## コード例（JS/CMS）

```js
// a.js
class TestClass {
    constructor(num) {
        this.num = num
    }

    calc(numA, numB) {
        return numA + numB + this.num
    }
}
```

```js
// a.test.js
const TestClass = require("./a")
jest.mock("./a", () => {
    return jest.fn().mockImplementation(() => {
        return {
            calc: jest.fn().mockReturnValue(20)
        };
    });
});

test("" ,() => {
    a = new TestClass(10);
    expect(TestClass).toHaveBeenCalled(); // pass
    expect(a.calc(1, 5)).toBe(16);
    // failed (Expected: 16 Received: 20)
})
```