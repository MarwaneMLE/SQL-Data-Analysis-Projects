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
  
### -- ========== Dimensions Exploratory ===========
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
 
## 2. 
