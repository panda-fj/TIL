# CommonJS

## コード例

```js
// module_text.js

class TextModule {
    exec(a, b) {
        return a + b;
    }
}

module.exports = TextModule
```

```js
// module_calc.js

class CalcModule {
    constructor(x) {}
    exec(a, b) {
        return a + b - this.x
    }
}

module.exports = CalcModule
```

```js
// module_test.js

const CalcModule = require("./module_calc")
const TextModule = require("./module_text")


class TestClass {
    constructor(text) {
        this.calc = new CalcModule(1)
    }

    getText(textA, textB) {
        const s1 = textA.split("s");
        const s2 = textB.substr(-2);
        const c = this.calc.exec(textA.length, textB.length);
        const t = this.text.exec(s1[0], s2);
        return t + s1[s1.length - 1] + c
    }
}

module.exports = TestClass
```

## テスト

```js
// module.test.js

const TestClass = require("./module_test")

test("testA", () => {
    const instance = new TestClass();
    expect(TestClass).toHaveBeenCalled();
    expect(instance.method).toBeDefined();
    expect(instance.method()).toBe(1);
})
```

### モック

```js
// module.test.js

// requireでインポートしたいモジュールのパスと合わせる
jest.mock("./module_sub", () => {
    return jest.fn().mockImplementation(() => {
        return {
            exec: jest.fn(),
        };
    });
});

```