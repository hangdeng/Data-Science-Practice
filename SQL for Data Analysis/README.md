### SQL for Data Analysis

#### 1 Basic SQL operations

`SELECT (*)`: `*` means select all columns. Use `AS` to calculate a new column from existing columns (e.g. `SELECT c/(a+b) AS new_name`)

`FROM`: from the dataset for analysis use

`ORDER BY`: order by column name, default is ascending, if not using `DESC` 

`WHERE`:
  - use `WHERE` to select data under condition (e.g. `>=`, `>`, `<=`, `<`, `=`, `!=`). 
  - `BETWEEN` and `AND` can be used in `WHERE` for certain condition (e.g. `WHERE columnA>=a AND columnB<=b`, `WHERE columnA BETWEEN a_1 AND a_2`).
  - `OR`: `OR` is used to select any row which has a column value match either sub-condition of the condition.
  - `LIKE` and `NOT LIKE` can be used to select similar names in `WHERE`: `WHERE name LIKE 'C%'` means any name starting with letter 'C' is selected. `WHERE name NOT LIKE '%s'` means any name ending with 's' is not selected. `AND` can be used for multiple conditions. `WHERE name LIKE '%as%'` means any name containing 'as' is selected. 
  - `IN`: Select column values within a range or is exactly any of several selected values (e.g. `WHERE name_c IN ('name_c1','name_c2')`).

`LIMIT`: only display a number of rows, for example, `LIMIT 10` only display first 10 rows

#### 2 SQL joins


