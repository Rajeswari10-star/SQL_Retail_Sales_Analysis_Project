# SQL_Retail_Sales_Analysis_Project
# Project Title: Retail Sales Analysis
# Level: Beginner
# Database: p1_retail_db

# This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

# Objectives:
# Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
# Data Cleaning: Identify and remove any records with missing or null values.
# Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
# Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.
# Project Structure
# 1. Database Setup
# Database Creation: The project starts by creating a database named p1_retail_db.
# Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
# CREATE DATABASE sql_project_p2;
# DROP TABLE IF EXISTS retail_sales;

# CREATE TABLE retail_sales
            (
                transaction_id INT PRIMARY KEY,	
                sale_date DATE,	 
                sale_time TIME,	
                customer_id	INT,
                gender	VARCHAR(15),
                age	INT,
                category VARCHAR(15),	
                quantity	INT,
                price_per_unit FLOAT,	
                cogs	FLOAT,
                total_sale FLOAT
            );
			
SELECT * FROM retail_sales
LIMIT 10;

SELECT COUNT(*) From retail_sales;


# ----- DATA CLEANING
SELECT * FROM retail_sales
where transaction_id is null;

SELECT * FROM retail_sales
WHERE sale_date IS NULL

SELECT * FROM retail_sales
WHERE sale_time IS NULL

SELECT * FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;

# ---- REMOVING THE NULL VALUES BY DELETING IT
DELETE FROM retail_sales
WHERE 
    transaction_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantity IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;

# ---- DATA EXPLORATION 
## -- How many sales we have?
select count(*) as total_sales from retail_sales;

## -- How many customers we have?
select count(distinct customer_id) as total_customers from retail_sales;

## -- How many categories we have?
select distinct category from retail_sales;

# -- Data Analysis & Business Key Problems & Answers
# -- My Analysis & Findings
#### -- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05

#### -- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 10 in the month of Nov-2022

#### -- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.

#### -- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.

#### -- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.

#### -- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.

#### -- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

#### -- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 

#### -- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.

#### -- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)

## -- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'?
select * from retail_sales where sale_date = '2022-11-05';

## -- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022
select *
from retail_sales 
where category='Clothing'
and quantity >= 4
and to_char(sale_date,'YYYY-MM')='2022-11';

## -- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
select category,
sum(total_sale)
from retail_sales
group by category;

## -- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
select
round(avg(age),2) as avggg
from retail_sales
where category='Beauty';

## -- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
select * from retail_sales where total_sale > 1000;

## -- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
select 
gender,
category,
count(transaction_id) 
from retail_sales
group by 1,2
order by 1;

## -- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
select * from (
select 
extract(YEAR from sale_date) as year,
extract(MONTH from sale_date) as month,
avg(total_sale) as total_sales,
rank()over (partition by extract(YEAR from sale_date) order by avg(total_sale)desc) rnk
from retail_sales
group by 1,2
) t1
where rnk=1;

## -- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales 
select 
customer_id,
sum(total_sale) as total_sales 
from retail_sales
group by 1
order by total_sales desc 
limit 5;

## -- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
select 
category,
count(distinct customer_id) as unque
from retail_sales
group by 1;

## -- Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
With Hourly_Sales as(
select *,
case 
    when extract(HOUR from sale_time) < 12 then 'Morning'
	when extract(HOUR from sale_time) between 12 and 17 then 'Afternoon'
	else 'Evening'
end as shift
from retail_sales)

select
shift,
count(*) as total_orders
from Hourly_Sales
group by 1;

-- END OF PROJECT --

# Findings
## Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
## High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
## Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
## Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.

# Reports
## Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
## Trend Analysis: Insights into sales trends across different months and shifts.
## Customer Insights: Reports on top customers and unique customer counts per category.

# Conclusion: This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.
