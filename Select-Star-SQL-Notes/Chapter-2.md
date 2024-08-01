# SELECT STAR SQL CHAPTER 2: CLAIMS OF INNOCENCE-- NOTES
#### Chapter Takeaways:
1. Use aggregate functions to perform operations on multiple rows to get one number (summary values)
2. Use `COUNT` to count non-`NULL` rows in data
  - `COUNT(*)` can return all rows in table
  - Aggregate functions work with `WHERE`
  - `CASE WHEN` works as if-else statement-- don't forget the syntax though!
3. Once you understand the general flow of querying, you can rely on documentation for specific names of functions

## Table of Contents:
1. [NULL Values](#null-values)
2. [Aggregate Functions](#aggregate-functions)
3. [The COUNT Function](#the-count-function)
   - [Counting all rows](#counting-all-rows-including-null-values)
   - [Basic conditionals](#basic-conditionals)
   - [Multi-conditionals -- CASE WHEN Blocks](#multi-conditionals---case-when-blocks)
4. [Looking Up Functions](#looking-up-functions)
5. [Practice](#practice)
6. [Chapter 2 Project](#chatper-2-project)
---
## NULL Values
**Main Takeaway:** `NULL` means a cell is completely empty
- `NULL` represents the value of an *empty* entry
  - In this case empty means *nothing* was entered in the field
    - An empty string "" and the integer 0 are *not* considered `NULL`
- To check if an entry is `NULL` we use `IS` and `IS NOT` rather than `=` and `!=`
---
## Aggregate Functions
**Main Takeaway:** Use aggregate functions to perform calculations on multiple rows to get one number
- Aggregate functions take multiple rows of data and combine them into one number
  - Useful when trying to calculate proportions (e.g. in book is proportion of executions that claimed to be innocent)
---
### The COUNT Function
**Main Takeaway:** Use `COUNT` to find non-`NULL` rows in data and use `WHERE` and `CASE WHEN` for simple and more complex conditionals
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
## Looking Up Functions
**Main Takeaway:** Learn the logic behind SQL so you can rely on documentation for the specific names of functions
- There is no need to memorize *every* SQL function
- Learn the way the computer executes SQL and the logic behind a query so you can look up *documentation* for specific syntx
- Useful resources:
  - [W3 Schools](https://www.w3schools.com/sql/default.asp)
  - StackOverflow
  - [Official SQLite Documentation](#https://sqlite.org/)
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

**2.Find the numver of inmates who have declined to give a last statement.**
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

- Using two `COUNT` functions:
```sql
SELECT 
	COUNT(*) - COUNT(last_statement)
FROM executions
```
| COUNT(*) - COUNT(last_statement) |
|----------------------------------|
| 110				   |

**Shows:**
- Different ways to get same result in SQL
- Can perform arithmetic operations between aggregation functions
- The first solution had computer filter down table then look whereas other two solutions had computer look through entire table *at least* once (twice for the third solution!)-- same output but different efficiencies

**3. Find the min, max, and avg age of inmates at the time of execution.**
```sql
SELECT
	MIN(ex_age),
	MAX(ex_age),
	AVG(ex_age)
FROM executions
```
| MIN(ex_age) |	MAX(ex_age) | AVG(ex_age) |
|-------------|-------------|-------------|
| 24	      | 67 	    | 39.47       |

**Shows:**
- Other aggregation functions

**4. Find the average length (based on the character count) of last statements.**
```sql
SELECT
	AVG(LENGTH(last_statement))
FROM executions
```
| AVG(LENGTH(last_statement)) |
|-----------------------------|
| 537.49                      |

**Shows:**
- Using documentation and google to find required function
  - I looked up "sqLite find number of characters in a string"

**5. List all the counties in the data *without duplication***.
```sql
SELECT DISTINCT
	county
FROM executions
```
*Output too long to display*

**Shows:**
- Commonly used `SELECT DISTINCT` to get unique values of a column
---
## Chapter 2 Project
**Find the proportion of inmates with claims of innocence in their last statements.**
To do decimal division, ensure that one of the numbers is a decimal by multiplying it by 1.0. Use `LIKE '%innocent%'` to find claims of innocence.
- Note that we are looking for the proportion of *inmates* not *last_statements* so we want to count people who had no last statement (NULL) in the denominator of our proportion.

**Query:**
```sql
SELECT
	1.0 * COUNT(CASE WHEN last_statement LIKE '%innocent%'
		  THEN 1 ELSE NULL END)/
		  COUNT(*)
FROM executions
```
| 1.0 * COUNT(CASE WHEN last_statement LIKE '%innocent%' THEN 1 ELSE NULL END)/ COUNT(*) |
|----------------------------------------------------------------------------------------|
| 0.056										         |


	
