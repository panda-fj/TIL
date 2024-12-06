# HAVING

グループ化したデータに条件を適用するために使用される。

`GROUP BY`句と組み合わせて使用されることが多く、集計関数の結果をフィルタリングしたいときに便利。

```sql
SELECT カラム名, 集計関数(カラム名)
FROM テーブル名
GROUP BY カラム名
HAVING 条件;
```

## HAVING と WHERE の違い

* WHERE: グループ化の前に行データに対して条件を適用。
* HAVING: グループ化の後にグループ化されたデータに対して条件を適用。

`HAVING`は、集計関数やグループ化の結果に条件を適用する唯一の手段であるため、`GROUP BY`と組み合わせて使われるのが一般的。

`GROUP BY`を使わない場合、`HAVING`は役割を果たせる状況がほぼない。
集計を行わない場合、データをフィルタリングするには`WHERE`で十分。

* フィルタリングが集計前に可能であれば、可能な限り`WHERE`を使用する。
* フィルタリングが集計結果に基づく場合は`HAVING`を使用する。

## 例

`sales`

|sale_id|product_id|store_id|sale_amount|sale_date|
|:----|:----|:----|:----|:----|
|1|101|1|500|2024-11-01|
|2|102|1|1200|2024-11-01|
|3|101|2|300|2024-11-02|
|4|103|1|700|2024-11-02|
|5|101|1|400|2024-11-03|
|6|102|2|900|2024-11-03|
|7|103|2|1000|2024-11-04|
|8|101|1|200|2024-11-04|

### 商品ごとの売上合計が1500以上、かつ平均売上金額が400以上の商品

```sql
SELECT product_id, SUM(sale_amount) AS total_sales, AVG(sale_amount) AS average_sales
FROM sales
GROUP BY product_id
HAVING SUM(sale_amount) >= 1500 AND AVG(sale_amount) >= 400;
```

結果

|product_id|total_sales|average_sales|
|:----|:----|:----|
|102|2100|1050|
|103|1700|850|
