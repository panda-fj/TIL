# DISTINCT

`SELECT` のあとに `DISTINCT` と続けると、重複を無くすことができる。

```sql
SELECT DISTINCT column
FROM mytable
WHERE condition;
```

## 複数の列を指定したとき

指定した複数の列の組み合わせがユニークであるかを基準に重複が除外される。

単独の列ではなく、指定されたすべての列をまとめて1つのキーと見なして判定する。

`SELECT DISTINCT col1, col2, col3` と指定すると、`col1, col2, col3` の全ての列の値の組み合わせが重複している行が除外される。

## NULL値

`NULL` は他の値とは異なるものとして扱われる。しかし、`NULL` 同士は同じとみなされる。

例

```sql
SELECT DISTINCT col1
FROM table_with_null;
```

`col1` に `NULL` が複数含まれている場合、`NULL` は1つだけ残る。