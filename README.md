# Coffee Sales Data - Dashboard

### Dashboard Link : https://app.powerbi.com/groups/me/reports/42acf4f9-b3c3-4404-ae34-7fb914d19239/b6d6bb701008a2e59ac6?experience=power-bi

Coffee Sales Analysis Project

A complete end-to-end Data Analytics Project using SQL + Power BI. This README is designed so you can directly upload it to GitHub.

📌 Project Overview

This project analyzes coffee shop sales data, cleans it using SQL, and builds an interactive dashboard using Power BI. The dataset contains common real-world data quality issues such as:

Missing values

Incorrect date formats

Duplicate rows

Inconsistent product names

Mixed data types

Outliers (extreme values)

Extra spaces and casing inconsistencies

The goal of this project is to demonstrate mid-level SQL data cleaning skills and Power BI data visualization abilities.


🧹 SQL Cleaning Steps Used

Below are the exact SQL queries used to clean the dataset.

1️) Remove extra spaces and standardize casing

    UPDATE coffee_sales
    SET product = LOWER(TRIM(product));

2)Fix mixed or invalid date formats

       UPDATE coffee_sales
       SET order_date = TRY_CONVERT(DATE, order_date);

3️) Convert string numbers to numeric

    UPDATE coffee_sales
    SET price = TRY_CAST(price AS FLOAT),
    quantity = TRY_CAST(quantity AS INT);

4) Replace NULL or blank values

       UPDATE coffee_sales
       SET category = 'Unknown'
       WHERE category IS NULL OR category = '';

5️) Handle negative prices and quantities

      UPDATE coffee_sales
      SET price = ABS(price),
      quantity = ABS(quantity)
      WHERE price < 0 OR quantity < 0;
 
6️) Identify and remove duplicate transactions

    WITH ranked AS (
    SELECT *,
    ROW_NUMBER() OVER (PARTITION BY transaction_id ORDER BY    
    order_date) AS rn
    FROM coffee_sales
    )
    DELETE FROM ranked
    WHERE rn > 1;

7️) Replace inconsistent product names

    UPDATE coffee_sales
    SET product = CASE
    WHEN product IN ('capachino','cappachino','capuccino')    
    THEN 'cappuccino'
    WHEN product IN ('exprsso','expresso') THEN 'espresso'
    ELSE product
    END;

Create calculated column for total revenue
UPDATE coffee_sales

SET total_amount = price * quantity;

###  Key KPIs Calculated

Power BI and SQL were used to compute these KPIs:

#### KPI	Description

Total Revenue  	  \   SUM(price * quantity)

Total Orders    \    COUNT(DISTINCT transaction_id)

Total Orders      \    DISTINCTCOUNT(coffee_sales[transaction_id])

Total Quantity Sold	       \        SUM(quantity)

Revenue by Product	      \     Category or drink-wise revenue

Monthly Sales         \          Trend	Revenue over time

Top Customers	         \      Customers generating highest revenue

### Power BI Dashboard Actions Taken

Below are the major actions performed during Power BI development.

#### 1) Data Transformation (Power Query)

* Imported cleaned SQL dataset

* Ensured correct data types (date, decimal, whole number)

* Merged category & product tables (if applicable)

* Removed unused columns

Created hierarchical fields (Year → Month → Day)

#### 2) DAX Measures Used

Total Sales = 

    dirty_cafe_sales[Price_Per_Unit]*dirty_cafe_sales[Quantity]


Total Quantity = 

    SUM(coffee_sales[quantity])

Day_Wise_Performance =

    IF(
    WEEKDAY(dirty_cafe_sales[Transaction_Date], 2) > 5, 
    "Weekend", 
    "Weekday"
    ) 

Total Orders = 

    count('dirty_cafe_sales'[Transaction_ID])


#### 3) Visuals Created

Revenue Trend (Bar Chart) – Monthly performance

Sales by Product (Bar Chart) – Top 10 drinks

Sales by Weekend & Weekdays (Donut Chart) 

Sales by Week - Column Chart

![Coffee Sales Dashboard](https://raw.githubusercontent.com/rawdyt/Coffee-Sales-Analysis/main/Coffee%20Sales%20Analysis.png)


🚀 Skills Demonstrated

SQL data cleaning

Data type conversions

Handling missing & dirty data

Text normalization & mapping

Window functions

DAX calculations

Data modeling

Advanced Power BI visualizations

KPI development
