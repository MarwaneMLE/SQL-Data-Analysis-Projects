# Project Title: Retail Sales Analysis

## Project Overview

This project involves analyzing retail sales using SQL qnd data analysis techniques to answer questions based on a retail sales database through expolory data analysis(EDA), data cleaning and analyse retail sales.


## Project Steps & Objectives

1. **Set Up Retail Sales Database**: Created a database named `retail_sales_dt`.
2. **Data Cleaning**: Identified and removed duplicates, missing values, and null records.
3. **Exploratory Data Analysis (EDA)**: Performed EDA to understand the structure and contents of the database.
4. **Business Analysis**: Used SQL to answer key business questions and derive insights from the sales data to support decision-making.

## Project structure

### Database Setup

- **Database creation:** Every SQL project start with database creation, so we will creating database names `retail_sales_db`.
```sql
DROP DATABASE IF EXISTS retail_sales_db;
CREATE DATABASE retail_sales_db;
```
- **Table creation:** We create table named `retail_sales` to store the sales data. The table columns name are transactions ID, sale date, sale time, customer ID, gender, age, category , quantiy, price per unit, cogs(cost of goods sold), total sale.
```sql
CREATE TABLE retail_sales(
	transactions_id INT PRIMARY KEY,
	sale_date DATE,
	sale_time TIME,
	customer_id INT,
	gender VARCHAR(25),
	age INT,
	category VARCHAR(25),
	quantiy INT,
	price_per_unit FLOAT,
	cogs FLOAT,
	total_sale FLOAT
);
```
After creation of database ans table, we will import cvs file into our created table.

- **Verification of imported data:**
```sql
-- Verify imported data
SELECT * FROM retail_sales

SELECT COUNT(*) number_records 
FROM retail_sales;
```
The imported table contain all 2000 records.

## 1. Exploratory Data Analysis
 To understand and cover insight about the dataset
**What does each column represent?**

- **transactions_id:** A unique identifier for each transaction 
- **sale_date:** the date where transaction occured, providing insightsinto sales trends over time.
- **sale_time:** the time where transaction occured, providing insightsinto sales trends over time
- **customer_id:** A unique identifier for each customer, enabling customer-centric analysis.
- **gender:** customer gender male or female for data segmentation 
- **age:**  The age of the customer, facilitating segmentation and exploration of age-related influences.
- **category:** The category of the purchased product
- **quantiy:** The number of units of the product purchased, contributing to insights on purchase volumes
- **price_per_unit:** The price of one unit of the product, aiding in calculations related to total spending.
- **cogs:** cost of goods sold
- **total_sale:** the total amount value of the transaction, showcasing the financial impact of each purchase

#### Explore database
```sql
-- Explore all objects in the Database
SELECT * FROM INFORMATION_SCHEMA.TABLES

-- Explore all columns in the Database
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'retail_sales'
```
### Explore retal_sales table
#### What is the time period covered?
```sql
SELECT 
	MIN(sale_date) Lowest_date,
	MAX(sale_date) Highest_date
FROM retail_sales
```
-- The covered period is from 2022-01-01 to 2023-12-31.  
-- The data is aggregated daily, and each day includes different timestamps.

#### 3. Data Cleaning
-- 3.1.  Handle missing values

 
 - Check the null values in all columns 
```sql
SELECT * FROM retail_sales
WHERE 
	transactions_id IS NULL
	OR sale_date IS NULL
	OR sale_time IS NULL
	OR customer_id IS NULL
	OR gender IS NULL
	OR age IS NULL
	OR category IS NULL
	OR quantiy IS NULL
	OR category IS NULL
	OR price_per_unit IS NULL
	OR cogs IS NULL
	OR total_sale IS NULL;
```
/*
There in total 13 NULL values in for columns,
We can drop 3 rows where columns price_per_unit, cogs, total_sale becouse ther are not contain any information
For age columns we can replace them by the averge age.
*/ 
  
###  Dimensions Exploratory
-- Identify the unique values (or categories) in each dimension.
-- To recognize how data might be grouped or segmented, which is useful for analysis

##### Explore gender & category
```sql
SELECT 
	DISTINCT gender
FROM retail_sales
   
SELECT 
	DISTINCT category
FROM retail_sales
```
We have two gender and 3 categories

#### Date exploration: 
-- Find the date of the first and last order
-- Understand the scope od data and the timespan
category
```sql
SELECT 
	MIN(sale_date) AS first_sale_date,
	MAX(sale_date) AS last_sale_date,
	DATEDIFF(MONTH, MIN(sale_date), MAX(sale_date)) AS sale_range_month
FROM retail_sales
```
Find youngest and oldest customer

```sql
SELECT 
	MIN(age) AS age_youngest_customer,
	MAX(age) AS age_oldest_customer
FROM retail_sales
```

### -- ==========EDA: Measures Exploratory ======
Calculate the key metric of the business (Big Numbers). Highest level of aggregation - Lowest level of details

#### Find the numbers of sales
SELECT 
	COUNT(*) AS total_sales
FROM retail_sales

#### Find the total number of customers
SELECT 
	COUNT(DISTINCT customer_id) AS number_customers
FROM retail_sales
 There are 155 customers

#### Find total sales
SELECT 
	SUM(total_sale) AS total_sales
FROM retail_sales

#### Find average of selling price
SELECT 
	ROUND(AVG(total_sale), 2) AS average_sale_price
FROM retail_sales


#### Find Cheaper and expensive unit price
 SELECT 
	MIN(price_per_unit) AS cheaper_unit_price,
	MAX(price_per_unit) AS expensive_unit_price
 FROM retail_sales

#### Find average price per unit
SELECT 
	ROUND(AVG(price_per_unit), 2) AS avg_prive_per_unit
FROM retail_sales
 
## 2.  Data Analysis, Business Key problems and answers
In this step we will answer to the business questions

-- ================Data Analysis & Findings======================
#### 1. Write a SQL query to retrieve all columns for sales made on '2022-11-05':
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05'
```

#### 2. Find all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
```sql
SELECT 
	*
FROM retail_sales
WHERE
	category = 'Clothing'
	AND quantiy >= 4
	AND FORMAT(sale_date, 'yyyy-MM') = '2022-11'
```
There are 146 sales in month november 2022.

#### 3. Find the total sales (total_sale) for each category
```sql
SELECT 
	category,
	SUM(total_sale) total_sales
FROM retail_sales
GROUP by category
```

#### 4.Find the average age of customers who purchased items from the 'Beauty' category.
```sqlSELECT 
	ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE  category = 'Beauty'
```

#### 5. Find all transactions where the total_sale is greater than 1000.
```sql
SELECT 
	*
FROM retail_sales
WHERE total_sale >= 1000
```

#### 6. Find the total number of transactions (transaction_id) made by each gender in each category.
```sql
SELECT 
	category,
	gender,
	COUNT(transactions_id) total_number_transaction
FROM retail_sales
GROUP BY  category, gender
```

#### 7. Calculate the average sale for each month. Find out best selling month in each year
```sql
SELECT
	date_year,
	date_month,
	avg_sale
FROM
(
	SELECT
		FORMAT(sale_date, 'yyyy') AS date_year,
		FORMAT(sale_date, 'yyyy-MM') AS date_month,
		ROUND(AVG(total_sale), 2) AS avg_sale,
		RANK() OVER(PARTITION BY FORMAT(sale_date, 'yyyy') ORDER BY ROUND(AVG(total_sale), 2) DESC) month_rank
	FROM retail_sales
	GROUP BY FORMAT(sale_date, 'yyyy'), FORMAT(sale_date, 'yyyy-MM')--, ROUND(AVG(total_sale), 2)
) AS t
WHERE month_rank = 1
```
The best salling month is Julla 2022 with average sales of $541.34
 The best salling month is April 2023 with average sales of $535.53.


#### 8. Find the top 5 customers based on the highest total sales
```sql
SELECT TOP 5
	customer_id,
	SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY SUM(total_sale) DESC
```

#### 9. Find the number of unique customers who purchased items from each category.
```sql
SELECT
	category,
	COUNT(DISTINCT customer_id)  number_unique_customer
FROM retail_sales
GROUP BY  category
```

#### 10. Write a SQL query to create each shift  of sale time
-- (Example Morning <12, Afternoon Between 12 & 17, Evening >17)
```sql
SELECT 
	transactions_id,
	sale_date,
	sale_time,
	category,
	quantiy,
	total_sale,
	CASE
		WHEN sale_hour < 12 THEN 'Morining'
		WHEN sale_hour BETWEEN  12 AND 17 THEN 'Afternoon' -- BETWEEN include 12 and 17
		ELSE 'Evening'
	END AS shift_time
FROM
(
	SELECT 
		*,
		FORMAT(sale_time,'hh') AS sale_hour
	FROM retail_sales
) t
ORDER BY total_sale DESC
```

 
 
