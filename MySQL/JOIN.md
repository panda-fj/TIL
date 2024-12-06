# JOIN

## INNER JOIN

結合条件に一致する行だけを取得する。

## LEFT JOIN (LEFT OUTER JOIN)

左側のテーブルの全データを取得し、右側テーブルに関連するデータがない場合は NULL が表示される。

```sql
SELECT customers.name, orders.amount
FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id;
```

* 全ての customers が取得される。
* 該当する orders がない場合は、orders.amount は NULL になる。
  
## RIGHT JOIN (RIGHT OUTER JOIN)

右側のテーブルの全データを取得し、左側テーブルに関連するデータがない場合は NULL が表示される。

## CROSS JOIN

すべての組み合わせ（直積）を取得する。条件を指定しないと膨大なデータ量になる。