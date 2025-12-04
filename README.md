# 📊 Sales Insights Project – AtliQ Hardware

A Complete End-to-End Data Analysis Project using SQL & Power BI

## 📍 Table of Contents

- Problem Statement

- Data Discovery

- Data Analysis using MySQL

- Data Cleaning and ETL

- Data Modeling

- Data Analysis (DAX)

- Dashboard / Report

- Tools and Technologies

- References

## 🚩 Problem Statement

In this project, we analyze AtliQ Hardware, an India-based company that supplies computer hardware and peripherals to clients across India.

The Sales Director is facing several challenges:

The market is growing dynamically.

Sales are declining overall.

Regional managers provide only verbal updates with no data-backed insights.

Decisions are made without factual evidence.

To solve this, the Sales Director wants a daily-use data visualization dashboard to:

- Track sales performance

- Identify problems early

- Make data-driven decisions

- Improve efficiency and reduce losses

This project builds a complete Sales Insights dashboard using Power BI with SQL as the backend.

## 🔍 Data Discovery
Project Planning using AIMS Grid -
It is a project management tool which consists of four components-

- Purpose - (What to do exactly)
- Stackholder - (Who will be involved)
- End result - (What do you want to achieve)
- Success Criteria - (Cost optimization and time save)

AIMS Grid – Project Planning
1. Purpose

To unlock previously unseen sales insights for decision support and automate reporting to reduce manual effort.

2. Stakeholders

- Sales Director

- Marketing Team

- Customer Service Team

- Data & Analytics Team

- IT Team

3. End Result

An automated Power BI dashboard providing quick and latest sales insights for data-driven decision-making.

4. Success Criteria

- Dashboard uncovering meaningful sales insights

- 10% cost savings through better decisions

- 20% time saved by eliminating manual data gathering

- Sales team reinvests saved time into value-added tasks

## 🗄 Data Analysis using MySQL
Step 1: Import Data

Import db_dump.sql into MySQL Workbench.

Step 2: Explore Tables

Example queries:

SELECT * FROM sales.customers;
SELECT COUNT(*) FROM sales.customers;

Example Analyses
1. Chennai Market Transactions
SELECT * FROM sales.transactions WHERE market_code = 'Mark001';

2. Distinct Product Codes — Chennai
SELECT DISTINCT product_code 
FROM sales.transactions 
WHERE market_code = 'Mark001';

3. Fetch USD Transactions
SELECT * FROM sales.transactions WHERE currency = "USD";

4. Join with Date Table (2020)
SELECT t.*, d.* 
FROM sales.transactions t
JOIN sales.date d 
ON t.order_date = d.date
WHERE d.year = 2020;

5. Total Revenue 2020
SELECT SUM(t.sales_amount)
FROM sales.transactions t
JOIN sales.date d ON t.order_date = d.date
WHERE d.year = 2020 
AND (t.currency = "INR\r" OR t.currency = "USD\r");

## Data Observations and ETL (Extract, Transform, Load):
<img width="1134" height="672" alt="Screenshot 2025-12-03 202401" src="https://github.com/user-attachments/assets/6d156bdc-375a-45e0-98db-d54432fbd214" />

Market table contains garbage values

Transactions table has negative amounts

Some values in USD → must convert to INR

Duplicate currency values (INR & INR\r)

- 🧹 Data Cleaning and ETL

Performed in Power Query Editor (inside Power BI).

- ✔ Cleaning Steps

Removed null / blank rows

Filtered negative and zero sales values

Removed garbage market entries

- ✔ Currency Conversion (USD → INR)

Added a conditional column:

= Table.AddColumn(#"Filtered Rows", "norm_sales_amount",
each if [currency] = "USD" 
then [sales_amount] * 75 
else [sales_amount])

- ✔ Handling Duplicates

Checked duplicate currency entries in SQL:

SELECT COUNT(*) FROM sales.transactions WHERE currency = 'INR\r';
SELECT COUNT(*) FROM sales.transactions WHERE currency = 'INR';


- Kept only:

INR\r

USD\r

- Removed:

INR

USD

## 🧩 Data Modeling

After cleaning, The sales insights data tables as show below:
<img width="1367" height="615" alt="image" src="https://github.com/user-attachments/assets/4d2cb9de-47c1-45bb-bc2b-baea93af5a0d" />


Fact Table: sales transactions

Dimension Tables:

- sales customers

- sales date

- sales products

- sales markets

This ensures optimized DAX performance and clean relationships.

## 📐 Data Analysis (DAX)
Key Measures: 
- Revenue = SUM('sales transactions'[sales_amount])

- Sales Quantity = SUM('sales transactions'[sales_qty])

- Total Profit Margin = SUM('Sales transactions'[Profit_Margin])

- Profit Margin % = DIVIDE([Total Profit Margin], [Revenue], 0)

- Revenue Contribution % =
DIVIDE(
    [Revenue],
    CALCULATE([Revenue], ALL('sales products'), ALL('sales customers'), ALL('sales markets'))
)

- Profit Margin Contribution % =
DIVIDE(
    [Total Profit Margin],
    CALCULATE([Total Profit Margin], ALL('sales products'), ALL('sales customers'), ALL('sales markets'))
)

- Revenue LY =CALCULATE([Revenue], SAMEPERIODLASTYEAR('sales date'[date]))

Target Measures:
- Profit Target1 = GENERATESERIES(-0.05, 0.15, 0.01)

- Profit Target Value = SELECTEDVALUE('Profit Target1'[Profit Target])

- Target Diff = [Profit Margin %] - 'Profit Target1'[Profit Target Value]

## 📊 Dashboard / Report

The Power BI dashboard includes:

1. Key Insights

- Total Revenue

- Total Quantity

- Revenue by Customer

- Revenue by Market

- Yearly Trends

2. Profit Analysis

- Profit Margin %

- Profit Contribution by Product

- Target vs Actual Profit

Dashboard pages include:

- Key Insights

- Sales Performance

- Profit Analysis

Data visualization for the data analysis (DAX) was done in Microsoft Power BI Desktop:

Shows visualizations from Sales insights :

Key Insights Dashboard:
<img width="1330" height="746" alt="Screenshot 2025-12-04 103428" src="https://github.com/user-attachments/assets/bd3bf962-ef60-4f92-ab7d-6a5981c8e96e" />

Profit Analysis Dashbaord:
<img width="1340" height="746" alt="Screenshot 2025-12-04 103501" src="https://github.com/user-attachments/assets/67700205-e57e-45eb-829d-2959b969df4a" />

Performance Analysis Dashboard:
<img width="1330" height="743" alt="Screenshot 2025-12-04 103518" src="https://github.com/user-attachments/assets/deb18800-8c07-4eb2-bd4d-df978bc25a99" />


## 🛠 Tools and Technologies

- MySQL

- Power BI Desktop

- Power Query Editor

- DAX (Data Analysis Expressions)

## 📚 References

- https://codebasics.io/panel/webinars/purchases

- https://www.sqlbi.com/learn/introducing-dax-video-course/0/

- https://dev.mysql.com/doc/
