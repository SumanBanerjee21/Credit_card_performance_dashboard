# 💳 Credit Card Analytics Dashboard (MySQL + Power BI)

## 📌 Project Overview

This project demonstrates an end-to-end data analytics pipeline built using **MySQL** and **Power BI**. The objective was to import raw CSV data into a relational database, clean and transform the data using SQL, and build an interactive dashboard for business insights.

The workflow covers database creation, bulk data import, data cleaning, troubleshooting technical issues, and connecting MySQL to Power BI using ODBC.

---

# 🗄 Database Setup

## Database Created

```sql
CREATE DATABASE ccdb;
USE ccdb;
```

---

# 📊 Tables

## 1️⃣ cc_detail

Stores credit card transaction data.

### Key Columns:

* `Client_Num`
* `Card_Category`
* `Week_Start_Date`
* `Credit_Limit`
* `Total_Trans_Amt`
* `Total_Trans_Ct`
* `Avg_Utilization_Ratio`
* `Interest_Earned`
* `Delinquent_Acc`

This table captures weekly transaction activity, spending trends, utilization ratio, and delinquency indicators.

---

## 2️⃣ cust_detail

Stores customer demographic data.

### Key Columns:

* `Client_Num`
* `Customer_Age`
* `Gender`
* `Education_Level`
* `Marital_Status`
* `Income`
* `Cust_Satisfaction_Score`

This table is used for customer segmentation and behavioral analysis.

---

# 📥 Data Import Process

## CSV Files Used

* `credit_card.csv`
* `customer.csv`
* `cc_add.csv`
* `cust_add.csv`

---

## 🚧 Challenges Faced During Data Import

### 1️⃣ PostgreSQL vs MySQL Syntax Issue

The tutorial referenced the PostgreSQL command:

```sql
COPY table_name
FROM 'file_path'
DELIMITER ','
CSV HEADER;
```

However, MySQL does not support the `COPY` command.

### ✅ Solution

Used MySQL equivalent:

```sql
LOAD DATA LOCAL INFILE 'file_path'
INTO TABLE table_name
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

---

### 2️⃣ Date Format Mismatch

CSV Date Format:

```
DD-MM-YYYY
```

MySQL Expected Format:

```
YYYY-MM-DD
```

This caused warnings:

```
Data truncated for column 'Week_Start_Date'
```

### ✅ Fix Applied

```sql
SET Week_Start_Date = STR_TO_DATE(@Week_Start_Date,'%d-%m-%Y');
```

---

### 3️⃣ MySQL Security Restriction

Error:

```
The MySQL server is running with the --secure-file-priv option
```

### ✅ Resolution Steps

* Enabled `local_infile=1` in `my.ini`
* Restarted MySQL service
* Used `LOAD DATA LOCAL INFILE`

---

# 🔌 Power BI Connection Challenges

## Issues Encountered

* MySQL Connector/NET not detected
* 32-bit vs 64-bit driver mismatch
* Power BI requiring additional components
* Native MySQL connector failing repeatedly

---

## ✅ Final Working Solution

Used **MySQL ODBC 9.6 (64-bit Unicode Driver)** instead of Connector/NET.

### Steps Implemented:

1. Installed MySQL ODBC 9.6 (64-bit)
2. Opened ODBC Data Source Administrator (64-bit)
3. Created System DSN named `MySQL_PowerBI`
4. Configured:

   * Server: `127.0.0.1`
   * Port: `3306`
   * Database: `ccdb`
   * User: `root`
5. Connected via:

```
Power BI → Get Data → ODBC → MySQL_PowerBI
```

---

# 📊 End-to-End Workflow

```
CSV Files
   ↓
MySQL Database (ccdb)
   ↓
Data Cleaning & Transformation (SQL)
   ↓
ODBC Connection
   ↓
Power BI Dashboard
   ↓
Business Insights
```

---

# 🎯 Key Learnings

* Difference between PostgreSQL and MySQL bulk import methods
* Handling date conversion using `STR_TO_DATE()`
* Managing MySQL server security configurations
* Resolving 32-bit vs 64-bit driver compatibility issues
* Configuring ODBC for BI tool integration
* Building an end-to-end analytics pipeline

---

# 🧠 Skills Demonstrated

* SQL (DDL & Data Import)
* Data Cleaning & Transformation
* MySQL Configuration & Troubleshooting
* ODBC Setup & Driver Management
* Data Modeling in Power BI
* Business Intelligence Dashboard Development

---

# 👨‍💻 Author

**Suman Banerjee**
B.Tech Student | Data Analytics Enthusiast

---

⭐ If you found this project useful, feel free to star the repository.
