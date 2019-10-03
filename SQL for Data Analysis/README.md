### SQL for Data Analysis

*Note summarized by [hangdeng](https://www.linkedin.com/in/hangdeng?trk=public_profile_browsemap_mini-profile_title) for a quick review.*

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
