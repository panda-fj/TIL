# USING

主にJOIN句で使用される構文で、複数のテーブルを結合する際に共通のカラムを指定するために使われる。

```sql
SELECT カラム名
FROM テーブル1
JOIN テーブル2
USING (共通カラム);
```

## 例

`employees`

|employee_id|name|department_id|
|:----|:----|:----|
|1|Alice|101|
|2|Bob|102|

`departments`

|department_id|department_name|
|:----|:----|
|101|HR|
|102|IT|

`SQL`

共通カラムは`department_id`で、完全に名前が一致しているため`USING`を使うことができる。

```sql
SELECT name, department_name
FROM employees
JOIN departments
USING (department_id);
```

結果セット

|name|department_name|
|:----|:----|
|Alice|HR|
|Bob|IT|

## USINGを使った場合の仮想テーブルの結果セット

`USING`句を使って、統一された列の名前は、テーブル名から指定でき、元の列名のみで指定することもできる。

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

SQL

```sql
SELECT employees.name, id, departments.name AS department_name
FROM employees
JOIN departments USING (id);
```

仮想テーブルのイメージ

`USING`によって`employees.id`と`departments.id`が統一され、その値は元のテーブル名からアクセスすることも、テーブル名を含まない値で指定することもできる。

|employees.name|id (employees.id, departments.id)|departments.name|
|:----|:----|:----|
|Alice|10|HR|
|Bob|20|Engineering|
