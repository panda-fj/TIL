# dict型

## 値の取得・設定

```py
data["key_name"]
```

## キー削除

```py
data = {"a": 1, "b": 2, "c": 3}

# 削除したいキー
key_to_remove = "b"

# キーを削除
if key_to_remove in data:
    del data[key_to_remove]

# {'a': 1, 'c': 3}
```

```py
data = {"a": 1, "b": 2, "c": 3, "d": 4}

# 削除したいキーのリスト
keys_to_remove = ["b", "d"]

# キーを削除
data = {k: v for k, v in data.items() if k not in keys_to_remove}

# {'a': 1, 'c': 3}
```

### 特定の値を持つキーの削除

```py
data = {"a": 1, "b": 2, "c": 3}

# 削除したい値
value_to_remove = 2

# 値を削除 (該当するキーのみ)
data = {k: v for k, v in data.items() if v != value_to_remove}

# {'a': 1, 'c': 3}
```