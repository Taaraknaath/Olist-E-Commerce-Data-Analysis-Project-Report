Olist E-Commerce Data Analysis Project Report:
Overview:
This project explored and analyzed the Olist E-Commerce dataset from Kaggle — a detailed dataset representing real Brazilian online retail transactions. The goal was to clean, organize, and analyze this data using Python (Pandas) and MySQL 8.4 to uncover meaningful business insights such as top products, high-value customers, and popular payment methods.

Objectives:
1. Data Cleaning with Python:
All raw CSV files from the Olist dataset were uploaded into Google Colab.
Using Pandas, null values were removed to ensure clean, consistent data.
Each cleaned dataset was then saved as a new CSV file for importing into MySQL.
2. Database Creation and Schema Design:
A new MySQL database named olist_db was created.
Seven tables were designed and created without foreign-key constraints to retain all rows, since the Olist dataset is not perfectly relational. The tables were:
. olist_customers
. olist_order_payments
. olist_products
. olist_sellers
. olist_orders
. olist_order_items
. product_category_name_translation
All cleaned CSV files were then imported into these tables using DBGate.
3. Data Analysis Using MySQL 8.4:
After successful import, SQL queries were executed to answer the most frequently asked business questions regarding sales, customers, and platform performance.

Key Business Insights:
1. Top-Selling Products
Products in the moveis_decoracao, cama_mesa_banho, and ferramentas_jardim categories recorded the highest sales.
2. Top-Spending Customers
The highest-spending customer recorded a purchase total of ₹13,664.08, followed by several other one-time high-value customers.
3. Top-Performing Sellers
Sellers such as 4869f7a5dfa277a7dca6462dcf3b52b2 and 53243585a1d6dc2643021fd1853d8905 generated over ₹2 lakh in revenue each.
4. Order Status Distribution
A total of 96,455 orders were successfully delivered, with only 6 orders canceled, reflecting an excellent fulfillment rate.
5. Average Items per Order
The average came to 1.14 items per order, showing that most customers purchased a single product per transaction.
6. Most Common Payment Methods
Credit cards were the most widely used payment method, followed by boleto, voucher, and debit card payments.

Tools and Technologies Used:
1. Python 3 (Pandas for data cleaning)
2. Google Colab (for code execution and file handling)
3. MySQL 8.4 (for structured data storage and analysis)
4. DBGate (for data import and management)
5. VS Code (for writing and maintaining SQL scripts and documentation)

Workflow Summary:
. Uploaded all raw CSV files from the Olist dataset into Google Colab.
. Cleaned each dataset using Pandas by removing null values and saved the cleaned files.
. Created a new MySQL database olist_db and defined seven tables.
. Imported the cleaned CSV files into MySQL using DBGate.
. Ran SQL queries to perform business analysis and interpret key results.
. Compiled the findings into a comprehensive analytical report.

Conclusion:
This project demonstrated the complete data analysis workflow—from cleaning raw data to performing SQL-based business intelligence.
The insights revealed key trends in Olist’s e-commerce operations, helping to understand product demand, seller performance, customer spending, and payment preferences.
Overall, the project highlights the combined use of Python and MySQL to transform raw data into actionable insights.

Dataset Source:
Olist Store Dataset — Kaggle
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

Author:
Taarak Naath Vallur
Data Analyst | SQL & Python Enthusiast
