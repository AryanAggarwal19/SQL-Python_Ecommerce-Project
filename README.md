
# SQL and Python E-commerce Sales Analysis Project

This project is a data analysis pipeline for an e-commerce database, utilizing SQL queries to retrieve insights, with Python for data manipulation and visualization.

## Project Setup

### Dependencies
- **Python Libraries**: `pandas`, `matplotlib`, `seaborn`, `mysql.connector`
- **Database**: MySQL

### Database Connection
```python
import mysql.connector

db = mysql.connector.connect(
    host="localhost",
    username="root",
    password="12345",
    database="ecommerce"
)
cur = db.cursor()
```

## Queries and Analysis

### 1. List Unique Customer Cities
Retrieve all unique cities where customers are located.
```python
query = "SELECT DISTINCT customer_city FROM customers"
cur.execute(query)
data = cur.fetchall()
df = pd.DataFrame(data, columns=["Customer City"])
df.head()
```

### 2. Count Orders in 2017
Count the total number of orders placed in the year 2017.
```python
query = "SELECT COUNT(order_id) FROM orders WHERE YEAR(order_purchase_timestamp) = 2017"
cur.execute(query)
data = cur.fetchall()
print("Total orders placed in 2017 are:", data[0][0])
```

### 3. Total Sales by Category
Calculate the total sales for each product category.
```python
query = '''
SELECT UPPER(products.product_category) AS category, 
ROUND(SUM(payments.payment_value), 2) AS sales
FROM products
JOIN order_items ON products.product_id = order_items.product_id
JOIN payments ON payments.order_id = order_items.order_id
GROUP BY category
'''
cur.execute(query)
data = cur.fetchall()
df = pd.DataFrame(data, columns=["Category", "Sales"])
df.head()
```

### 4. Percentage of Orders Paid in Installments
Calculate the percentage of orders paid in installments.
```python
query = '''
SELECT (SUM(CASE WHEN payment_installments >= 1 THEN 1 ELSE 0 END) / COUNT(*)) * 100
FROM payments
'''
cur.execute(query)
data = cur.fetchall()
print("Percentage of orders paid in installments:", data[0][0])
```

### 5. Customer Count by State
Count the number of customers in each state and visualize the distribution.
```python
query = "SELECT customer_state, COUNT(customer_id) FROM customers GROUP BY customer_state"
cur.execute(query)
data = cur.fetchall()
df = pd.DataFrame(data, columns=["State", "Customer Count"])
```

... (Continued with all other queries as outlined in your initial request) ...

## Visualizations
- **Customer Count by State**: A bar chart to show the number of customers from each state.
- **Orders per Month (2018)**: Monthly order count for 2018, visualized with a bar plot.
- **Sales Distribution by Category**: Sales breakdown for each category.

## Advanced Analytics
- **Moving Average of Order Values**: Computes a rolling average of order values for each customer.
- **Year-over-Year Sales Growth**: Calculates the annual growth rate in sales.
- **Customer Retention Rate**: Calculates the retention rate by identifying repeat customers within a six-month period.

## Conclusion
This project offers insights into customer distribution, sales performance across categories, monthly trends, and advanced metrics like customer retention and moving averages.

---

