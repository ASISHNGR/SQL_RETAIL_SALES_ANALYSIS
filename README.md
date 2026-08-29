# 🛍️ Retail Sales Analysis Using SQL

## 📌 Project Overview

**This project is a Retail Sales Analysis project built using SQL to analyze retail transaction data and extract meaningful business insights.**

**The analysis covers the complete workflow of a SQL-based data analytics project, starting from database and table creation, data validation and cleaning, exploratory data analysis, and finally business-oriented analysis.**

**The primary objective of this project is to understand sales performance, customer behavior, purchasing patterns, product category performance, gender-wise purchasing behavior, and sales trends across different time periods.**


## 🎯 Project Objectives

### The major objectives of this project are:

**1.Analyze overall retail sales performance**

**2.Understand customer demographics and purchasing behavior**

**3.Compare purchasing patterns between male and female customers**

**4.Identify the best-performing product categories**

**5.Analyze sales trends by month and year**

**6.Identify the best-selling month in each year**

**7.Find the top customers based on total spending**

**8.Analyze sales and customer behavior across different time shifts**

**9.Identify peak shopping periods**

**10.Understand category preferences across genders and time shifts**

**11.Clean and validate the retail transaction dataset using SQL**




## 🗂️ Dataset Structure

The project uses a `RETAIL_SALES_ANALYSIS` table containing the following columns:

| Column | Data Type | Description |
|---|---|---|
| `transactions_id` | INT | Unique transaction identifier |
| `sale_date` | DATE | Date of the transaction |
| `sale_time` | TIME | Time of the transaction |
| `customer_id` | INT | Unique customer identifier |
| `gender` | VARCHAR | Gender of the customer |
| `age` | INT | Age of the customer |
| `category` | VARCHAR | Product category |
| `quantity` | INT | Quantity purchased |
| `price_per_unit` | FLOAT | Price of one unit |
| `cogs` | FLOAT | Cost of goods sold |
| `total_sale` | FLOAT | Total value of the transaction |

🛠️ Technologies Used
SQL
PostgreSQL / SQL-based relational database
Database & Table Creation
Data Cleaning
Data Exploration
Data Aggregation
Business Analysis
SQL Concepts Used
CREATE DATABASE
CREATE TABLE
SELECT
WHERE
DISTINCT
COUNT()
SUM()
AVG()
MAX()
MIN()
GROUP BY
ORDER BY
HAVING
CASE WHEN
EXTRACT()
TO_CHAR()
ROUND()
LIMIT
WITH / CTE
Subqueries
COUNT(DISTINCT)
Window Functions
RANK()
```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
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
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
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

## Conclusion

This project focuses on performing a comprehensive Retail Sales Analysis using SQL to uncover meaningful insights from retail transaction data. The analysis explores sales trends, customer purchasing behavior, gender-based purchasing patterns, time-based sales performance, and monthly/yearly revenue trends.

The primary objective of this project is to transform raw retail transaction data into actionable business insights that can help understand customer behavior, identify high-performing periods, and support data-driven business decisions.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: First set up a database on any SQL platform like PostgreSQL , MySQL and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `RETAIL_SALE_ANALYSIS.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Asish Kumar Sahoo

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

