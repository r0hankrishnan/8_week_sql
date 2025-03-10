# SELECT STAR SQL CHAPTER 3: THE LONG TAIL-- NOTES
#### [Chapter 3](https://selectstarsql.com/longtail.html) Takeaways:
   1. Use `GROUP BY` to aggregate by columns
      - Only the specific columns that you group by can be unaggregated
      - Columns should be in the same order in `SELECT` and `GROUP BY` statements
   2. Use `HAVING` to filter after grouping by columns
      - Function is performed **after** `WHERE` block
   3. Use nested queries to perform calculations that require individual row values **AND** overall totals
      - Syntax involves including nested query in parentheses
      - Can be used in more than just row-level calculations (see percentage example)
        - See length example for `WHERE` nested query

## Table of Contents:
1. [The GROUP BY Block](#the-group-by-block)
2. [The HAVING Block](#the-having-block)
3. [Nested Queries](#nested-queries)
4. [Practice](#Practice)
---

## The GROUP BY Block
- Split up data into groups and perform aggregations that result in one row **per group**
  - Helpful when we need to perform aggregations over group instances of our rows instead over the whole table
  - Basic syntax:
     - `GROUP BY <column>, <column>, ...` after the `WHERE` block

**Example:**
```sql
SELECT
   county,
   COUNT(*) AS county_executions
FROM executions
GROUP BY county
```

- Why can we mix non aggregated columns (county) with aggregated columns (county_executions)?
  - With `GROUP BY`, **only** the grouped columns are allowed to be non-aggregate
  - **If you group multiple columns, they must be in the same order in the SELECT statement as they are in the `GROUP BY` statement**
- Notice also the use of `AS`, which is called **aliasing**
  - Whatever you refer to your expression with using `AS` can be referenced later in the query, allowing us to avoiding rewriting long expressions

**Output (truncated to top 5):**
| county | county_executions |
|--------|-------------------|
| Anderson | 4 |
| Aransas | 1 |
| Atascosa | 1 |
| Bailey | 1 |
| Bastrop | 1 |
| ... | ... |

**Practice:**
Modify the query to count executions with and without last staements by county.

*Original*
```sql
SELECT
   last_statement IS NOT NULL AS has_last_statement,
   COUNT(*)
FROM executions
GROUP BY has_last_statement
```

*Solution*
```sql
SELECT
   county,
   last_statement IS NOT NULL AS has_last statement,
   COUNT(*)
FROM executions
GROUP BY has_last_statement, county
```

**Output:**
| has_last_statement | county       | COUNT(*) |
|--------------------|--------------|----------|
| 0                  | Bell         | 1        |
| 0                  | Bexar        | 9        |
| 0                  | Brazoria     | 1        |
| 0                  | Brazos       | 2        |
| 0                  | Chambers     | 1        |
| 0                  | Collin       | 1        |
| 0                  | Dallas       | 6        |
| 0                  | Denton       | 2        |
| 0                  | El Paso      | 1        |
| 0                  | Fort Bend    | 2        |
| 0                  | Galveston    | 3        |
| 0                  | Gillespie    | 1        |
| 0                  | Gregg        | 2        |
| 0                  | Hamilton     | 1        |
| 0                  | Hardin       | 1        |
| 0                  | Harris       | 33       |
| 0                  | Henderson    | 1        |
| 0                  | Jefferson    | 4        |
| 0                  | Johnson      | 2        |
| 0                  | Lee          | 1        |
| 0                  | Liberty      | 1        |
| 0                  | Lubbock      | 1        |
| 0                  | McLennan     | 1        |
| 0                  | Montgomery   | 3        |
| 0                  | Navarro      | 2        |
| 0                  | Nueces       | 5        |
| 0                  | Pecos        | 1        |
| 0                  | Potter       | 3        |
| 0                  | San Patricio | 1        |
| 0                  | Shelby       | 1        |
| 0                  | Tarrant      | 6        |
| 0                  | Taylor       | 1        |
| 0                  | Travis       | 2        |
| 0                  | Upshur       | 1        |
| 0                  | Val Verde    | 1        |
| 0                  | Victoria     | 1        |
| 0                  | Walker       | 1        |
| 0                  | Wharton      | 1        |
| 0                  | Wilbarger    | 2        |
| 1                  | Anderson     | 4        |
| 1                  | Aransas      | 1        |
| 1                  | Atascosa     | 1        |
| 1                  | Bailey       | 1        |
| 1                  | Bastrop      | 1        |
| 1                  | Bee          | 2        |
| 1                  | Bell         | 2        |
| 1                  | Bexar        | 37       |
| 1                  | Bowie        | 5        |
| 1                  | Brazoria     | 3        |
| 1                  | Brazos       | 10       |
| 1                  | Brown        | 1        |
| 1                  | Caldwell     | 1        |
| 1                  | Cameron      | 6        |
| 1                  | Cherokee     | 3        |
| 1                  | Clay         | 1        |
| 1                  | Collin       | 6        |
| 1                  | Comal        | 2        |
| 1                  | Coryell      | 1        |
| 1                  | Crockett     | 1        |
| 1                  | Dallas       | 52       |
| 1                  | Dawson       | 1        |
| 1                  | Denton       | 4        |
| 1                  | El Paso      | 2        |
| 1                  | Ellis        | 2        |
| 1                  | Fort Bend    | 3        |
| 1                  | Freestone    | 1        |
| 1                  | Galveston    | 3        |
| 1                  | Grayson      | 3        |
| 1                  | Gregg        | 3        |
| 1                  | Hale         | 2        |
| 1                  | Harris       | 95       |
| 1                  | Harrison     | 1        |
| 1                  | Henderson    | 1        |
| 1                  | Hidalgo      | 6        |
| 1                  | Hopkins      | 1        |
| 1                  | Houston      | 1        |
| 1                  | Hunt         | 4        |
| 1                  | Jefferson    | 11       |
| 1                  | Jones        | 1        |
| 1                  | Kaufman      | 1        |
| 1                  | Kendall      | 1        |
| 1                  | Kerr         | 3        |
| 1                  | Kleberg      | 1        |
| 1                  | Lamar        | 2        |
| 1                  | Leon         | 2        |
| 1                  | Liberty      | 2        |
| 1                  | Llano        | 1        |
| 1                  | Lubbock      | 12       |
| 1                  | Madison      | 1        |
| 1                  | Matagorda    | 2        |
| 1                  | McLennan     | 6        |
| 1                  | Milam        | 1        |
| 1                  | Montgomery   | 12       |
| 1                  | Morris       | 1        |
| 1                  | Nacogdoches  | 1        |
| 1                  | Navarro      | 4        |
| 1                  | Newton       | 1        |
| 1                  | Nueces       | 11       |
| 1                  | Parker       | 2        |
| 1                  | Pecos        | 1        |
| 1                  | Polk         | 2        |
| 1                  | Potter       | 7        |
| 1                  | Randall      | 3        |
| 1                  | Red River    | 2        |
| 1                  | Refugio      | 2        |
| 1                  | Sabine       | 1        |
| 1                  | San Jacinto  | 1        |
| 1                  | Scurry       | 1        |
| 1                  | Smith        | 12       |
| 1                  | Tarrant      | 35       |
| 1                  | Taylor       | 4        |
| 1                  | Tom Green    | 3        |
| 1                  | Travis       | 6        |
| 1                  | Trinity      | 1        |
| 1                  | Victoria     | 1        |
| 1                  | Walker       | 2        |
| 1                  | Wichita      | 2        |
| 1                  | Williamson   | 3        |
| 1                  | Wood         | 1        |

---

## The HAVING Block
- Filtering with `WHERE` occurs **before** grouping and aggregation
- Use `HAVING` to filter after grouping data
**Example:**
*Count the number of inmates aged 50 or older that were executed in each county*

```sql
SELECT
   county,
   COUNT(*) as number_exec
FROM executions
WHERE ex_age >= 50
GROUP BY county
```

**Output (truncated to top 5):**
| county | number_exec |
|--------|-------------|
| Anderson | 1 |
| Bexar | 2 |
| Caldwell | 1 |
| Cameron | 1 | 
| Colin | 2 |
| ... | ... |

- In this example, we are looking for the number of executions of inmates over the age of 50 in each county. Since we only want executions over 50 across all counties, we can use the WHERE clause.

**What do we do if we want to filter on the result of the grouping/aggregation?**
- Use the HAVING block
- Syntax is HAVING *condition*
**Example:**
*List the counties in which more than 2 inmates aged 50 or older have been executed*

``` sql
SELECT
   county
FROM executions
WHERE ex_age >= 50
GROUP BY county
HAVING COUNT(*) > 2
```

**Output:**
| county |
|--------|
| Dallas |
| Harris | 
| Lubbock |
| Montogomery |
| Tarrant |

- In the above example, we only wanted the counties with more than two inmates over the age of 50. We would not have been able to filter the counties using WHERE so we use HAVING after grouping by counties.

---
## Nested Queries
- Nested queries are when we feed a query as an argument for a block of another query
  - We use this when we need to perform aggregations on the row level **AND** across the whole data
  - Consider calculating a percentage count: You need to count the individual rows for the numerator but you also need to aggregate and count across the whole dataset for the denominator
- Use parentheses to indicate the boundary between the inner and outer query

**Example:**
*Find the first and last name of the inmate with the longest last statement by character count*

```sql
SELECT
   first_name, last_name
FROM executions
WHERE LENGTH(last_statement) =
   (SELECT MAX(LENGTH(last_statement))
    FROM executions)
```

*Output:*
|first_name | last_name |
|-----------|-----------|
| Gary | Graham |

**Example:**
*Find the percentage of executions from each county*

```sql
SELECT
   county,
   100.0 * COUNT(*) / (SELECT COUNT(*) FROM executions)
FROM executions
GROUP BY county
ORDER BY percentage DESC
```

*Output:*
| county       | percentage          |
|--------------|---------------------|
| Harris       | 23.146473779385172  |
| Dallas       | 10.488245931283906  |
| Bexar        | 8.318264014466546   |
| Tarrant      | 7.414104882459313   |
| Nueces       | 2.8933092224231465  |
| Jefferson    | 2.7124773960216997  |
| Montgomery   | 2.7124773960216997  |
| Lubbock      | 2.3508137432188065  |
| Brazos       | 2.1699819168173597  |
| Smith        | 2.1699819168173597  |
| Potter       | 1.8083182640144666  |
| Travis       | 1.4466546112115732  |
| Collin       | 1.2658227848101267  |
| McLennan     | 1.2658227848101267  |
| Cameron      | 1.0849909584086799  |
| Denton       | 1.0849909584086799  |
| Galveston    | 1.0849909584086799  |
| Hidalgo      | 1.0849909584086799  |
| Navarro      | 1.0849909584086799  |
| Bowie        | 0.9041591320072333  |
| Fort Bend    | 0.9041591320072333  |
| Gregg        | 0.9041591320072333  |
| Taylor       | 0.9041591320072333  |
| Anderson     | 0.7233273056057866  |
| Brazoria     | 0.7233273056057866  |
| Hunt         | 0.7233273056057866  |
| Bell         | 0.5424954792043399  |
| Cherokee     | 0.5424954792043399  |
| El Paso      | 0.5424954792043399  |
| Grayson      | 0.5424954792043399  |
| Kerr         | 0.5424954792043399  |
| Liberty      | 0.5424954792043399  |
| Randall      | 0.5424954792043399  |
| Tom Green    | 0.5424954792043399  |
| Walker       | 0.5424954792043399  |
| Williamson   | 0.5424954792043399  |
| Bee          | 0.3616636528028933  |
| Comal        | 0.3616636528028933  |
| Ellis        | 0.3616636528028933  |
| Hale         | 0.3616636528028933  |
| Henderson    | 0.3616636528028933  |
| Johnson      | 0.3616636528028933  |
| Lamar        | 0.3616636528028933  |
| Leon         | 0.3616636528028933  |
| Matagorda    | 0.3616636528028933  |
| Parker       | 0.3616636528028933  |
| Pecos        | 0.3616636528028933  |
| Polk         | 0.3616636528028933  |
| Red River    | 0.3616636528028933  |
| Refugio      | 0.3616636528028933  |
| Victoria     | 0.3616636528028933  |
| Wichita      | 0.3616636528028933  |
| Wilbarger    | 0.3616636528028933  |
| Aransas      | 0.18083182640144665 |
| Atascosa     | 0.18083182640144665 |
| Bailey       | 0.18083182640144665 |
| Bastrop      | 0.18083182640144665 |
| Brown        | 0.18083182640144665 |
| Caldwell     | 0.18083182640144665 |
| Chambers     | 0.18083182640144665 |
| Clay         | 0.18083182640144665 |
| Coryell      | 0.18083182640144665 |
| Crockett     | 0.18083182640144665 |
| Dawson       | 0.18083182640144665 |
| Freestone    | 0.18083182640144665 |
| Gillespie    | 0.18083182640144665 |
| Hamilton     | 0.18083182640144665 |
| Hardin       | 0.18083182640144665 |
| Harrison     | 0.18083182640144665 |
| Hopkins      | 0.18083182640144665 |
| Houston      | 0.18083182640144665 |
| Jones        | 0.18083182640144665 |
| Kaufman      | 0.18083182640144665 |
| Kendall      | 0.18083182640144665 |
| Kleberg      | 0.18083182640144665 |
| Lee          | 0.18083182640144665 |
| Llano        | 0.18083182640144665 |
| Madison      | 0.18083182640144665 |
| Milam        | 0.18083182640144665 |
| Morris       | 0.18083182640144665 |
| Nacogdoches  | 0.18083182640144665 |
| Newton       | 0.18083182640144665 |
| Sabine       | 0.18083182640144665 |
| San Jacinto  | 0.18083182640144665 |
| San Patricio | 0.18083182640144665 |
| Scurry       | 0.18083182640144665 |
| Shelby       | 0.18083182640144665 |
| Trinity      | 0.18083182640144665 |
| Upshur       | 0.18083182640144665 |
| Val Verde    | 0.18083182640144665 |
| Wharton      | 0.18083182640144665 |
| Wood         | 0.18083182640144665 |

---

## Practice
**Question 1:**
*This query finds the number of inmates from each county and 10 year age range. Mark the statements that are true.*

```sql
SELECT
   county,
   ex_age/10 AS decade_age
   COUNT(*)
FROM executions
GROUP BY county, decade_age
```
*Options:*
1. The query is valid
2. The query would return more rows if we were to use ex_age instead of ex_age/10
3. The output will have as many rows as there are unique combinations of counties and decade_ages in the dataset
4. The output will have a group ('Bexar', 6) even though no Bexar county inmates were between 60 adn 69 at execution time
5. The ouput will have a different value of county for every row it returns
6. The output can have groups where the count is 0
7. The query would be valid even if we don't specify county in the `SELECT` block
8. It is reasonable to add last_name to the `SELECT` block even without grouping it

*Answer (bolded are true):*
**1. The query is valid**
**2. The query would return more rows if we were to use ex_age instead of ex_age/10**
**3. The output will have as many rows as there are unique combinations of counties and decade_ages in the dataset**
4. The output will have a group ('Bexar', 6) even though no Bexar county inmates were between 60 adn 69 at execution time
5. The ouput will have a different value of county for every row it returns
6. The output can have groups where the count is 0
**7. The query would be valid even if we don't specify county in the `SELECT` block**
8. It is reasonable to add last_name to the `SELECT` block even without grouping it

**Question 2:**
*List all the distinct counties in the dataset*

```sql
SELECT
   county
FROM executions
GROUP BY county
```

*Output (truncated):*
|county|
|------|
|Anderson|
|Aransas|
|Atascosa|
|...|




