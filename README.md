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
### 2. What are the least and most expensive items on the menu?

```sql
SELECT * 
FROM 
menu_items
ORDER BY 
price;
SELECT * 
FROM 
menu_items
ORDER BY 
price desc;
```
### 3. How many Italian dishes are on the menu?

```sql
SELECT   
COUNT(*) AS italian_dishes
FROM
menu_items
WHERE 
category ='Italian';
```
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
### 7.  What is the date range?

```sql
SELECT                       
MIN(order_date),
MAX(order_date)
FROM
order_details;
```
### 8.  How many orders were made within this date range?

```sql
SELECT                     
COUNT(DISTINCT order_id) AS total_orders
FROM
order_details;
```
### 9.  How many items were ordered within this date range?

```sql
SELECT                     
COUNT(*) AS total_items_ordered
FROM
order_details;
```
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
