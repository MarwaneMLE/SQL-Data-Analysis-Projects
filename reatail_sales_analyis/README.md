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
