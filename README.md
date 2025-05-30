# Retail_Sales-Analysis_SQL-P1
## Project Overview

**Project Title**: Retail Sales Analysis   
**Database**: `sql_project1`

This project is made to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries.
## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_project1`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_project1;
USE sql_project1;
CREATE TABLE retail_sales (
    transaction_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales 
WHERE transaction_id = '' OR sale_date IS NULL OR sale_time = '' OR customer_id = '' OR gender = '' OR category = '' OR quantity = '' OR price_per_unit = '' OR cogs = '' OR total_sale = '';

DELETE FROM retail_sales
WHERE transaction_id = '' OR sale_date IS NULL OR sale_time = '' OR customer_id = '' OR gender = '' OR category = '' OR quantity = '' OR price_per_unit = '' OR cogs = '' OR total_sale = '';
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT * FROM retail_sales 
WHERE category = 'Clothing' 
    AND 
    quantity >= 4
    AND
    sale_date BETWEEN '2022-11-01' AND '2022-11-30' 
ORDER BY transaction_id;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT category, sum(total_sale) net_sale, COUNT(*) as total_orders
FROM retail_sales 
GROUP BY 1;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT customer_id, ROUND(AVG(age), 2) avg_age, category FROM retail_sales
WHERE category = 'Beauty'
GROUP BY 1
ORDER BY 1;
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
ORDER BY transaction_id;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT gender, category, count(transaction_id) total_transactions
FROM retail_sales
GROUP BY 1,2
ORDER BY 2;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT sale_year, sale_month FROM
(
  SELECT 
    YEAR(sale_date) sale_year,
    MONTH(sale_date) sale_month, 
    ROUND(AVG(total_sale),2) avg_sales,
    RANK() OVER(PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) best_selling_month_rank
  FROM retail_sales
  GROUP BY 1, 2
) t1
WHERE best_selling_month_rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT customer_id, sum(total_sale) total_sales FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5; 
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT COUNT(DISTINCT customer_id) No_of_customers , category FROM retail_sales 
WHERE customer_id 
GROUP BY 2;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
SELECT (CASE WHEN TIME(sale_time) <= '12:00:00' THEN 'Morning' WHEN TIME(sale_time) BETWEEN '12:00:00' AND '17:00:00' THEN 'Afternoon' ELSE 'Evening' END) Shift, COUNT(customer_id) No_of_orders
FROM retail_sales
GROUP BY 1
ORDER BY 2;
```
## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.
