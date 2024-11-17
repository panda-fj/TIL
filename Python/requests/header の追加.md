# リクエスト header の追加

<https://docs.python-requests.org/en/latest/user/quickstart/#custom-headers>

```py
# 例
import requests

url = "https://example.com/api"
headers = {
    "Authorization": "Bearer your_token_here",
    "Content-Type": "application/json",
    "Accept": "application/json",
}

response = requests.get(url, headers=headers)
print(response.status_code)
print(response.json())
```

## ヘッダーの種類

リクエストヘッダーは HTTP プロトコルの仕様に準拠

<https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers>

## User-Agent

```py
headers = {
    "User-Agent": "MyCustomClient/1.0",
}
```

UAを明示的に設定しない場合、デフォルトのUser-Agent（例：`python-requests/x.x.x`）が送信される。

