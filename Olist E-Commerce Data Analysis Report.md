Olist E-commerce Data Analysis Report

Author: Taarak
Dataset: Olist Kaggle Dataset
Tools Used: Python (Pandas in Google Colab), MySQL 8.4, DBGate

1. Project Overview

Objective: To perform a complete data analysis of the Olist e-commerce dataset, including data cleaning, database preparation, and business insights extraction.

The project workflow included:

Cleaning and preprocessing raw CSV data using Python (Pandas)

Creating database tables in MySQL 8.4 without foreign keys

Importing cleaned data using DBGate

Analyzing business-critical questions with SQL

The analysis preserved data integrity while answering frequent business questions.

2. Data Cleaning and Preprocessing (Python / Pandas)

Objective: To clean the raw CSV files and prepare them for database import without losing data unnecessarily.

Process:

All relevant CSV files were uploaded to Google Colab.

Python Pandas was used to load each CSV, remove rows with null values, and save the cleaned data as new CSV files.

Code Used:

import pandas as pd

# 1. Customers dataset
df = pd.read_csv('olist_customers_dataset.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_olist_customers_data.csv', index=False)

# 2. Products dataset
df = pd.read_csv('olist_products_dataset.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_olist_products_data.csv', index=False)

# 3. Order payments dataset
df = pd.read_csv('olist_order_payments_dataset.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_olist_order_payments_data.csv', index=False)

# 4. Sellers dataset
df = pd.read_csv('olist_sellers_dataset.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_olist_sellers_data.csv', index=False)

# 5. Product category name translation dataset
df = pd.read_csv('product_category_name_translation.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_product_category_name_translation_data.csv', index=False)

# 6. Orders dataset
df = pd.read_csv('olist_orders_dataset.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_olist_orders_data.csv', index=False)

# 7. Order items dataset
df = pd.read_csv('olist_order_items_dataset.csv')
df_cleaned = df.dropna()
df_cleaned.to_csv('cleaned_olist_order_items_data.csv', index=False)

Notes:

. Only rows with null values were removed.

. Cleaned CSVs were ready for MySQL import via DBGate.

3. MySQL Database and Table Creation

Objective: To create a structured relational database for Olist data, ensuring all cleaned data could be stored without data loss.

Query: Database Creation

CREATE DATABASE olist_db;
USE olist_db;

All subsequent tables were created within the olist_db database. No foreign keys were added to preserve all dataset rows.

3.1 Customers Table

Objective: To store customer-related information.

Query:

CREATE TABLE `olist_customers` ( 
  `customer_id` VARCHAR(255) NOT NULL,
  `customer_unique_id` VARCHAR(255) NOT NULL,
  `customer_zip_code_prefix` VARCHAR(10),
  `customer_city` VARCHAR(255),
  `customer_state` VARCHAR(2),
  PRIMARY KEY (`customer_id`)
);

3.2 Orders Table

Objective: To store order-level information including timestamps and status.

Query:

CREATE TABLE `olist_orders` (
  `order_id` VARCHAR(50) NOT NULL,
  `customer_id` VARCHAR(50),
  `order_status` VARCHAR(50),
  `order_purchase_timestamp` DATETIME,
  `order_approved_at` DATETIME,
  `order_delivered_carrier_date` DATETIME,
  `order_delivered_customer_date` DATETIME,
  `order_estimated_delivery_date` DATETIME,
  PRIMARY KEY (`order_id`)
);

3.3 Order Items Table

Objective: To store details about individual items in each order.

Query:

CREATE TABLE `olist_order_items` (
  `order_id` VARCHAR(50) NOT NULL,
  `order_item_id` INT NOT NULL,
  `product_id` VARCHAR(50),
  `seller_id` CHAR(32),
  `shipping_limit_date` DATETIME,
  `price` DECIMAL(10,2),
  `freight_value` DECIMAL(10,2),
  PRIMARY KEY (`order_id`, `order_item_id`)
);

3.4 Order Payments Table

Objective: To record payment information for each order.

Query:

CREATE TABLE `olist_order_payments` ( 
  `order_id` VARCHAR(50) NOT NULL,
  `payment_sequential` INT NOT NULL,
  `payment_type` VARCHAR(50),
  `payment_installments` INT,
  `payment_value` DECIMAL(10,2),
  PRIMARY KEY (`order_id`, `payment_sequential`)
);

3.5 Products Table

Objective: To store product-level information.

Query:

CREATE TABLE `olist_products` ( 
  `product_id` VARCHAR(50) NOT NULL,
  `product_category_name` VARCHAR(255),
  `product_name_lenght` INT,
  `product_description_lenght` INT,
  `product_photos_qty` INT,
  `product_weight_g` INT,
  `product_length_cm` INT,
  `product_height_cm` INT,
  `product_width_cm` INT,
  PRIMARY KEY (`product_id`)
);

3.6 Sellers Table

Objective: To store seller-related information.

Query:

CREATE TABLE `olist_sellers` ( 
  `seller_id` CHAR(32) NOT NULL,
  `seller_zip_code_prefix` CHAR(8) NOT NULL,
  `seller_city` VARCHAR(100) NOT NULL,
  `seller_state` CHAR(2) NOT NULL,
  PRIMARY KEY (`seller_id`)
);

3.7 Product Category Name Translation Table

Objective: To store category name translations for standardization.

Query:

CREATE TABLE `product_category_name_translation` (  
  `product_category_name` VARCHAR(100) NOT NULL,
  `product_category_name_english` VARCHAR(100) NOT NULL,
  PRIMARY KEY (`product_category_name`)
);

4. Data Import

Objective: To load cleaned CSV files into MySQL tables for analysis.

All cleaned CSVs were imported into the MySQL database olist_db using DBGate. Table counts were verified using SELECT COUNT(*) FROM <table> for consistency.

5. Business Question Analysis (SQL)

5.1 Top-Selling Products

Objective: To identify the most sold products and their categories.

Query:

SELECT p.product_id, COUNT(oi.order_item_id) AS total_sold, p.product_category_name
FROM olist_order_items oi
JOIN olist_products p ON oi.product_id = p.product_id
GROUP BY p.product_id, p.product_category_name
ORDER BY total_sold DESC
LIMIT 10;

Output:

product_id	                     total_sold product_category_name
aca2eb7d00ea1a7b8ebd4e68314663af	527	moveis_decoracao
99a4788cb24856965c36a24e339b6058	488	cama_mesa_banho
422879e10f46682990de24d770e7f83d	484	ferramentas_jardim
389d119b48cf3043d311335e499d9c6b	392	ferramentas_jardim
368c6c730842d78016ad823897a372db	388	ferramentas_jardim
53759a2ecddad2bb87a079a1f1519f73	373	ferramentas_jardim
d1c427060a0f73f6b889a5c7c61f2ac4	343	informatica_acessorios
53b36df67ebb7c41585e8d54d6772e08	323	relogios_presentes
154e7e31ebfa092203795c972e5804a6	281	beleza_saude
3dd2a17168ec895c781a9191c1e95ad7	274	informatica_acessorios

5.2 Top Customers by Spending

Objective: To identify highest-spending customers and their order counts.

Query:

SELECT o.customer_id, SUM(op.payment_value) AS total_spent, COUNT(o.order_id) AS total_orders
FROM olist_orders o
JOIN olist_order_payments op ON o.order_id = op.order_id
GROUP BY o.customer_id
ORDER BY total_spent DESC
LIMIT 10;


Output:

customer_id	                    total_spent total_orders
1617b1357756262bfa56ab541c47bc16	13664.08 1
ec5b2ba62e574342386871631fafd3fc	7274.88	1
c6e2731c5b391845f6800c97401a43a9	6929.31	1
f48d464a0baaea338cb25f816991ab1f	6922.21	1
3fd6777bbce08a352fddd04e4a7cc8f6	6726.66	1
05455dfa7cd02f13d132aa7a6a9729c6	6081.54	1
df55c14d1476a9a3467f131269c2477f	4950.34	1
24bbf5fd2f2e1b359ee7de94defc4a15	4764.34	1
3d979689f636322c62418b6346b1c6d2	4681.78	1
1afc82cd60e303ef09b4ef9837c9505c	4513.32	1

5.3 Top Sellers by Revenue

Objective: To identify the highest revenue-generating sellers and their order counts.

Query:

SELECT oi.seller_id, SUM(oi.price) AS total_revenue, COUNT(DISTINCT oi.order_id) AS total_orders
FROM olist_order_items oi
GROUP BY oi.seller_id
ORDER BY total_revenue DESC
LIMIT 10;


Output:

seller_id	                      total_revenue total_orders
4869f7a5dfa277a7dca6462dcf3b52b2	229472.63	1132
53243585a1d6dc2643021fd1853d8905	222776.05	358
4a3ca9315b744ce9f8e9374361493884	200472.92	1806
fa1c13f2614d7b5c4749cbc52fecda94	194042.03	585
7c67e1448b00f6e969d365cea6b010ab	187923.89	982
7e93a43ef30c4f03f38b393420bc753a	176431.87	336
da8622b14eb17ae2831f4ac5b9dab84	  160236.57 1314
7a67c85e85bb2ce8582c35f2203ad736	141745.53	1160
1025f0e2d44d7041d6cf58b6550e0bfa	138968.55	915
955fee9216a65b617aa5c0531780ce60	135171.70	1287

5.4 Order Status Distribution

Objective: To analyze order completion and cancellation trends.

Query:

SELECT order_status, COUNT(*) AS total_orders
FROM olist_orders
GROUP BY order_status;


Output:

order_status total_orders
delivered	96455
canceled	6

5.5 Average Items per Order

Objective: To determine the typical number of items purchased per order.

Query:

SELECT AVG(item_count) AS avg_items_per_order
FROM (
    SELECT order_id, COUNT(*) AS item_count
    FROM olist_order_items
    GROUP BY order_id
) AS order_items_count;


Output:

avg_items_per_order
1.1417

5.6 Payment Method Usage

Objective: To identify the most popular payment methods.

Query:

SELECT payment_type, COUNT(*) AS total_payments
FROM olist_order_payments
GROUP BY payment_type
ORDER BY total_payments DESC;


Output:

payment_type total_payments
credit_card	76795
boleto	19784
voucher	5775
debit_card 1529
not_defined	3

6. Key Insights and Business Takeaways

Top Products:

Home decor, tools, and tech accessories dominated sales.

Inventory and marketing were focused on these categories.

Customer Spending:

High-spending customers mostly made single purchases.

Opportunities for retention and cross-selling were identified.

Seller Performance:

Revenue was not strictly tied to order count; high-ticket sellers dominated revenue.

Top-performing sellers were highlighted for incentives and product variety expansion.

Order Fulfillment:

Delivery rates were very high; cancellations were minimal.

Logistics processes were efficient.

Buying Patterns:

Average 1.14 items per order suggested most customers bought single items.

Cross-selling opportunities could increase order size.

Payment Preferences:

Credit cards dominated, followed by boleto.

Payment infrastructure and marketing strategies were aligned to these methods.

7. Conclusion

The project demonstrated an end-to-end data analysis workflow:

Python Pandas was used to clean and preprocess data in Google Colab.

MySQL 8.4 database olist_db and tables were created for structured storage without foreign keys to avoid losing data.

DBGate was used to import cleaned CSVs into the database.

SQL queries were executed to answer key business questions about products, customers, sellers, orders, and payments.

The dataset was fully prepared for further predictive modeling, sales forecasting, or dashboard creation.

Note: Only null values were dropped during Python cleaning; no additional transformations were applied.


