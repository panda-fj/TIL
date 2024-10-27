# popupからcontentへのメッセージ

```ts
// popup.ts

/**
 * 2
 * 呼ばれた時点のアクティブなタブを取得する
 * @returns アクティブなタブ
 */
async function getActiveTab(): Promise<chrome.tabs.Tab> {
    return new Promise((resolve, reject) => {
        chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
            if (chrome.runtime.lastError) {
                return reject(chrome.runtime.lastError);
            }
            if (tabs[0]) {
                resolve(tabs[0]);
            } else {
                reject(new Error("No active tab found"));
            }
        });
    });
}

/**
 * 3
 * 指定したタブにメッセージを送信する  
 * （単体では使用せず、`sendMessageToContent()`から実行すること）  
 * @param tabId 対象タブのID
 * @param message 送信するメッセージ
 * @returns レスポンス
 */
async function sendMessageToTab(tabId: number, message: string): Promise<{ reply: string }> {
    return new Promise((resolve, reject) => {
        chrome.tabs.sendMessage(tabId, { message }, (response) => {
            if (chrome.runtime.lastError) {
                return reject(chrome.runtime.lastError);
            }
            resolve(response);
        });
    });
}

/**
 * 1
 * `content_script`で指定したJSファイル(例:`content.js`)にメッセージを送信する
 * @param message 送信するメッセージ
 */
async function sendMessageToContent(message: string): Promise<void> {
    // タブを検出して、そのタブにある`content_script`を探す
    try {
        const activeTab = await getActiveTab();
        const response = await sendMessageToTab(activeTab.id!, message);
        console.log(response.reply);
    } catch (error) {
        console.error('Error:', error);
    }
};
```

```ts
// content
/**
 * `background.js``popup.html`からメッセージを受け取る
 */
chrome.runtime.onMessage.addListener(async function(request: any, _sender: chrome.runtime.MessageSender, sendResponse: (response: any) => void) {
    // storage内のuserリストが変化したとき
    if (request.message === "updated_userdata") {
        observedUserList = await getDataFromStorage('xObservedUsers') as string[];
        await processPosts();
    }
    // popupのチェックボックスの値が変動した時
    if (request.message === "changed_checkbox_state") {
        isObserve = await getDataFromStorage('xPopupCheckboxState') as boolean;
        await processPosts();
    }
    sendResponse({ reply: "Content script received the message!" });
});
```