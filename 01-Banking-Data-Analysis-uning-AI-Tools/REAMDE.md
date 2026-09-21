Banking Data Analysis Dashboard — Power BI & SQL

📊 Project Overview

This project analyzes a synthetic banking dataset using SQL Server and Microsoft Power BI.

The project was designed around a realistic banking dataset containing customers, accounts, and transactions, including common data-quality issues such as mixed date formats, inconsistent text capitalization, missing values, duplicate transaction possibilities, invalid account/customer relationships, and transaction outliers.

The data was prepared and combined using SQL, then analyzed and visualized in Power BI through interactive dashboards.

🎯 Project Objectives

The main objectives of this project are to:

Analyze transaction volume and transaction values over time.

Compare Credit and Debit transaction activity.

Analyze balances across different account types.

Identify inactive accounts based on recent transaction activity.

Analyze customer distribution by gender and age group.

Examine customer transaction values.

Understand account distribution by account type.

Build a clear banking analytics dashboard using Power BI.

🛠️ Tools & Technologies

Tool / Technology

Purpose

SQL Server

Database creation, data generation, cleaning, and transformation

SQL

Data preparation, date standardization, joins, and dataset creation

Power BI

Data analysis, modeling, DAX, and dashboard development

DAX

Measures and calculated columns

Power Query

Data transformation and preparation in Power BI

🗂️ Dataset Structure

The SQL database contains three main tables:

1. Customers

Contains customer-level information such as:

Customer ID

Name

Gender

Date of Birth

Address

Email

Phone

Account ID

2. Accounts

Contains account-level information such as:

Account ID

Customer ID

Account Type

Open Date

Balance

3. Transactions

Contains transaction-level information such as:

Transaction ID

Account ID

Transaction Date

Transaction Type

Amount

Description

Currency

The transaction table is populated with 10,000 synthetic transaction records for analysis.

🧪 Data Quality Issues Included

The dataset intentionally contains several issues to simulate real-world banking data.

Customer data

Missing gender values

Missing email and address values

Mixed date formats

Inconsistent name capitalization

Account data

Different account-type capitalization such as SAVINGS, Savings, and current

Mixed date formats

Negative/outlier account balances

Invalid customer references

Transaction data

Mixed transaction-date formats

Inconsistent transaction-type capitalization such as Credit and DEBIT

Negative and positive transaction values

Large transaction outliers

Missing descriptions

Mixed currency capitalization such as usd, USD, and INR

Non-matching account IDs

Transaction IDs were not enforced as a primary key to allow duplicate-data scenarios

These issues provide a practical environment for demonstrating data cleaning and transformation.

🔄 Data Preparation Workflow

The project follows this workflow:

SQL Server Database
        ↓
Customers
Accounts
Transactions
        ↓
Data Quality Issues
        ↓
SQL Data Cleaning
        ↓
Date Standardization
        ↓
Table Joins
        ↓
CombinedBankingDataset
        ↓
Power BI
        ↓
Data Modeling & DAX
        ↓
Interactive Dashboards

The three source tables are combined using LEFT JOIN relationships between transactions, accounts, and customers.

The final dataset is created as:

CombinedBankingDataset

The combined dataset includes transaction, account, and customer attributes for analysis.

📈 Power BI Dashboard

The Power BI report contains two dashboard pages.

Page 1 — Transaction & Customer Analysis
<img width="1338" height="732" alt="Screenshot 2026-09-21 212803" src="https://github.com/user-attachments/assets/8c2e7895-2c3c-4b80-a5cd-485988b16986" />

1. Inactive Accounts by Year and Month

Visual: Area/Line Chart

Shows the number of inactive accounts over time.

An account is considered inactive when its latest transaction is older than 90 days.

2. Sum of Amount by Account Type

Visual: Column Chart

Displays the aggregated transaction amount by account type.

3. Sum of Amount by Name

Visual: Bar Chart

Shows the total transaction amount associated with each customer.

This helps identify customers with higher transaction values.

4. Monthly Transaction Amount by Month

Visual: Area/Line Chart

Shows how total transaction amount changes month by month.

This provides a view of transaction-value trends over time.

5. Count of Transactions by Transaction Type

Visual: Pie Chart

Shows the distribution of transactions between:

Credit

Debit

📊 Page 2 — Account & Customer Analysis

<img width="1310" height="712" alt="Screenshot 2026-09-21 212740" src="https://github.com/user-attachments/assets/7d89925f-b5ce-4a50-81f1-daed83364a87" />

1. Monthly Transaction Balance by Month

Visual: Area/Line Chart

Shows the monthly transaction balance trend.

2. Total Balance by Account Type

Visual: Bar Chart

Compares the total account balance across different account types.

3. Account Count by Type

Visual: Treemap

Shows the number of accounts belonging to each account type.

4. Number of Customers by Age Group

Visual: Column Chart

Groups customers into age categories and displays the number of customers in each group.

5. Customer Count by Gender

Visual: Donut Chart

Shows the distribution of customers by gender.

📐 Key DAX Measures & Calculations

The project uses DAX measures and calculated columns to support the dashboard analysis.

Transactions by Type

Transactions by Type =
COUNT(CombinedBankingDataset[TransactionID])

Used with TransactionType to analyze transaction volume by type.

Monthly Transaction Amount

Monthly Transaction Amount =
CALCULATE(
    SUM(CombinedBankingDataset[Amount]),
    ALLEXCEPT(
        CombinedBankingDataset,
        CombinedBankingDataset[TransactionDate].[Month]
    )
)

Total by Customer

Total by Customer =
CALCULATE(
    SUM(CombinedBankingDataset[Amount]),
    ALLEXCEPT(
        CombinedBankingDataset,
        CombinedBankingDataset[CustomerID]
    )
)

Average Account Balance

Average Balance =
AVERAGE(CombinedBankingDataset[Balance])

Total Balance

Total Balance =
SUM(CombinedBankingDataset[Balance])

Inactive Accounts

Inactive Accounts =
CALCULATE(
    DISTINCTCOUNT(CombinedBankingDataset[Account_AccountID]),
    FILTER(
        VALUES(CombinedBankingDataset[Account_AccountID]),
        CALCULATE(
            MAX(CombinedBankingDataset[TransactionDate])
        ) < TODAY() - 90
    )
)

Customer Count by Gender

Customer Count by Gender =
DISTINCTCOUNT(CombinedBankingDataset[CustomerID])

Customer Age

Customer Age =
DATEDIFF(
    CombinedBankingDataset[DateOfBirth],
    TODAY(),
    YEAR
)

Customer Age Group

Customer Age Group =
SWITCH(
    TRUE(),
    [Customer Age] <= 25, "≤25",
    [Customer Age] <= 35, "26-35",
    [Customer Age] <= 50, "36-50",
    "51+"
)

Account Count by Type

Account Count by Type =
COUNT(CombinedBankingDataset[Account_AccountID])

Transactions by Month

Transactions by Month =
CALCULATE(
    COUNT(CombinedBankingDataset[TransactionID]),
    ALLEXCEPT(
        CombinedBankingDataset,
        CombinedBankingDataset[TransactionDate].[Month]
    )
)

🧹 SQL Data Cleaning

Because the source data contains mixed date formats, SQL Server TRY_CONVERT() was used to safely interpret multiple date formats before standardizing them.

The cleaning process handles formats including:

MM/DD/YYYY

YYYY-MM-DD

YYYY/MM/DD

DD-MM-YYYY

DD/MM/YYYY

The cleaned values are formatted consistently as:

MM/DD/YYYY

This process was applied to:

Account opening dates

Customer dates of birth

Transaction dates

🔗 Creating the Combined Dataset

The final Power BI dataset is created by joining the three source tables.

SELECT
    t.TransactionID,
    t.AccountID AS Transaction_AccountID,
    t.TransactionDate,
    t.Type AS TransactionType,
    t.Amount,
    t.Description,
    t.Currency,
    a.AccountID AS Account_AccountID,
    a.CustomerID AS Account_CustomerID,
    a.Type AS AccountType,
    a.OpenDate,
    a.Balance,
    c.CustomerID,
    c.Name,
    c.Gender,
    c.DateOfBirth,
    c.Address,
    c.Email,
    c.Phone
INTO CombinedBankingDataset
FROM Transactions t
LEFT JOIN Accounts a
    ON t.AccountID = a.AccountID
LEFT JOIN Customers c
    ON a.CustomerID = c.CustomerID;

📌 Key Analytical Areas

The dashboard focuses on the following areas:

Transaction Analysis

Transaction amount

Transaction count

Credit vs Debit

Monthly transaction trends

Customer transaction value

Account Analysis

Account type distribution

Account balances

Average account balance

Inactive accounts

Customer Analysis

Customer count

Gender distribution

Age-group distribution

Customer transaction values

📁 Suggested Project Structure

Banking-Data-Analysis/
│
├── README.md
│
├── SQL/
│   ├── database_setup.sql
│   ├── data_cleaning.sql
│   └── combined_dataset.sql
│
├── PowerBI/
│   └── Banking_Data_Analysis.pbix
│
└── Screenshots/
    ├── dashboard-page-1.png
    └── dashboard-page-2.png

💡 Learning Outcomes

Through this project, I practiced:

Creating and working with SQL Server databases.

Creating relational tables for customers, accounts, and transactions.

Generating synthetic transaction data.

Identifying common real-world data-quality issues.

Cleaning and standardizing mixed date formats using SQL.

Joining multiple datasets using LEFT JOIN.

Creating a consolidated analytical dataset.

Building Power BI dashboards.

Creating DAX measures.

Creating calculated columns for customer age and age groups.

Using filters, aggregation, and time-based analysis.

Selecting appropriate visuals for different analytical questions.

Presenting banking data through an interactive business dashboard.

🚀 Project Highlights

10,000 synthetic transaction records

3 source tables: Customers, Accounts, Transactions

1 consolidated dataset: CombinedBankingDataset

SQL-based data preparation

Power BI data analysis and visualization

DAX measures and calculated columns

Transaction, account, and customer analysis

Two-page Power BI dashboard

Real-world-style data-quality challenges

📷 Dashboard Preview

Page 1 — Transaction & Customer Analysis



Page 2 — Account & Customer Analysis



👨‍💻 Project Focus

This project demonstrates an end-to-end analytics workflow:

Data Generation → SQL Data Preparation → Data Cleaning → Data Integration → Power BI → DAX → Dashboard → Business Analysis

It is intended to demonstrate practical skills in SQL, Power BI, data cleaning, DAX, data modeling, and business-oriented data visualization.
