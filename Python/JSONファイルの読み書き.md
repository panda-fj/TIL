# JSONファイル

## 読み込み

```py
with open(file_path, 'r', encoding='UTF-8') as f:
    return json.load(f)
```

## 書き込み

```py
# content は dict型
with open(file_path, 'w', encoding='UTF-8') as f:
    json.dump(content, f, ensure_ascii=False, indent=4) 
```

## アクセス競合を防ぐ

マルチスレッドで同時に同じファイルにアクセスすると、結果が得られない場合がある。

```py
import threading
lock = threading.Lock()

with lock:
    with open(file_path, 'r', encoding='UTF-8') as f:
        return json.load(f)
```

