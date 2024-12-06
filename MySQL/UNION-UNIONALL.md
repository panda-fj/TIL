# UNION, UNION ALL

複数のクエリ結果を、縦方向に結合して、1つの結果セットとしてまとめる。

## 例

```sql
SELECT name FROM customers
UNION
SELECT name FROM suppliers;
```

この場合、`customers` と `suppliers` という2つのテーブルから名前を取得し、1つのリストにまとめる。

## UNION と UNION ALLの違い

UNIONは重複を削除、UNION ALLは保持する。