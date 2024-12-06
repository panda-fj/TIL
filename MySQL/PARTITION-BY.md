# PARTITION BY

MySQL のウィンドウ関数で使用される。

```sql
<ウィンドウ関数>(列名) OVER (PARTITION BY 列名 [ORDER BY 列名])
```

* 指定した列でデータをグループ化して、各グループ内でウィンドウ関数が計算を実行する。

## 例1

`employees`

|department|employee_name|salary|
|:----|:----|:----|
|HR|Alice|5000|
|HR|Bob|6000|
|IT|Charlie|7000|
|IT|Dave|8000|

各部門内で給与の高い順に順位を付けるクエリ

```sql
SELECT 
    department,
    employee_name,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS salary_rank
FROM employees;
```

* `PARTITION BY department` は、各部門（HR, IT）でデータを分割する。
* `ORDER BY salary DESC` により、各部門で給与の高い順に順位を付ける。

結果セット

|department|employee_name|salary|salary_rank|
|:----|:----|:----|:----|
|HR|Bob|6000|1|
|HR|Alice|5000|2|
|IT|Dave|8000|1|
|IT|Charlie|7000|2|

## 例2

`sales`

|sale_id|region|amount|
|:----|:----|:----|
|1|East|100|
|2|East|150|
|3|West|200|
|4|West|50|

各地域ごとの累積売上を計算するクエリ

```sql
SELECT 
    region,
    amount,
    SUM(amount) OVER (PARTITION BY region ORDER BY sale_id) AS cumulative_sales
FROM sales;
```

* `SUM(amount)` は累積売上を計算する。
* `PARTITION BY region` は地域ごとに売上を累積する。
* `ORDER BY sale_id` は、販売 ID の順で累積を計算する。

結果テーブル

|region|amount|cumulative_sales|
|:----|:----|:----|
|East|100|100|
|East|150|250|
|West|200|200|
|West|50|250|