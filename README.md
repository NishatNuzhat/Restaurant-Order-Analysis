# Restaurant-Order-Analysis
## Overview
Taste of the world cafe, a restaurant that has diverse menu offerings and serves generous portions. It debuted a new menu at the start of the year. Explored into the customer data to see which menu items are doing well, not well and what the top customers seem to like.

## The Objectives
- Get an idea of what's on the new menu.
- Explore order details to get an idea of the data that's been collected.
- Understand how customers are reacting to the new menu.

## Dataset
The data for this project is sourced from the Maven Analytics.

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
