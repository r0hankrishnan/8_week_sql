# SELECT STAR SQL CHAPTER 1: BEAZLEY'S LAST STATEMENT-- NOTES
#### [Chapter 1](https://selectstarsql.com/beazley.html) Takeaways:
   1. Think of SQL statements like legos -- specific blocks with specific functions that fit together in defined ways
   2. The general syntax of a simple SQL query is `SELECT <column>,... FROM <table> WHERE <clause>;`

## Table of Contents:
1. [Thinking about SQL Queries](#thinking-about-sql-queries)
2. [SQL Syntax](#sql-syntax)
   - [SELECT](#select-statments)
   - [FROM](#from-statements)
   - [WHERE](#where-statements)
   - [Commenting](#commenting)
   - [Capitalization](#capitalization)
   - [Strings](#strings)
   - [Whitespace](#whitespace)
3. [Chapter 1 Project](#chapter-1-project)
---
## Thinking about SQL Queries
**Main Takeaway 1:** Queries are constructed like legos
  - Think of "blocks" instead of individual words
    - [SELECT *] + [FROM table] + [LIMIT 3]
  - Each block has a specific format and function and fits in with the other blocks in a *specific way*
---
## SQL Syntax
### SELECT Statements
**Main Takeaway 1:** `SELECT` tells the DB what columns you want to output
- Purpose: pick which columns you want to show
- Format: `SELECT <column>, <column>, ...`
  - Use `*` to select all columns in a table
----
### FROM Statements
**Main Takeaway 1:** `FROM` tells the DB what table to pull the selected columns from
- Purpose: chose table to query from
- Format: `FROM <table>`
  - `FROM` *ALWAYS comes AFTER* `SELECT`
  - Note that the query will not run if you misspell the table name
- Do not need to use `FROM` if not using any data from a table
----
### WHERE Statements
**Main Takeaway 1:** `WHERE` tells the DB to filter rows that match the boolean condition we set
- Purpose: filter chosen tables for rows that meet certain conditions
- Format: `WHERE <clause>`
  - `WHERE` *ALWAYS comes AFTER* `FROM`
  - A clause = statement that can be evaluated to TRUE or FALSE
    - e.g. `ex_number = 145`
- Use arithmetic operators like `>`, `>=`, etc. --> *do NOT use `==`*
- Also several string operators that work with `WHERE`
    - `LIKE` allows us to use `%` and `_` to match various characters:
      - `%` returns any string starting with "whatever" and ending with the arg (or starting with arg if `%` is at end)
        - `first_name LIKE '%roy'` returns roy, Troy, Deroy but not royman   because royman has characters after the arg 'roy'
      - `_` returns only any string that contains the arg after the *first* character (or before if `_` is in front)
        -`first_name LIKE '_roy'` only returns Troy because roy and royman do not have characters before 'roy' and Deroy has 2 characters before 'roy'
    - `AND`, `NOT`, `OR` are also useful operators
----
### Commenting
- Use `/*` at the start and end of lines to comment them out
  - Can also use `--` to comment out entire lines
----
### Capitalization
- SQL commands are *not* case-sensitive BUT it is good practice to capitalize SQL commands for ease of reading
- SQL table names, column names, and variables *are* case-sensitive for many versions of SQL
----
### Strings
- Denote with *single quotes*
- Backticks (`) can be used for table and column names
----
### Whitespace
- SQL is not sensitive to whitespace (as long as different words are separated)
- Good practice to put each new command on its own line for most queries
---
## Chapter 1 Project
**Find Napoleon Beazeley's last statement:**
- What column do we want? Last statements --> last_statement
- From what table? executions
- Any conditions? The name has to be Napoleon Beazely. We don't know if it is entered exactly as that so lets make it so our query returns any names with 'Napoleon' in the first name and 'Beazeley' in the last name

**Query:**
```sql
SELECT last_statement
FROM executions
WHERE first_name LIKE '%Napoleon%'
AND last_name LIKE '%Beazley%';
```

|last_statement|
|--------------|
|The act I committed to put me here was not just heinous, it was senseless. But the person that committed that act is no longer here - I am. I'm not going to struggle physically against any restraints. I'm not going to shout, use profanity or make idle threats. Understand though that I'm not only upset, but I'm saddened by what is happening here tonight. I'm not only saddened, but disappointed that a system that is supposed to protect and uphold what is just and right can be so much like me when I made the same shameful mistake. If someone tried to dispose of everyone here for participating in this killing, I'd scream a resounding, "No." I'd tell them to give them all the gift that they would not give me...and that's to give them all a second chance. I'm sorry that I am here. I'm sorry that you're all here. I'm sorry that John Luttig died. And I'm sorry that it was something in me that caused all of this to happen to begin with. Tonight we tell the world that there are no second chances in the eyes of justice...Tonight, we tell our children that in some instances, in some cases, killing is right. This conflict hurts us all, there are no SIDES. The people who support this proceeding think this is justice. The people that think that I should live think that is justice. As difficult as it may seem, this is a clash of ideals, with both parties committed to what they feel is right. But who's wrong if in the end we're all victims? In my heart, I have to believe that there is a peaceful compromise to our ideals. I don't mind if there are none for me, as long as there are for those who are yet to come. There are a lot of men like me on death row - good men - who fell to the same misguided emotions, but may not have recovered as I have. Give those men a chance to do what's right. Give them a chance to undo their wrongs. A lot of them want to fix the mess they started, but don't know how. The problem is not in that people aren't willing to help them find out, but in the system telling them it won't matter anyway. No one wins tonight. No one gets closure. No one walks away victorious.|
