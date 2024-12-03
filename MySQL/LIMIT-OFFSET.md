# LIMIT-OFFSET

MySQLでクエリの結果セットの一部だけを取得したい場合に使用する。

ページネーション（検索結果や一覧の表示をページごとに分割すること）などで使われる。

```sql
SELECT カラム名
FROM テーブル名
LIMIT 取得する行数 OFFSET スキップする行数;
```

`OFFSET` を省略することもできる。

```sql
SELECT カラム名
FROM テーブル名
LIMIT スキップする行数, 取得する行数;
```

`LIMIT`

* 取得する行数を指定
* 必須

`OFFSET`

* セットの先頭からスキップする行数を指定する。
* 任意（デフォルトはスキップをしない 0 が適用）

## ページネーション

例えば、1ページに10件ずつデータを表示したい場合、OFFSETを計算して使う。

```sql
SELECT * 
FROM products 
LIMIT 10 OFFSET (ページ番号 - 1) * 10;
```

3ページ目のデータを取得する場合、ページ番号は3なので、`OFFSETは(3 - 1) * 10 = 20` となる。

```sql
SELECT * 
FROM products 
LIMIT 10 OFFSET 20;
```
