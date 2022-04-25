# SQL study notes

## Basic data manipulation with SQL
- SQL clause order: **SELECT --> FROM --> WHERE --> GROUP BY --> HAVING --> ORDER BY**
- SQL evaluates these clauses in the order: FROM, WHERE, GROUP BY, HAVING, and finally, SELECT. Therefore, each clause receives the filtered results of the previous filter. It would look like this: `SELECT(HAVING(GROUP BY(WHERE(FROM...))))`
- Create new/modify existing variables
  - String functions
    - `UPPER()` & `LOWER()`
    - `REPLACE(var, pattern, replacement)`
    - `CONCAT(var1, connection symbol, var2)`
    - `SUBSTR(var, start_index, length)`: Oracle
    - `SUBSTRING(var, start_index, length)`: MySql
    - `LEN()`
    - `LTRIM()`, `RTRIM()`, & `TRIM()`: removing empty space in leading, trailing or both
    - `TRIM(leading/trailing/both, char to be trimed FROM var)`: to remove specific char from a variable
    - `LEFT(str, length)`, `RIGHT()`: selecte certain length of string from left/right
    - `POSITION(substring IN string)`: return a numeric value, which is the index counted from left where the substring appears first in the string
    - ```sql
      CASE WHEN ... THEN ... 
      ELSE ... 
      END AS new_var_name
      ```
   - Date functions
     -  `DATEDD`: add one year to an existing date
     -  `TO_DATE`: convertrs a string into date
     -  `DATEDIFF`: find the difference b/t two given dates
     -  `DATEPART`: get year, month, or date from the date variable
     -  `DAY`: get day of the month for the given date
     -  `CURRENT_TIMESTAMP`: get the date and time (time stamp)  
- Using aggregate functions (COUNT, AVG, SUM, MIN, MAX)
  - usually used with `GROUP BY`
  - NULL value is elimiated in all thse functions, except for `COUNT(*)` (`COUNT(var)` still excludes NULL records)
- Apply conditions with `WHERE`
  - use `BETWEEN ... AND... ` to select obs with value with a range of a variable (inclusive)
  - Using regrular expression with `LIKE`
    - %: any string of 0 or more character
    - _(under score): any single character; 
    - []: any single character with the specificed range e.g. [a-f] <=> [abcdef]
    - [^]: any single character not within the specified range
- Apply conditions using aggregated varaible/value with `HAVING`

- Converting data types
  
  -  `CAST(column_name AS integer)` or `column_name::integer`: convert to integer  

-----
## Advanced SQL tips
- Joining tables
  
  ![c5831be5.png](https://raw.githubusercontent.com/askming/picgo/master/c5831be5_20200625134008.png)

  - Filtering with "join" operations: 
    - using "AND" after "ON" clause: filtering happens **before** joining
    - using "WHERE" after "ON" caluse: filtering happens **after** joining

  - Another way to visualize SQL joins: [You Should Use This to Visualize SQL Joins Instead of Venn Diagrams](https://towardsdatascience.com/you-should-use-this-to-visualize-sql-joins-instead-of-venn-diagrams-ede15f9583fc)
  
    ![NtZh3N_2022_04_24](https://cdn.jsdelivr.net/gh/askming/upic@master/uPic/NtZh3N_2022_04_24.jpg)
  
- UNION operator: to stack one dataset on top of the other
  ```sql
  SELECT var1, var2, var3 as var3_new
  FROM table1
  UNION
  SELECT var1, var2, var4 as var3_new
  FROM table2
  ```
  - If there are same rows from two tables, only one unique row will be shown; or to use `UNION ALL` to keep duplicates; The opposite is `UNION DISTINCT`
  - Two tables must have same # of cols
  - Columns must have same data types in the same order
-  "Cross join": performs cross product b/t 2 tables. connects each row in the left table with each row in the right table
- Create a **view** of the data table[^Socratica]: security of the data, giving access to only the variables included in the view table
  ```sql
  CREATE VIEW table_name AS
  SELECT var1, var2, var3
  FROM table;
  ```
- **Subquery**: innter query/nested query: to perform operation in several steps
  - subquery need to have an alias
  - EXAMPLE CODE NEEDED
- **Window functions**: performs a calculation across a set of table rows that are somehow related to the current row
  - Examples
    ```sql
    Aggregate_fun(var) OVER
      (PARTITION BY var1 ORDER BY var2
       ORDER BY var3
       ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS new_Var
    ```
  - This doesn't cause rows to be grouped into a single output row as done in the aggreated functions

  - Three (optional) **components**:
    - PARTITION BY: divides the rows of the table into different groups
    - ORDER BY: defines an ordering with each partition.
    - The final (window) clause: defining the **window frame**: defines the set of rows used in each calculation. e.g.
      - `ROWS BETWEEN 1 PRECEDING AND CURRENT ROW` - the previous row and the current row.
      - `ROWS BETWEEN 3 PRECEDING AND 1 FOLLOWING` - the 3 previous rows, the current row, and the following row.
      - `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` - all rows in the partition.
    
   - Three types of **analytic functions**[^kaggle]
      1. Analytic aggretate functions
      2. Analytic navigation functions
          - `FIRST_VALUE()`/`LAST_VALUE()`: returns the first/last value in the input 
          - `NTILE()`: determine the percentiles of a give value in a variable
          - `LAG() & LEAD()`: `LAG` pulls record(s) from previous and `LEAD` pulls from following rows
      3. Analytic numbering functions   
          - `ROW_NUMBER()`: show row # across the OVER & ORDER BY var
          - `RANK()`: similar to `ROW_NUMBER()` but will give same rank if value for ORDER BY variable is the same
          - `DENSE_RANK()`: differently from `RANK()`, it won't skip rank number if there are more than one obs. share the same rank

  - Defining a window alias: a convenient way to use several window functions that use the same window:
    ```sql
    WINDOW window_name AS
      (PARTITION BY var1 ORDER BY var2)
    ```
    this should come after `WHERE` clause
  - Can't include a window function in a GROUP BY clause

- Nested data
  - SQL data can also include a column with multiple fields in it; those fields are nested inside of this column and the nested column has type `STRUCT` (or type `RECORD`)
  - To query a column with nested data, we need to identify each field in the context of the column that contains it:
    - `C.Name`: referes to the `Name` field in the `C` column.

- Repeated data
  - When an ID contains multiple records in another table, and we want to put the multiple records into one column and we say this conlumn contains repeated data.
  - Each entry in a repeated field is an ARRAY, or an ordered list of (zero or more) values with the same datatype.
  - When querying repeated data, we need to put the name of the column containing the repeated data inside an `UNNEST()` function. `UNNEST(Column) AS var_name`; This essentially flattens the repeated data (which is then appended to the right side of the table) so that we have one element on each row.

- Nested and repeated data
  - A combination of nest and repeat in a column
  - When reading the values, we need UNNEST and use `C.Name` to read individual field

- Pivoting data in SQL (TBD)

### Resrouces to practice SQL
#### [Practice SQL](https://www.sql-practice.com/)
#### [LeetCode SQL Summary](https://github.com/siqichen-usc/LeetCode-SQL-Summary)

-----
## Miscellaneous tips
- `IS NULL`: check if it's missing value 
- `JOIN` is equivalent to `INNTER JOIN`
- `COALESCE()`: select teh first non NULL value in a list
- Use `EXPLAIN` at the beginning of the program, which will roughly show the complex of the program
- Performance tuning SQL queries
  - Table size: try to use `LIMIT XX` to limit the lines to show
  - Joins: make joins less complicated
- We refer to the structure of a table as its schema (something like `str()` in R). 

[^Socratica]: from [Socratica|YouTube](https://www.youtube.com/channel/UCW6TXMZ5Pq6yL6_k5NZ2e0Q)
[^kaggle]: from [Advanced SQL \| Kaggle](https://www.kaggle.com/learn/advanced-sql)

