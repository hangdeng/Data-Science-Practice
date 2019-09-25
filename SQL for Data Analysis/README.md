#### SQL for Data Analysis
`SELECT (*)`: `*` means select all columns. Use `AS` to calculate a new column from existing columns (e.g. `SELECT c/(a+b) AS new_name`)

`FROM`: from the dataset for analysis use

`ORDER BY`: order by column name, default is ascending, if not using `DESC` 

`WHERE`: use where to select data under condition (e.g. `>=`, `>`, `<=`, `<`, `=`, `!=`). `BETWEEN` and `AND` can be used in `WHERE` for certain condition (e.g. `WHERE columnA>=a and columnB<=b`, `WHERE columnA BETWEEN a_1 AND a_2`)

`LIMIT`: only display a number of rows, for example, `LIMIT 10` only display first 10 rows

