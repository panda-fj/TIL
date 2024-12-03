# JOINの結合結果

JOINの結果は「結合された仮想的な大きなテーブル」として扱われる。

実際には、大きなテーブルは、各元テーブルの列が並べられた形になる。

## JOIN-ONの結果セット

`employees`

|id|name|department_id|
|:----|:----|:----|
|1|Alice|10|
|2|Bob|20|

`departments`

|id|name|
|:----|:----|
|10|HR|
|20|Engineering|

`SQL`

```sql
SELECT *
FROM employees
JOIN departments ON employees.department_id = departments.id;
```

結果セット

|employees.id|employees.name|employees.department_id|departments.id|departments.name|
|:----|:----|:----|:----|:----|
|1|Alice|10|10|HR|
|2|Bob|20|20|Engineering|


