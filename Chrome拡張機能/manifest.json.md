# manifest.json

```json
{
    "manifest_version": 3, // とりあえず3じゃないとダメ
    "name": "name",  // 拡張機能の名前
    "version": "1.0.0.0",

    // 拡張機能ボタン押したとき出てくるポップアップ
    "action": {
        "default_popup": "popup/popup.html",
        "default_icon": {
            "16": "icons/icon16.png",
            "48": "icons/icon48.png",
            "128": "icons/icon128.png"
        }
    },

    // ブラウザの機能使うときはここで許可が必要
    "permissions": [
        "activeTab",
        "tabs",
        "scripting",
        "storage"
    ],

    // DOMを使わないやりとりとか
    "background": {
        "service_worker": "background.js"
    },

    // DOMを使うやりとり
    "content_scripts": [
        {
            // 適用するサイトの条件
            "matches": ["https://x.com/*"],
            // matchesに適するサイトで実行するjsファイル
            "js": [
                "content.js"
            ]
        }
    ]
}
```