### SQL for Data Analysis

*Note summarized by [Hang_Deng](https://www.linkedin.com/in/hangdeng?trk=public_profile_browsemap_mini-profile_title) for a quick review.*

#### 1 Basic SQL operations

`SELECT (*)`: `*` means select all columns. Use `AS` to calculate a new column from existing columns (e.g. `SELECT c/(a+b) AS new_name`)

`FROM`: from the dataset for analysis use

`ORDER BY`: order by column name, default is ascending, if not using `DESC` 

`WHERE`:
  - use `WHERE` to select data under condition (e.g. `>=`, `>`, `<=`, `<`, `=`, `!=`). 
  - `BETWEEN` and `AND` can be used in `WHERE` for certain condition (e.g. `WHERE columnA>=a AND columnB<=b`, `WHERE columnA BETWEEN a_1 AND a_2`).
  - `OR`: `OR` is used to select any row which has a column value match either sub-condition of the condition.
  - `LIKE` and `NOT LIKE` can be used to select similar names in `WHERE`: `WHERE name LIKE 'C%'` means any name starting with letter 'C' is selected. `WHERE name NOT LIKE '%s'` means any name ending with 's' is not selected. `% S%` means to skip the first word and find any last name starting with S. `AND` can be used for multiple conditions. `WHERE name LIKE '%as%'` means any name containing 'as' is selected. 
  - `IN`: Select column values within a range or is exactly any of several selected values (e.g. `WHERE name_c IN ('name_c1','name_c2')`).
  - `IS NULL` or `IS NOT NULL`: uses to select null values or not null values from the data.
  
`LIMIT`: only display a number of rows, for example, `LIMIT 10` only display first 10 rows

#### 2 SQL Joins

`JOIN`: `JOIN` is the inner join by default. Venn diagram can be used for a better view. `LEFT JOIN` keeps everything in the left outer circle and only keep values in the inner circle once. `RIGHT JOIN` is similar to `LEFT JOIN`. `OUTER JOIN` joins everything but only keep the values in the inner circle once.
`SELECT DISTINCT`: `SELECT DISTINCT` narrows down the results to unique values. For example, there might be multiple rows with the same value, `SELECT DISTINCT` keep the same value only once. 
 
 **Primary Key vs. Foreign Key**: A primary key is the unique column in a particular table. It is usually the first column in the table. A     foreign key is one column in a table which is a primary key in another table.

#### 3 SQL Aggregations

`SUM`, `MIN`, `MAX`, `AVG`, `COUNT`, etc.: use as indicated.

`GROUP BY`:
  - use `HAVING` instead of `WHERE` for subset condition after aggregations.

`CASE`: uses as IF conditional statement after `SELECT`. For example, `SELECT CASE WHEN a>100 THEN 'name_a' WHEN b>50 THEN 'name_b' ELSE 'name_c' END AS column_name`

`DATE_PART` and `DATE_TRUNC`: these two functions are used for using a specific part of or truncating dates to for further analysis. For example, `DATE_PART('year', data.date)` with `GROUP_BY data.date` groups date with the same year. `DATE_TRUNC('day', data.date)` only leaves YYYY-MM-DD and replace the rest (e.g. hh:mm:ss) with the default value (i.e. 00:00:00).

#### 4 SQL Subqueries & Temporary Tables

`WITH`: `WITH` is used to create temporary tables. Further analysis can simply use the temporary tables to save time. For example, the code with `WITH` can be written as
```
WITH a AS (
       SELECT *
       FROM table1
       WHERE DATE_PART('year', table1.date) BETWEEN '2001' AND '2018')
     b AS (
       SELECT *
       FROM table2
       WHERE table2.qty >= 1000)
       
SELECT a.name, COUNT(a.item), SUM(b.qty)
FROM a
JOIN b
ON a.id = b.a_id
GROUP BY 1
ORDER BY 2 DESC;
```

#### 5 SQL Data Cleaning

`LEFT`: `LEFT` select characters in a column from left. For example, the code below select the left 2 characters in the name column:
```
SELECT LEFT(a.name, 2)
FROM accounts a
```

`RIGHT`: `RIGHT` is similar to `LEFT` but start to select from the right.

`LENGTH`: `LENGTH` count the length of each element in one column.

`POSITION` and `STRPOS`: These two functions are quite similar but the syntax is a little different (e.g. `POSITION(',', a.name)` and `STRPOS(a.name, ',')`)

`LOWER` and `UPPER`: make characters in the column all lower or uppercase.

`CONCAT` and `||`: This function concatenates characters in two or more columns (e.g. `CONCAT(a.first_name, '.', a.last_name, '@', a.company_name, '.com') email_address` or `a.first_name || '.' || a.last_name || '@' || a.company_name || '.com' email_address`).

`CAST` and `::`: This function changes the data type of one columne to another (e.g. `CAST(date_column AS DATE)` or `date_column::DATE`).

`COALESCE`: For example, a and b are columns. `COALESCE(a, b) name` returns a column 'name' that assigns value b if the value in the row of column a is NULL. Otherwise, the function assigns value a to the column 'name' if the row of column a is not NULL. The function evaluates from first expression (i.e. a in the example)  to the last expression (i.e. b in the example) in order.

#### 6 SQL Window Functions
`OVER` and `PARTITION BY`: The two functions are keys to window functions. Window functions are similar with aggregate functions but window functions keep separate entities in their rows comparing to aggregate functions that group entities in one row.
For example, the SQL code for calculating running total
```
SELECT standard_amt_usd, SUM(standard_amt_usd) OVER (ORDER BY o.occurred_at) AS running_total
FORM orders o
```

In addition, if `PARTITION BY` is applied, the result table separately display data by different groups of accounts_id keys, 

```
SELECT accounts_id, standard_amt_usd, SUM(standard_amt_usd) OVER (PARTITION BY accounts_id, ORDER BY occurred_at) AS running_total
FORM orders 
```

`RANK()` and `DENSE_RANK()`: `RANK()` ranks the order from the smallest to the largest (or from the largest to the smallest if `DESC` is included). If two values are the same, for example, `RANK` on ('a', 'a', 'b', 'c', 'd') returns 1, 1, 3, 4, 5. However, `DENSE-RANK` returns 1, 1, 2, 3, 4. 

`LAG` and `LEAD`: `LAG(b) AS a` returns a new column a which values are the values of b shift one row backward/upward (values of the previous rows). `LEAD(b) AS a` is similar but returns the values of b shift one row forward/downward (values of the following rows).
For example,
```
LEAD (standard_sum) OVER ( ORDER BY standard_sum) - standard_sum AS lead_difference
```
```
standard_sum - LAG (standard_sum) OVER ( ORDER BY standard_sum) AS lag_difference
```
compare the difference between the current row and the previous row.

`NTILE`: `NTILE()` returns a column assign the values of a specific column to their percentiles. For example, `NTILE(4)` follows `SELECT a` returns a column that specifies quartile # of each value of column a. `NTILE()` can be used with the window functions.


#### 7 SQL Advanced JOINs & Performance Tuning
`LIMIT` and `OFFSET`: For example, `LIMIT 10` only 10 rows will be returned. `OFFSET 10` skip 10 rows and start returning values from the 11th row. `OFFSET 0` doesn't skip any row.

`INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`, `FULL JOIN`: 
  - `INNER JOIN` joins matched rows between two tables. 
  - `LEFT JOIN` includes unmatched rows from the left table and matched rows between two tables. 
  - `RIGHT JOIN` includes unmatched rows from the right table and matched rows between two tables. 
  - `FULL JOIN` is the same as `FULL OUTER JOIN` which includes unmatched rows from both tables and matched rows from both tables. 

Inequal JOINs: inequal joins use conditions to join tables, for example,
```
SELECT *
FROM table_a
LEFT JOIN table_b
ON table_a.id = table_b.id
AND table_a.name < table_b.name
```

Self JOINs: Self JOINs with inequal JOINs is very common, for example,
```
SELECT *
FROM table_a a1
LEFT JOIN table_a a2
ON a1.id = a2.id
AND a1.occurred_at > a2.occurred_at
AND a1.occurred_at < a2.occurred_at + INTERVAL '28 days'
ORDER BY a1.occurred_at
```

##### Tips for enhancing performance:
Performance can be affected by table size, number of joins, and aggregations in SQL analysis.

  - joining subqueries can improve performance.
