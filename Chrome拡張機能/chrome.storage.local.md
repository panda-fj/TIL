# chrome.storage.local

データを保存できる

## データ取得

```js
function getData() {
    return new Promise((resolve, reject) => {
        chrome.storage.local.get('myData', (result) => {
            if (chrome.runtime.lastError) {
                reject(chrome.runtime.lastError);
            } else {
                resolve(result.myData);
            }
        });
    });
}

// 例
getData()
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.error('Error:', error);
    });

// 例
async function fetchData() {
    try {
        const data = await getData();
        console.log(data);
    } catch (error) {
        console.error('Error:', error);
    }
}
fetchData();
```

## データ設定

```js
function setData(data) {
    return new Promise((resolve, reject) => {
        chrome.storage.local.set({ myData: data }, () => {
            if (chrome.runtime.lastError) {
                reject(chrome.runtime.lastError);
            } else {
                resolve();
            }
        });
    });
}
```

## データ変更検知

```js
chrome.storage.onChanged.addListener((changes, areaName) => {
    console.log("Storage area changed:", areaName);
    console.log("Changes:", changes);

    // changesオブジェクトの処理例
    for (let [key, { oldValue, newValue }] of Object.entries(changes)) {
        console.log(`Key: ${key}, Old Value: ${oldValue}, New Value: ${newValue}`);
    }
});
```

* `changes`: 変更されたキーとその前後の値を含むオブジェクト。プロパティとして、変更されたキー名が格納され、その値として以下のようなオブジェクトが格納される  
    `oldValue`: 変更前の値  
    `newValue`: 変更後の値  

* `areaName`: どのストレージ領域が変更されたかを示す文字列で、`local`か`sync`のどちらかかが入る。

```js
// 特定のキーを監視したいとき
chrome.storage.onChanged.addListener((changes, areaName) => {
    if (areaName === "local" && changes.myKey) {
        const oldValue = changes.myKey.oldValue;
        const newValue = changes.myKey.newValue;
        console.log(`Key 'myKey' changed from ${oldValue} to ${newValue}`);
    }
});
```

## 一般的なローカルストレージ（`window.localStorage`）との違い

### データフォーマット

* `window.localStorage`は、文字列のみ保存可能で、配列やオブジェクトを保存するときは、JSON形式に変換して文字列として保存、取り出すときには再度JSON形式にパースする。

    ```js
    localStorage.setItem('myData', JSON.stringify([1, 2, 3]));
    let data = JSON.parse(localStorage.getItem('myData'));
    ```
  
* `chrome.storage.local`は、配列やオブジェクトなど、JSON互換のデータ形式をそのまま保存できる。
  
    ```js
    chrome.storage.local.set({ myData: [1, 2, 3] });
    chrome.storage.local.get('myData', (result) => {
        let data = result.myData;
    });
    ```

### 非同期 / 同期

* `window.localStorage`は、同期的にデータを保存および取得する。保存や取得がすぐに完了し、結果が即時に返されるが、大量のデータ操作がある場合にはブロッキングが発生する可能性がある。
  
* `chrome.storage.local`は、非同期で動作する。データの保存や取得はコールバック関数またはPromiseを通じて行われ、大量のデータがあっても、メインスレッドをブロックしない。
  
    ```js
    chrome.storage.local.get(['key'], (result) => {
        console.log(result.key);
    });
    ```

### イベントリスナー

* `window.localStorage`は、別のタブでデータが変更されたときに反応するためのstorageイベントをサポートしているが、リスナーを追加できるのはデータ変更が発生したタブと異なるタブのみ。
  
* `chrome.storage.local`は、`chrome.storage.onChanged`イベントを使うことで、同じタブ内でも変更が監視でき、変更があるたびにイベントが発生する。