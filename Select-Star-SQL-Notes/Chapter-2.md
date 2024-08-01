# SELECT STAR SQL CHAPTER 2: CLAIMS OF INNOCENCE-- NOTES
#### Chapter Takeaways:


## Table of Contents:
1. [NULL Values](#null-values)
2. [Aggregate Functions](#aggregate-functions)
3. [The COUNT Function](#the-count-function)
   - [Counting all rows](#counting-all-rows-including-null-values)
   - [Basic conditionals](#basic-conditionals)
   - [Multi-conditionals -- CASE WHEN Blocks](#multi-conditionals---case-when-blocks)
4. [Practice](#practice)
---
## NULL Values
- `NULL` represents the value of an *empty* entry
  - In this case empty means *nothing* was entered in the field
    - An empty string "" and the integer 0 are *not* considered `NULL`
- To check if an entry is `NULL` we use `IS` and `IS NOT` rather than `=` and `!=`
## Aggregate Functions
- Aggregate functions take multiple rows of data and combine them into one number
  - Useful when trying to calculate proportions (e.g. in book is proportion of executions that claimed to be innocent)
---
### The COUNT Function
- Purpose: Counts (by default, non-`NULL`) rows in a specified column
- Format: `COUNT <column>`
- Make sure to put the specific column you want to count so `NULL` values are filtered out
#### Counting *all* rows (including `NULL` values)
- Use `COUNT(*)` which counts rows as long as *any one* of their columns is non-`NULL`
  - Useful for finding the length of a table -- no row should have `NULL` values for every column
#### Basic conditionals
- Can use `COUNT` with `WHERE`
  - E.g.: `SELECT COUNT(*) FROM executions WHERE county = 'Harris' will select all exectuions in Harris county
#### Multi-conditionals-- CASE WHEN Blocks
- If you wanted to filter by multiple conditions (like looking at counts for multiple counties) you can use a `CASE WHEN` *block*
- Purpose: Act as an if-else statement for aggregation
- Format:
  ```sql
  CASE
    WHEN <clause> THEN <clause>
    WHEN <clause> THEN <clause>
    ...
    ELSE <result>
  END
  ```
- Note that the `END` command and `ELSE` condition are both *required*
- Each `WHEN` clause must be boolean
  - E.g.: Count number of Harris and Bexar county executions 
    ```sql
    SELECT
      COUNT(CASE WHEN county = 'Harris' THEN 1
      ELSE NULL END)
      COUNT(CASE WHEN county = 'Bexar' THEN 1
      ELSE NULL END)
    FROM executions;
    ```
---
## Practice
**1. Find how many inmates were over the age of 50 at execution time.**
```sql
SELECT
	COUNT(ex_age)
FROM executions
WHERE ex_age > 50;
```
| COUNT(ex_age) |
|---------------|
| 68            |

**Shows:**
- `WHERE` block filters *before* the `COUNT` (or any) aggregation

**2.Find the numver of inmates who have declined to give a last statement**
- Using `WHERE`:
```sql
SELECT
	COUNT(*)
FROM executions
WHERE last_statement IS NULL;
```
| COUNT(*) |
|----------|
| 110      |

- Using `COUNT` and `CASE WHEN`:
```sql
SELECT
	COUNT(CASE WHEN last_statement IS NULL
		  THEN 1
		  ELSE NULL END)
FROM executions
```
| COUNT(CASE WHEN last_statement IS NULL THEN 1 ELSE NULL END) |
|--------------------------------------------------------------|
| 110                                                          |
