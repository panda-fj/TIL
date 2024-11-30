# FROM-JOIN-ON

```sql
FROM table1 t1 JOIN table2 t2 ON t1.id = t2.id 
```

`FROM` メインテーブル  
`JOIN` 結合する別のテーブル
`ON` 結合条件、どの行が結び付くかを決定する

## テーブルの左右

`FROM` で指定したテーブルが左、 `JOIN` で指定したテーブルが右になる。

### LEFT JOIN

「左側のテーブル」のすべての行が結果セットに含まれる。  
結合条件（ON）が満たされない場合、右側のテーブルのカラムは NULL になる。

### RIGHT JOIN

「右側のテーブル」のすべての行が結果セットに含まれる。  
結合条件（ON）が満たされない場合、左側のテーブルのカラムは NULL になる。

## 結合するテーブルが3つ以上の場合

### 順番

* 原則、JOINは2つのテーブル間で1つずつ結合していく。  
* 各JOINは、それまでに結合された結果（左側）と、新たに結合するテーブル（右側）との間で行われる。
* JOINの順番がそのまま結合の処理順序に対応する。

```sql
SELECT customers.name, orders.amount, payments.status
FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id
LEFT JOIN payments ON orders.id = payments.order_id;
```

`FROM customers`: `customers` テーブルを「左端」として開始。  

`LEFT JOIN orders`: `customers` と `orders` を結合する。`customers` の全ての行を保持し、`orders` に関連するデータを結合（関連がない場合は NULL）。

`LEFT JOIN payments`: ここで結合対象となるのは、1番目の結合結果（customers + orders）と `payments` を結合する。

