# 🛍️ Retail Sales Analysis Using SQL

![SQL](https://img.shields.io/badge/Analysis-100%25%20SQL-blue)
![Database](https://img.shields.io/badge/Database-PostgreSQL-orange)
![Project](https://img.shields.io/badge/Project-Retail%20Sales%20Analysis-green)

---
## 📌 Project Overview

**This project is a Retail Sales Analysis project built using SQL to analyze retail transaction data and extract meaningful business insights.**

**The analysis covers the complete workflow of a SQL-based data analytics project, starting from database and table creation, data validation and cleaning, exploratory data analysis, and finally business-oriented analysis.**

**The primary objective of this project is to understand sales performance, customer behavior, purchasing patterns, product category performance, gender-wise purchasing behavior, and sales trends across different time periods.**

---
## 🎯 Project Objectives

- 📊 Analyze overall retail sales performance
- 👥 Understand customer purchasing behavior
- 👨‍🦱👩‍🦰 Compare purchasing behavior between different genders
- 🛒 Identify the best-performing product categories
- 📅 Analyze monthly and yearly sales trends
- 📈 Identify the best-performing month in each year
- 💰 Identify the top customers based on total spending
- 🕐 Analyze sales according to different time shifts
- 🔍 Identify peak shopping periods
- 📊 Understand category preferences based on gender and time
- 💡 Generate meaningful business insights from transactional data

---

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


---
## 🛠️ Technologies & SQL Concepts Used

| Category | Technologies / Concepts |
|---|---|
| Database | SQL |
| Data Analysis | Data Cleaning, Data Validation, Data Aggregation |
| Filtering | `WHERE`, `HAVING` |
| Aggregation | `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()` |
| Sorting | `ORDER BY` |
| Conditional Logic | `CASE WHEN` |
| Date Analysis | `EXTRACT()`, `TO_CHAR()` |
| Advanced SQL | CTEs, Subqueries, Window Functions |
| Ranking | `RANK()` |

---
## 🔄 Project Workflow

```text
Raw Retail Data
       ↓
Database & Table Creation
       ↓
Data Import
       ↓
Data Validation
       ↓
Data Cleaning
       ↓
Exploratory Data Analysis
       ↓
Sales Analysis
       ↓
Customer Behavior Analysis
       ↓
Gender-Based Analysis
       ↓
Monthly & Yearly Analysis
       ↓
Time-Shift Analysis
       ↓
Business Insights
```
---

# 🗄️ 1. Database & Table Creation

## Create Database

```sql
CREATE DATABASE Retail_Sales_Analysis;
```

## Create Retail Sales Table

```sql
CREATE TABLE RETAIL_SALES
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

# 🔍 2. Data Exploration

## Fetch All Records

```sql
SELECT * 
FROM RETAIL_SALES;
```

## Count Total Records

This query was used to validate whether the records were imported correctly.

```sql
SELECT COUNT(*)
FROM RETAIL_SALES;
```

## Find Available Product Categories

```sql
SELECT DISTINCT CATEGORY
FROM RETAIL_SALES;
```

---

# 🧹 3. Data Validation & Cleaning

## Check Missing Records

The following query checks for missing values across the important columns.

```sql
SELECT *
FROM RETAIL_SALES
WHERE TRANSACTIONS_ID IS NULL
   OR SALE_DATE IS NULL
   OR SALE_TIME IS NULL
   OR CUSTOMER_ID IS NULL
   OR GENDER IS NULL
   OR AGE IS NULL
   OR CATEGORY IS NULL
   OR QUANTITY IS NULL
   OR PRICE_PER_UNIT IS NULL
   OR COGS IS NULL
   OR TOTAL_SALE IS NULL;
```

## Check Duplicate Transactions

```sql
SELECT TRANSACTIONS_ID,
       COUNT(TRANSACTIONS_ID) AS COUNT_TRANSACTIONS_ID
FROM RETAIL_SALES
GROUP BY TRANSACTIONS_ID
HAVING COUNT(TRANSACTIONS_ID) > 1;
```

## Remove Missing Records

```sql
DELETE FROM RETAIL_SALES
WHERE TRANSACTIONS_ID IS NULL
   OR SALE_DATE IS NULL
   OR SALE_TIME IS NULL
   OR CUSTOMER_ID IS NULL
   OR GENDER IS NULL
   OR AGE IS NULL
   OR CATEGORY IS NULL
   OR QUANTITY IS NULL
   OR PRICE_PER_UNIT IS NULL
   OR COGS IS NULL
   OR TOTAL_SALE IS NULL;
```

## Remove Duplicate Records

```sql
DELETE FROM RETAIL_SALES
WHERE TRANSACTIONS_ID IN (
    SELECT TRANSACTIONS_ID
    FROM RETAIL_SALES
    GROUP BY TRANSACTIONS_ID
    HAVING COUNT(TRANSACTIONS_ID) > 1
);
```

---

# 📊 4. Data Analysis & Business Questions

The following SQL queries were used to answer real-world retail business questions.

---

## 1️⃣ Sales on a Specific Date

### Business Question

**Retrieve all sales made on `2022-11-05`.**

```sql
SELECT *
FROM RETAIL_SALES
WHERE SALE_DATE = '2022-11-05';
```

---

## 2️⃣ Clothing Transactions in November 2022

### Business Question

**Retrieve all Clothing transactions where quantity sold was greater than 3 during November 2022.**

```sql
SELECT *
FROM RETAIL_SALES
WHERE CATEGORY = 'Clothing'
  AND QUANTITY > 3
  AND TO_CHAR(SALE_DATE, 'YYYY-MM') = '2022-11';
```

---

## 3️⃣ Total Sales by Category

### Business Question

**Calculate the total sales generated by each product category.**

```sql
SELECT CATEGORY,
       SUM(TOTAL_SALE) AS TOTAL_SALE
FROM RETAIL_SALES
GROUP BY 1;
```

---

## 4️⃣ Youngest Customer

### Business Question

**Retrieve all data belonging to the youngest customer.**

```sql
SELECT *
FROM RETAIL_SALES
WHERE AGE IN (
    SELECT MIN(AGE)
    FROM RETAIL_SALES
);
```

---

## 5️⃣ Customers by Gender

### Business Question

**Find the number of male and female customers.**

```sql
SELECT GENDER,
       COUNT(GENDER) AS CUST_GENDER
FROM RETAIL_SALES
GROUP BY GENDER;
```

---

## 6️⃣ Maximum Age of Female Customers

### Business Question

**Find the maximum age among female customers.**

```sql
SELECT MAX(AGE) AS AGE
FROM RETAIL_SALES
WHERE GENDER = 'Female';
```

---

## 7️⃣ Average Age of Beauty Customers

### Business Question

**Find the average age of customers who purchased from the Beauty category.**

```sql
SELECT ROUND(AVG(AGE)) AS AVERAGE_AGE
FROM RETAIL_SALES
WHERE CATEGORY = 'Beauty';
```

---

## 8️⃣ High-Value Transactions

### Business Question

**Find all transactions where total sales were greater than 1000.**

```sql
SELECT *
FROM RETAIL_SALES
WHERE TOTAL_SALE > 1000;
```

---

## 9️⃣ Gender-Wise Transactions by Category

### Business Question

**Find the total number of transactions made by each gender in each category.**

```sql
SELECT CATEGORY,
       GENDER,
       COUNT(*) AS NO_OF_TRANSACTION
FROM RETAIL_SALES
GROUP BY CATEGORY, GENDER
ORDER BY 1;
```

---

## 🔟 Average Sales by Month & Best-Selling Month

### Business Question

**Calculate the average sale for each month and identify the best-selling month in each year.**

```sql
SELECT *
FROM (
    SELECT
        EXTRACT(YEAR FROM SALE_DATE) AS YEAR,
        EXTRACT(MONTH FROM SALE_DATE) AS MONTH,
        AVG(TOTAL_SALE) AS AVG_SALE,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM SALE_DATE)
            ORDER BY AVG(TOTAL_SALE) DESC
        ) AS RANK
    FROM RETAIL_SALES
    GROUP BY 1, 2
)
WHERE RANK = 1;
```

### SQL Concepts Used

- `EXTRACT()`
- `AVG()`
- `GROUP BY`
- `RANK()`
- Window Functions
- Subquery

---

## 1️⃣1️⃣ Top 5 Customers

### Business Question

**Find the top 5 customers based on the highest total sales.**

```sql
SELECT CUSTOMER_ID,
       SUM(TOTAL_SALE) AS TOTAL_SALE
FROM RETAIL_SALES
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

---

## 1️⃣2️⃣ Unique Customers by Category

### Business Question

**Find the number of unique customers who purchased items from each category.**

```sql
SELECT CATEGORY,
       COUNT(DISTINCT CUSTOMER_ID) AS UNIQUE_CUSTOMERS
FROM RETAIL_SALES
GROUP BY 1;
```

---

# 🕐 5. Time-Shift Analysis

The project divides customers into three time shifts based on their purchase time.

| Time | Shift |
|---|---|
| Before 12 PM | 🌅 Morning |
| 12 PM – 5 PM | ☀️ Afternoon |
| After 5 PM | 🌙 Evening |

The `CASE WHEN` statement is used to classify transactions into different shifts.

---

## 1️⃣3️⃣ Number of Orders by Time Shift

### Business Question

**Create time shifts and calculate the number of orders in each shift.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       COUNT(*)
FROM HOURLY_SALE
GROUP BY TIME_SHIFT;
```

---

## 1️⃣4️⃣ Time Shift with Highest Number of Orders

### Business Question

**Identify the time shift with the highest number of orders.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       COUNT(*) AS NO_OF_ORDERS
FROM HOURLY_SALE
GROUP BY TIME_SHIFT
ORDER BY 2 DESC;
```

---

## 1️⃣5️⃣ Time Shift with Highest Total Sales

### Business Question

**Identify the time shift that generates the highest total sales.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       SUM(TOTAL_SALE) AS TOTAL_SALE
FROM HOURLY_SALE
GROUP BY TIME_SHIFT
ORDER BY 2 DESC;
```

---

## 1️⃣6️⃣ Time Shift with Highest Unique Customers

### Business Question

**Identify the time shift with the highest number of unique customers.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       COUNT(DISTINCT CUSTOMER_ID) AS UNIQUE_CUSTOMERS
FROM HOURLY_SALE
GROUP BY TIME_SHIFT
ORDER BY 2 DESC;
```

---

# 👨‍🦱👩‍🦰 6. Gender & Time-Shift Analysis

The project combines **gender analysis and time-shift analysis** to understand customer behavior in greater detail.

---

## 1️⃣7️⃣ Transactions by Gender in Each Time Shift

### Business Question

**Find the total number of transactions made by each gender in each time shift.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       GENDER,
       COUNT(*) AS NO_OF_TRANSACTION
FROM HOURLY_SALE
GROUP BY TIME_SHIFT, GENDER;
```

---

## 1️⃣8️⃣ Highest Sale by Gender in Each Shift

### Business Question

**Find the highest total sale made by each gender in each time shift.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       GENDER,
       MAX(TOTAL_SALE) AS HIGHEST_TOTAL_SALE
FROM HOURLY_SALE
GROUP BY TIME_SHIFT, GENDER
ORDER BY 1, 2;
```

---

# 🛒 7. Category & Time-Shift Analysis

---

## 1️⃣9️⃣ Highest Transaction Category in Each Shift

### Business Question

**Find which category has the highest number of transactions in each time shift.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       CATEGORY,
       COUNT(*) AS NO_OF_TRANSACTION
FROM HOURLY_SALE
GROUP BY TIME_SHIFT, CATEGORY
ORDER BY TIME_SHIFT, COUNT(*) DESC;
```

---

## 2️⃣0️⃣ Category Preference by Gender and Time Shift

### Business Question

**Find which category is preferred by each gender in each time shift.**

```sql
WITH HOURLY_SALE AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM SALE_TIME) < 12
                THEN 'MORNING'
            WHEN EXTRACT(HOUR FROM SALE_TIME) BETWEEN 12 AND 17
                THEN 'AFTERNOON'
            ELSE 'EVENING'
        END AS TIME_SHIFT
    FROM RETAIL_SALES
)
SELECT TIME_SHIFT,
       GENDER,
       CATEGORY,
       COUNT(*) AS NO_OF_TRANSACTION
FROM HOURLY_SALE
GROUP BY TIME_SHIFT, GENDER, CATEGORY
ORDER BY TIME_SHIFT, GENDER, COUNT(*) DESC;
```

---

---
# 📈 Key Analysis Areas

This project covers the following major analytical areas:

| Analysis Area | SQL Techniques |
|---|---|
| Data Exploration | `SELECT`, `COUNT`, `DISTINCT` |
| Data Validation | `IS NULL`, `COUNT` |
| Data Cleaning | `DELETE`, Subquery |
| Sales Analysis | `SUM`, `AVG`, `GROUP BY` |
| Customer Analysis | `COUNT(DISTINCT)`, `MIN`, `MAX` |
| Gender Analysis | `GROUP BY`, `COUNT` |
| Monthly Analysis | `EXTRACT`, `AVG` |
| Yearly Analysis | `EXTRACT`, `RANK()` |
| Top Customers | `SUM`, `ORDER BY`, `LIMIT` |
| Time Analysis | `EXTRACT(HOUR)` |
| Time Shifts | `CASE WHEN` |
| Advanced Analysis | CTEs, Subqueries, Window Functions |

---
# 💡 Business Insights

The analysis helps answer important questions about:

### 👥 Customer Behavior

- Who are the youngest customers?
- How many male and female customers are present?
- Which categories attract the most unique customers?
- Who are the highest-spending customers?

### 💰 Sales Performance

- Which category generates the highest sales?
- Which transactions generate more than 1000 in sales?
- Which month performs best in each year?

### 🕐 Time-Based Behavior

- Which shift has the highest number of orders?
- Which shift generates the highest sales?
- Which shift attracts the most unique customers?

### 👨‍🦱👩‍🦰 Gender-Based Behavior

- How many transactions are made by each gender?
- Which gender has the highest transaction value during each shift?
- Which categories are preferred by each gender?

### 🛒 Product Category Behavior

- Which category has the highest number of transactions in each shift?
- Which categories are preferred by different customer groups?

---

# 📌 Business Applications

The insights generated from this project can help businesses with:

- 📦 **Inventory Planning**
- 🎯 **Targeted Marketing**
- 👥 **Customer Segmentation**
- 🕐 **Staff Scheduling**
- 📈 **Sales Planning**
- 🛍️ **Product Promotion**
- 💰 **Revenue Optimization**
- 📊 **Data-Driven Decision Making**

---

# 📁 Project Structure

```text
Retail-Sales-Analysis/
│
├── RETAIL_SALE_ANALYSIS.sql
│
└── README.md
```

---

# ▶️ How to Run the Project

### 1. Clone the Repository

```bash
git clone <https://github.com/KumarAsishSahoo/SQL_RETAIL_SALES_ANALYSIS.git>
```

### 2. Navigate to the Project

```bash
cd Retail-Sales-Analysis
```

### 3. Open the SQL File

Open:

```text
RETAIL_SALE_ANALYSIS.sql
```

using your preferred SQL environment.

### 4. Create the Database

```sql
CREATE DATABASE Retail_Sales_Analysis;
```

### 5. Create the Table

Run the `CREATE TABLE RETAIL_SALES` query from the SQL file.

### 6. Import the Dataset

Import the retail sales dataset into the `RETAIL_SALES` table.

### 7. Run the Queries

Execute the SQL queries sequentially to reproduce the analysis.

---

# 🧠 What I Learned

Through this project, I developed practical experience in:

- Writing SQL queries for real-world business problems
- Data cleaning using SQL
- Data validation
- Exploratory Data Analysis
- Customer behavior analysis
- Sales analysis
- Gender-based analysis
- Time-based analysis
- Monthly and yearly trend analysis
- CTEs
- Subqueries
- Window Functions
- `RANK()`
- Data aggregation
- Translating business questions into SQL queries
- Extracting actionable insights from transactional data

---

# 🚀 Future Improvements

This project can be further enhanced by:

- 📊 Creating an interactive **Power BI dashboard**
- 🐍 Connecting SQL with **Python**
- 👥 Performing **RFM Customer Segmentation**
- 💰 Adding profit and profit-margin analysis
- 📈 Performing Year-over-Year growth analysis
- 🔮 Building sales forecasting models
- 👤 Performing Customer Lifetime Value analysis
- 📅 Performing cohort analysis
- 🤖 Applying Machine Learning for customer prediction

---

# 👨‍💻 Author

## **Asish Kumar Sahoo**

🎯 Aspiring **Data Analyst | AI/ML Engineer**

### Technical Skills

`SQL` • `Python` • `Excel` • `Power BI` • `Data Analysis` • `Statistical Analysis`

---

# ⭐ Project Highlights

> **This project demonstrates how SQL can be used to transform raw retail transaction data into meaningful business insights by analyzing customer behavior, sales trends, product categories, gender patterns, and time-based purchasing behavior.**

If you find this project useful, consider giving the repository a ⭐ **Star**!
This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

