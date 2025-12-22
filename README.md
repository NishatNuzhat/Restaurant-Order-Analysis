# Restaurant-Order-Analysis
## Overview
Taste of the world cafe, a restaurant that has diverse menu offerings and serves generous portions. It debuted a new menu at the start of the year. Explored into the customer data to see which menu items are doing well, not well and what the top customers seem to like.

## The Objectives
- Get an idea of what's on the new menu.
- Explore order details to get an idea of the data that's been collected.
- Understand how customers are reacting to the new menu.

## Dataset
The data for this project is sourced from Maven Analytics.

## Schema

```sql
DROP SCHEMA IF EXISTS restaurant_db;
CREATE SCHEMA restaurant_db;
USE restaurant_db;

--
-- Table structure for table `order_details`
--

CREATE TABLE order_details (
  order_details_id SMALLINT NOT NULL,
  order_id SMALLINT NOT NULL,
  order_date DATE,
  order_time TIME,
  item_id SMALLINT,
  PRIMARY KEY (order_details_id)
);

--
-- Table structure for table `menu_items`
--

CREATE TABLE menu_items (
  menu_item_id SMALLINT NOT NULL,
  item_name VARCHAR(45),
  category VARCHAR(45),
  price DECIMAL(5,2),
  PRIMARY KEY (menu_item_id)
);

);
```
## Business Problems and Solutions

### 1. Find the number of items on the menu

```sql
SELECT      
COUNT(*) AS number_of_items
FROM menu_items;
```
-- OUTPUT --
| number_of_items |
|-----------------|
| 32              |
### 2. What are the least and most expensive items on the menu?

```sql
SELECT * 
FROM 
menu_items
ORDER BY 
price;
```
-- OUTPUT --
| menu_item_id | item_name | category | price |
|--------------|-----------|----------|-------|
| 113          | Edamame   | Asian    | 5.00  |
| 105          | Mac & Cheese | American | 7.00  |
| 106          | French Fries | American | 7.00  |
| 122          | Chips & Salsa | Mexican  | 7.00  |
| 103          | Hot Dog   | American | 9.00  |
```sql
SELECT * 
FROM 
menu_items
ORDER BY 
price desc;
```
-- OUTPUT --
| menu_item_id | item_name | category | price |
|--------------|-----------|----------|-------|
| 130          | Shrimp Scampi | Italian  | 19.95 |
| 109          | Korean Beef Bowl | Asian    | 17.95 |
| 110          | Pork Ramen | Asian    | 17.95 |
| 125          | Spaghetti & Meatballs | Italian  | 17.95 |
| 127          | Meat Lasagna | Italian  | 17.95 |
### 3. How many Italian dishes are on the menu?

```sql
SELECT   
COUNT(*) AS italian_dishes
FROM
menu_items
WHERE 
category ='Italian';
```
-- OUTPUT --
| italian_dishes |
|----------------|
| 9              |
### 4. What are the least and most expensive Italian dishes on the menu?

```sql
SELECT  
*
FROM
menu_items
WHERE 
category = 'Italian'
ORDER BY
price;
```
-- OUTPUT --
| menu_item_id | item_name | category | price |
|--------------|-----------|----------|-------|
| 124          | Spaghetti | Italian  | 14.50 |
| 126          | Fettuccine Alfredo | Italian | 14.50 |
| 128          | Cheese Lasagna | Italian | 15.50 |
| 129          | Mushroom Ravioli | Italian | 15.50 |
| 132          | Eggplant Parmesan | Italian | 16.95 |

```sql
SELECT  
*
FROM
menu_items
WHERE 
category = 'Italian'
ORDER BY
price desc;
```
-- OUTPUT --
| menu_item_id | item_name | category | price |
|--------------|-----------|----------|-------|
| 130          | Shrimp Scampi | Italian  | 19.95 |
| 125          | Spaghetti & Meatballs | Italian  | 17.95 |
| 127          | Meat Lasagna | Italian  | 17.95 |
| 131          | Chicken Parmesan | Italian | 17.95 |
| 132          | Eggplant Parmesan | Italian | 16.95 |
### 5. How many dishes are in each category? 

```sql
SELECT 
category,
COUNT(category) AS total_dishes
FROM
menu_items
GROUP BY
category;
```
-- OUTPUT --
| category | total_dishes |
|----------|--------------|
| American | 6            |
| Asian    | 8            |
| Mexican  | 9            |
| Italian  | 9            |
### 6.  What is the average dish price within each category?

```sql
SELECT  
category,
ROUND(AVG(price),2) AS avg_price
FROM
menu_items
GROUP BY
category
```
-- OUTPUT --
| category | avg_price |
|----------|-----------|
| American | 10.07     |
| Asian    | 13.48     |
| Mexican  | 11.80     |
| Italian  | 16.75     |
### 7.  What is the date range?

```sql
SELECT                       
MIN(order_date),
MAX(order_date)
FROM
order_details;
```
-- OUTPUT --
| MIN(order_date) | MAX(order_date) |
|-----------------|-----------------|
| 2023-01-01      | 2023-03-31      |
### 8.  How many orders were made within this date range?

```sql
SELECT                     
COUNT(DISTINCT order_id) AS total_orders
FROM
order_details;
```
-- OUTPUT --
| total_orders |
|--------------|
| 5370         |
### 9.  How many items were ordered within this date range?

```sql
SELECT                     
COUNT(*) AS total_items_ordered
FROM
order_details;
```
-- OUTPUT --
| total_items_ordered |
|---------------------|
| 12234               |
### 10.  Which orders had the most number of items?

```sql
SELECT                    
order_id,
COUNT(item_id) AS number_of_items
FROM
order_details
GROUP BY
order_id
ORDER BY 
2 desc;
```
-- OUTPUT --
| order_id | number_of_items |
|----------|-----------------|
| 3473     | 14              |
| 443      | 14              |
| 4305     | 14              |
| 2675     | 14              |
| 440      | 14              |
| 330      | 14              |
| 1957     | 14              |
| 4836     | 13              |
### 11.  How many orders had more than 12 items?

```sql
SELECT COUNT(*) FROM   
(SELECT                    
order_id,
COUNT(item_id) AS number_of_items
FROM
order_details
GROUP BY
order_id
HAVING
COUNT(item_id) > 12) AS a;
```
-- OUTPUT --
| COUNT(*) |
|----------|
| 20       |
### 12.  Combine the menu_items and order_details tables into a single table

```sql
SELECT             
o.order_details_id,
o.order_id,
o.order_date,
o.order_time,
o.item_id,
m.item_name,
m.category,
m.price
FROM
order_details o 
LEFT JOIN
menu_items m 
ON
o.item_id = m.menu_item_id;
```
-- OUTPUT --
| order_details_id | order_id | order_date | order_time | item_id | item_name | category | price |
|------------------|----------|------------|------------|---------|-----------|----------|-------|
| 1                | 1        | 2023-01-01 | 11:38:36   | 109     | Korean Beef Bowl | Asian    | 17.95 |
| 2                | 2        | 2023-01-01 | 11:57:40   | 108     | Tofu Pad Thai | Asian    | 14.50 |
| 3                | 2        | 2023-01-01 | 11:57:40   | 124     | Spaghetti | Italian  | 14.50 |
| 4                | 2        | 2023-01-01 | 11:57:40   | 117     | Chicken Burrito | Mexican  | 12.95 |
| 5                | 2        | 2023-01-01 | 11:57:40   | 129     | Mushroom Ravioli | Italian  | 15.50 |
### 13.  What were the least and most ordered items? What categories were they in?

```sql
SELECT              
item_name,category,       
COUNT(order_details_id) AS num_purchases
FROM
(SELECT
o.order_details_id,
o.order_id,
o.order_date,
o.order_time,
o.item_id,
m.item_name,
m.category,
m.price
FROM
order_details o 
LEFT JOIN
menu_items m 
ON
o.item_id = m.menu_item_id) AS single_table
group by item_name, category
ORDER BY 3 ;
```
-- OUTPUT --
| item_name | category | num_purchases |
|-----------|----------|---------------|
| Chicken Tacos | Mexican  | 123           |
```sql
SELECT              
item_name,category,       
COUNT(order_details_id) AS num_purchases
FROM
(SELECT
o.order_details_id,
o.order_id,
o.order_date,
o.order_time,
o.item_id,
m.item_name,
m.category,
m.price
FROM
order_details o 
LEFT JOIN
menu_items m 
ON
o.item_id = m.menu_item_id) AS single_table
group by item_name, category
ORDER BY 3 desc;
```
-- OUTPUT --
| item_name | category | num_purchases |
|-----------|----------|---------------|
| Hamburger | American | 622           |

### 14.  What were the top 5 orders that spent the most money?

```sql
SELECT order_id,SUM(price) AS money_spent  
FROM (SELECT
o.order_details_id,
o.order_id,
o.order_date,
o.order_time,
o.item_id,
m.item_name,
m.category,
m.price
FROM
order_details o 
LEFT JOIN
menu_items m 
ON
o.item_id = m.menu_item_id) AS single_table
GROUP BY order_id
ORDER BY 2 desc
LIMIT 5;
```
-- OUTPUT --
| order_id | money_spent |
|----------|-------------|
| 440      | 192.15      |
| 2075     | 191.05      |
| 1957     | 190.10      |
| 330      | 189.70      |
| 2675     | 185.10      |
### 15.  View the details of the highest spend order. Which specific items were purchased?

```sql
SELECT category,count(item_id) as num_of_items
FROM (SELECT
o.order_details_id,
o.order_id,
o.order_date,
o.order_time,
o.item_id,
m.item_name,
m.category,
m.price
FROM
order_details o 
LEFT JOIN
menu_items m 
ON
o.item_id = m.menu_item_id) AS single_table
WHERE order_id = 440
GROUP BY category;
```
-- OUTPUT --
| category | num_of_items |
|----------|--------------|
| Mexican  | 2            |
| American | 2            |
| Italian  | 8            |
| Asian    | 2            |
### 16.  View the details of the top 5 highest spend orders

```sql
SELECT order_id,category,count(item_id) as num_of_items
FROM (SELECT
o.order_details_id,
o.order_id,
o.order_date,
o.order_time,
o.item_id,
m.item_name,
m.category,
m.price
FROM
order_details o 
LEFT JOIN
menu_items m 
ON
o.item_id = m.menu_item_id) AS single_table
WHERE order_id IN (440,2075,1957,330,2675)
GROUP BY order_id,category;
```
## Findings and Conclusion

The least ordered item was the Chicken Tacos and the most ordered item was a Hamburger. The most ordered items were American and Asian items and mostly Mexican items were least ordered. People seem to be ordering Italian dishes a lot, especially the highest spend customers.

This analysis provides a comprehensive view of customer's behaviour towards the menu of that restaurant and can help inform strategy and decision-making.
