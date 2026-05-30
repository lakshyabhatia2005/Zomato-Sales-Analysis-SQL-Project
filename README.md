# 🍕 Zomato Sales Analysis — SQL Project

![MySQL](https://img.shields.io/badge/Tool-MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Domain](https://img.shields.io/badge/Domain-Food%20Tech%20%7C%20Sales%20Analytics-orange?style=for-the-badge)

---

## 📌 Project Overview

This project involves analyzing a **Zomato sales database** containing real-world style data on customers, orders, restaurants, employees, and food items. The goal is to extract meaningful business insights using SQL queries — covering revenue analysis, customer behavior, delivery performance, and more.

---

## 🗄️ Database Schema

The database `zomatodb` consists of the following tables:

| Table | Description |
|---|---|
| `customer` | Customer details — ID, name, contact info |
| `foods` | Food items — ID, name, price per unit, restaurant |
| `order_detail` | Order info — ID, status, order time, delivery time |
| `order_food` | Order line items — food ID, quantity, customer, restaurant |
| `payment_table` | Payment records — order ID, payment type |
| `restaurant` | Restaurant info — ID, name, location, rating |
| `zomato_employee` | Employee info — ID, name, average delivery rating |

---

## 📋 Tasks & SQL Queries

### 1. 🏆 Top 3 Customers by Orders
```sql
SELECT c.customer_id, c.customer_name, COUNT(ofd.orderF_id) AS total_orders 
FROM customer c 
JOIN order_food AS ofd ON c.customer_id = ofd.customer_id 
GROUP BY c.customer_id, c.customer_name 
ORDER BY total_orders DESC LIMIT 3;
```

---

### 2. ⭐ Restaurant with Highest Average Rating
```sql
SELECT restaurant_id, restaurant_name, rlocation, AVG(rrating) AS average_rating 
FROM restaurant
GROUP BY restaurant_name, rlocation, restaurant_id 
ORDER BY average_rating DESC LIMIT 1;
```

---

### 3. ⚡ Orders Delivered in Under 30 Minutes
```sql
SELECT order_id, order_status, order_time, delivered_time,
TIMESTAMPDIFF(MINUTE, order_time, delivered_time) AS delivery_minutes
FROM order_detail 
WHERE TIMESTAMPDIFF(MINUTE, order_time, delivered_time) < 30;
```

---

### 4. 💰 Total Revenue by Food Item
```sql
SELECT f.food_id, f.food_name, SUM(f.price_per_unit * quantity) AS total_revenue
FROM foods f 
JOIN order_food ON f.food_id = order_food.food_id
GROUP BY f.food_id, f.food_name 
ORDER BY total_revenue DESC;
```

---

### 5. 🥈 Second Highest Revenue-Generating Restaurant
```sql
SELECT r.restaurant_id, r.restaurant_name, SUM(f.price_per_unit * ofd.quantity) AS total_revenue 
FROM restaurant r
JOIN foods f ON r.restaurant_id = f.restaurant_id 
JOIN order_food ofd ON f.food_id = ofd.food_id
GROUP BY r.restaurant_id, r.restaurant_name 
ORDER BY total_revenue DESC LIMIT 1 OFFSET 1;
```

---

### 6. 🍔 Top 5 Most Popular Food Items
```sql
SELECT f.food_id, f.food_name, SUM(ofd.quantity) AS total_quantity_sold 
FROM order_food ofd
JOIN foods f ON ofd.food_id = f.food_id
GROUP BY f.food_id, f.food_name 
ORDER BY total_quantity_sold DESC LIMIT 5;
```

---

### 7. 🚴 Top 3 Employees by Delivery Rating
```sql
SELECT employee_id, employee_name 
FROM zomato_employee 
ORDER BY employee_avg_rating DESC LIMIT 3;
```

---

### 8. 📅 Month with Highest Number of Orders
```sql
SELECT MONTH(order_time) AS month_no, MONTHNAME(order_time) AS month_name, COUNT(*) AS total_orders 
FROM order_detail
GROUP BY MONTH(order_time), MONTHNAME(order_time) 
ORDER BY total_orders DESC LIMIT 1;
```

---

### 9. 💳 Average Order Amount per Customer
```sql
SELECT c.customer_id, c.customer_name, AVG(order_total) AS avg_order_amount 
FROM customer c
JOIN (
    SELECT ofd.order_id, ofd.customer_id, SUM(ofd.quantity * f.price_per_unit) AS order_total
    FROM order_food ofd 
    JOIN foods f ON ofd.food_id = f.food_id
    GROUP BY ofd.order_id, ofd.customer_id
) t ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.customer_name 
ORDER BY avg_order_amount DESC;
```

---

### 10. 🔁 Most Frequent Customer per Restaurant
```sql
SELECT restaurant_id, customer_id, COUNT(*) AS total_orders 
FROM order_detail
GROUP BY restaurant_id, customer_id 
ORDER BY total_orders DESC;
```

---

### 11. 📆 Total Orders Placed on Weekends
```sql
SELECT COUNT(*) AS weekend_orders 
FROM order_detail
WHERE DAYOFWEEK(order_time) IN (1, 7);
```

---

### 12. ⏱️ Average Delivery Time: Weekdays vs Weekends
```sql
SELECT
    CASE
        WHEN DAYOFWEEK(order_time) IN (1, 7) THEN 'Weekend'
        ELSE 'Weekday'
    END AS day_type,
    AVG(TIMESTAMPDIFF(MINUTE, order_time, delivered_time)) AS avg_delivery_time
FROM order_detail
GROUP BY day_type;
```

---

### 13. 💸 Top 5 Most Expensive Food Items
```sql
SELECT food_id, food_name, price_per_unit 
FROM foods 
ORDER BY price_per_unit DESC LIMIT 5;
```

---

### 14. 🍽️ Restaurant with Most Diverse Menu
```sql
SELECT r.restaurant_id, r.restaurant_name, COUNT(DISTINCT ofd.food_id) AS total_food_items
FROM restaurant r 
JOIN order_food ofd ON r.restaurant_id = ofd.restaurant_id
GROUP BY r.restaurant_id, r.restaurant_name 
ORDER BY total_food_items DESC LIMIT 1;
```

---

### 15. 💵 Total Payment Amount by Payment Type
```sql
SELECT p.payment_type, SUM(ofd.quantity * f.price_per_unit) AS total_payment_amount
FROM payment_table p
JOIN order_food ofd ON p.order_id = ofd.order_id
JOIN foods f ON ofd.food_id = f.food_id 
GROUP BY p.payment_type;
```

---

## 💡 Key Business Insights

- Identified the **top 3 most loyal customers** driving repeat orders
- Found the **highest-rated restaurant** for quality benchmarking
- Discovered the **busiest month** for order volume — useful for campaign planning
- Revealed **weekend vs weekday delivery time** differences for operational improvements
- Highlighted the **most diverse menu** restaurant — a competitive advantage indicator
- Analyzed **payment type trends** to guide payment partner decisions

---

## 🛠️ Tools Used

- **MySQL** — Query writing & execution
- **MySQL Workbench** — Database management & testing

---

## 📂 Project Structure

```
zomato-sql-sales-analysis/
│
├── README.md
├── Solved_Query.sql        # All 15 SQL queries
└── zomatodf.sql            # Database file (run this first)
```

---

## 🙋‍♂️ About

This project was built as part of a data analytics portfolio to demonstrate skills in:
- Writing complex SQL queries (JOINs, subqueries, aggregations, window functions)
- Business problem solving through data
- Deriving actionable insights from raw transactional data

---

> 📬 Connect with me on [LinkedIn](https://www.linkedin.com/in/lakshyabhatia10/) | ⭐ Star this repo if you found it useful!
