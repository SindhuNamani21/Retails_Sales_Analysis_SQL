# Retail Sales Analysis using SQL

## 📌 Project Overview

This project focuses on analyzing retail sales data using SQL to uncover business insights, customer behavior, sales trends, and product performance.

The analysis includes:

* Data cleaning
* Data exploration
* Business problem solving using SQL queries
* Sales trend analysis
* Customer segmentation insights

The project demonstrates practical SQL skills commonly used in Data Analyst and Business Intelligence roles.

---

## 🛠️ Technologies Used

* SQL
* PostgreSQL / MySQL (queries are mostly PostgreSQL compatible)
* CSV Dataset

---

## 📂 Database Setup

### Create Database

```sql
CREATE DATABASE RETAILS_SALES_ANALYSIS_PROJECT;
```

### Create Table

```sql
CREATE TABLE retail_sales_tb (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(15),
    age INT,
    category VARCHAR(15),
    quantiy INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);
```

---

## 📊 Dataset Information

The dataset contains retail transaction records with the following attributes:

| Column Name     | Description           |
| --------------- | --------------------- |
| transactions_id | Unique transaction ID |
| sale_date       | Date of sale          |
| sale_time       | Time of sale          |
| customer_id     | Unique customer ID    |
| gender          | Customer gender       |
| age             | Customer age          |
| category        | Product category      |
| quantiy         | Quantity purchased    |
| price_per_unit  | Price per unit        |
| cogs            | Cost of goods sold    |
| total_sale      | Total sales amount    |

---

## 🧹 Data Cleaning

The project includes:

* Checking for NULL values
* Removing incomplete records
* Validating transaction data

Example:

```sql
DELETE FROM retail_sales_tb
WHERE age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

---

## 🔍 Data Exploration

Key exploratory analysis performed:

* Total number of sales
* Total customers
* Unique customers
* Product categories available

Example:

```sql
SELECT COUNT(DISTINCT customer_id)
FROM retail_sales_tb;
```

---

# 📈 Business Problems & SQL Solutions

## 1. Sales on a Specific Date

Retrieve all sales made on `2022-11-05`.

```sql
SELECT *
FROM retail_sales_tb
WHERE sale_date = '2022-11-05';
```

---

## 2. Clothing Sales in November 2022

Find Clothing category transactions with quantity greater than or equal to 4.

```sql
SELECT *
FROM retail_sales_tb
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantiy >= 4;
```

---

## 3. Total Sales by Category

Calculate total revenue generated per category.

```sql
SELECT category,
       SUM(total_sale)
FROM retail_sales_tb
GROUP BY category;
```

---

## 4. Average Age of Beauty Customers

Find average age of customers purchasing Beauty products.

```sql
SELECT category,
       ROUND(AVG(age), 2) AS avg_age
FROM retail_sales_tb
WHERE category = 'Beauty'
GROUP BY category;
```

---

## 5. High Value Transactions

Find transactions where total sales exceeded 1000.

```sql
SELECT transactions_id,
       total_sale
FROM retail_sales_tb
WHERE total_sale > 1000;
```

---

## 6. Transactions by Gender and Category

Count transactions grouped by gender and category.

```sql
SELECT category,
       gender,
       COUNT(transactions_id) AS total_transactions
FROM retail_sales_tb
GROUP BY category, gender
ORDER BY category;
```

---

## 7. Best Selling Month Each Year

Identify the highest average sales month in each year.

```sql
SELECT *
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale),
        RANK() OVER(
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rank
    FROM retail_sales_tb
    GROUP BY 1,2
) AS t1
WHERE rank = 1;
```

---

## 8. Top 5 Customers by Sales

Find customers with the highest total purchases.

```sql
SELECT customer_id,
       SUM(total_sale)
FROM retail_sales_tb
GROUP BY customer_id
ORDER BY 2 DESC
LIMIT 5;
```

---

## 9. Unique Customers by Category

Count distinct customers in each category.

```sql
SELECT COUNT(DISTINCT customer_id),
       category
FROM retail_sales_tb
GROUP BY category;
```

---

## 10. Sales Shift Analysis

Categorize sales into Morning, Afternoon, and Evening shifts.

```sql
WITH Hourly_sales AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales_tb
)

SELECT shift,
       COUNT(*)
FROM Hourly_sales
GROUP BY shift;
```

---

# 📌 Key Insights

* Clothing and Beauty categories contribute significantly to sales.
* Certain months consistently generate higher average revenue.
* Customer purchasing behavior varies across time shifts.
* High-value customers can be identified for loyalty programs.

---

# 🚀 Future Improvements

* Build interactive dashboards using Power BI or Tableau
* Add advanced SQL analytics
* Perform customer segmentation
* Predict future sales using Machine Learning
* Automate ETL pipeline

---

# 📁 Project Structure

```bash
Retail-Sales-Analysis/
│
├── dataset/
│   └── retail_sales.csv
│
├── sql_queries/
│   └── retail_sales_analysis.sql
│
└── README.md
```

---

# ⭐ Learning Outcomes

Through this project, I practiced:

* SQL querying
* Window functions
* Aggregate functions
* Common Table Expressions (CTEs)
* Data cleaning techniques
* Business-oriented data analysis

---

# 👨‍💻 Author

**Sindhu Namani**

* Master’s in Information Systems
* Aspiring Data Analyst / Business Analyst
* Skilled in SQL, Python, Power BI, Tableau, and Data Analytics
