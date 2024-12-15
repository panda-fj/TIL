# 親ディレクトリを起点にimportする

## 構造

```
root
├─dir1
│  │  file.py
```

## sys.pathに親ディレクトリを追加する

```py
import sys
from pathlib import Path
# 親ディレクトリを取得し、sys.pathに追加する
parent_directory = Path(__file__).resolve().parent.parent
sys.path.append(str(parent_directory))
```

その後の import は、rootを起点にすることができる。