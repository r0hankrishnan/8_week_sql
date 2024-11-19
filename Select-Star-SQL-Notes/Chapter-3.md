# SELECT STAR SQL CHAPTER 3: THE LONG TAIL-- NOTES
#### [Chapter 3](https://selectstarsql.com/longtail.html) Takeaways:
   1. 

## Table of Contents:
1. [The GROUP BY Block](#the-group-by-block)
2. [The HAVING Block](#the-having-block)
---

## The GROUP BY Block
- Split up data into groups and perform aggregations that result in one row **per group**
  - Helpful when we need to perform aggregations over group instances of our       rows instead over the whole table

**Example:**
```sql
SELECT
   county,
   COUNT(*) AS county_executions
FROM executions
GROUP BY county
```

**Output (truncated to top 5):**
| county | county_executions |
|--------|-------------------|
| Anderson | 4 |
| Aransas | 1 |
| Atascosa | 1 |
| Bailey | 1 |
| Bastrop | 1 |
| ... | ... |

- Why can we mix non aggregated columns (county) with aggregated columns (county_executions)?
  - With GROUP BY, **only** the grouped columns are allowed to be non-aggregate
  - If you group multiple columns, they must be in the same order in the SELECT    statement as they are in the GROUP BY statement
- Notice also the use of AS, which is called **aliasing**
  - Whatever you refer to your expression with using AS can be referenced later    in the query, allowing us to avoiding rewriting long expressions
---

## The HAVING Block
- Filtering with WHERE occurs **before** grouping and aggregation
**Example:**
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

**What do we do if we want to filter on the result of the grouping/aggregation?**
- Use the HAVING block
- Syntax is HAVING *condition*
**Example:**
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
