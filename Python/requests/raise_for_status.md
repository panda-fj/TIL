# raise_for_status()

HTTPステータスコードがエラー（4xxまたは5xx）の場合に、`requests.exceptions.HTTPError` を投げる

```py
import requests

response = requests.get("https://example.com/some-endpoint")
response.raise_for_status()  # エラーが発生すれば例外がスローされる
```

## raise_for_status() を使用しない場合

レスポンスがエラー（4xxや5xx）でも例外は発生せず、ステータスコードやレスポンス内容を自分で確認する必要がある。

状況に応じてエラー対応が可能になるため、特定のエラーを無視したりカスタム処理を設定できる。

```py
import requests

response = requests.get("https://example.com/some-endpoint")
if response.status_code >= 400:
    print(f"Error occurred: {response.status_code}")
```