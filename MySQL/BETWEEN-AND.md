# BETWEEN AND

指定された開始値と終了値の間にあるかを判定する。範囲は境界値を含む。

```sql
SELECT column_name
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

BETWEEN は境界値も含むため、`value1 <= column_name <= value2` と同じ意味を持つ。

## 例

### 数値

```sql
SELECT * 
FROM employees 
WHERE salary BETWEEN 3000 AND 7000;
```

### 日付

```sql
SELECT * 
FROM orders 
WHERE order_date BETWEEN '2023-01-01' AND '2023-12-31';
```

### 文字列

```sql
SELECT * 
FROM products 
WHERE product_name BETWEEN 'A' AND 'M';
```

## NOT BETWEEN

BETWEEN の反対条件を指定する場合は、NOT を付ける。

```sql
SELECT * 
FROM employees 
WHERE salary NOT BETWEEN 3000 AND 7000;
```
