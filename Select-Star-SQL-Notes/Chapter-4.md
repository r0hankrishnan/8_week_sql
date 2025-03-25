# SELECT STAR SQL CHAPTER 4: EXECUTION HIATUSES-- NOTES
#### [Chapter 4](https://selectstarsql.com/hiatuses.html) Takeaways:
   1. Use joins to pull in row-level info from other tables
   2. `JOIN` defaults to inner join --> will only return matching rows
   3. Other join options include `LEFT JOIN`, `RIGHT JOIN`, and `OUTER JOIN`
   4. SQLITE does not have a "date" type so you need to use a function when subtracting dates
   5. You can include query-generated tables in your `JOIN` block

## Table of Contents:
1. [JOINS](#JOINS)
2. [TYPES OF JOINS](#TYPES-OF-JOINS)
3. [DATES](#DATES)
4. [SELF JOINS](SELF-JOINS)
---

## JOINS
- We use joins to pull in row level information that isn't contained in the current table we are working in
- We join on a **key** >> there should be a column in both tables that can be matched
  - Syntax: `FROM <table 1> JOIN ON <table 1>.<key> = <table 2>.<key>`
- `JOIN` creates a big **combined** table that is then fed into the `FROM` block like any other table
---

## TYPES OF JOINS
- `JOIN` is executed like `WHERE` blocks in that they evaluate to TRUE or FALSE
  - If two rows match on the `JOIN` criteria, they are joined
- `JOIN` defaults to **inner join**
  - If a row doesn't match, **it is dropped**
- How do you perserve all the rows on the **left**, **right**, or **both** table(s)?
  - Use `LEFT JOIN` in place of `JOIN` to keep all rows from the left table (the first table in the `JOIN` block) and leave NULLs for rows that don't match in the right table
  - Use `RIGHT JOIN` in place of `JOIN` to keep all rows from the right table (the second table in the `JOIN` block) and leave NULLs for rows that don't match in the left table
  - Use `OUTER JOIN` in place of `JOIN` to keep all rows from both tables, with NULLs placed in empty cells
- What happens if a row has **2** (duplicate) matches in a `JOIN`?
  - `JOIN` will create enough rows in the left table to match the matching rows in the right table
  - This can result in `JOIN`ed tables that are larger than their composite tables
---

## DATES
- How does the computer know what a date is?
  - It **doesn't** unless it is a specific type of data
  - In SQLITE, there is no "date" data type so the computer reads dates as strings
  - We need to apply a helper function to first tell the computer that we are working with dates (in the case of SQLITE it's the `JULIANDAY` function)
- If we try to find the difference between two dates, how does the computer calculate the value?
  - In SQLITE, we use the `JULIANDATE` to convert the date strings to a numeric value and then perform the mathematical operation we want to do
---

## SELF JOINS
- You can nest query-created tables into `JOIN` statements
- Most useful for heirarchical data
