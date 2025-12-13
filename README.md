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

## Business Problems and Solutions

### 1. find the number of items on the menu

```sql
SELECT      
COUNT(*) AS number_of_items
FROM menu_items;
```
