# contentからpopupへのメッセージ

* popup -> content
* content -> background -> popup

```ts
// content

function sendMessageToPopup(message: any): Promise<void> {
    // backgroundにメッセージを送信している
    return new Promise((resolve, _reject) => {
        chrome.runtime.sendMessage({ message: "message", data: message }, (response) => {
            console.log("Response from background:", response);
        });
        resolve();
    });
}
```

```ts
// background

// popupが開かれたら接続をキャッシュ
let popupPort: chrome.runtime.Port | null = null;
chrome.runtime.onConnect.addListener(function (port: chrome.runtime.Port) {
    if (port.name === "popup-channel") {
        popupPort = port;

        // 接続が閉じられたらキャッシュをクリア
        port.onDisconnect.addListener(function () {
            popupPort = null;
        });
    }
});

// contentからメッセージを受け取り、popupに送信する
chrome.runtime.onMessage.addListener(function (request: any, _sender: chrome.runtime.MessageSender, sendResponse: (response: any) => void) {
    // `request`: contentからのメッセージ
    if (request.message === "message") {
        console.log("Received from content script:", request.data);

        // popupが開いている場合のみデータを送る
        if (popupPort) {
            popupPort.postMessage({ data: request.data });
        }

        // contentに送る確認用メッセージ
        sendResponse({ reply: "Background received data" });
    }
});
```

```ts
// popup

// popupを開いた時点でbackgroundに接続
let port = chrome.runtime.connect({ name: "popup-channel" });

// backgroundからのメッセージをここで待機
port.onMessage.addListener(async function (message) {
    console.log("Received data in popup:", message.data);
    // message: backgroundからのデータ
    if (message.data == "updated_userdata") {
        // メッセージを受けて行う処理
        const newUserData = await getDataFromStorage('xObservedUsers') as string[];
        const addedUsers = newUserData.filter(item => !observedUserList.includes(item));
        // 差分が複数になることはない
        if (addedUsers.length >= 1) {
            addUserElement(addedUsers[0])
        }
        observedUserList = newUserData;
    }
})
```