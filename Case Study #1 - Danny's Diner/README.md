

## Table of Contents
1. [Business Case](#business-case)
2. [Entity Relationship Diagram](#entity-relationship-diagram)
3. [Solutions](#solutions)

[View on DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138)

## Business Case
Danny's Diner is a small restaurant that serve three dishes -- sushi, curry, and ramen. They have collected several months of operational data that he wants to use to answer some key questions about his business. Namely, he wants to know how much money customers are spending, whether he should expand the customer loyalty program, what the main customer visiting patterns are, and what dishes are customers' favorites. He also wants to be able to create some data sets for his team so they do not need to use SQL. 

## Entity Relationship Diagram

## Solutions
**1. What is the total amount each customer spent at the restaurant?**

    SELECT 
    	sales.customer_id, 
    	SUM(menu.price) AS sum_sales
    FROM dannys_diner.sales
    INNER JOIN dannys_diner.menu 
    	ON sales.product_id = menu.product_id
    GROUP BY sales.customer_id
    ORDER BY sum_sales DESC;

| customer_id | sum_sales |
| ----------- | --------- |
| A           | 76        |
| B           | 74        |
| C           | 36        |

---

[View on DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138)
| customer_id | sum_sales |
| ----------- | --------- |
| A           | 76        |
| B           | 74        |
| C           | 36        |

---
