# atliqon-sportsbar-data-platform
End-to-End Data Engineering Project using AWS S3, Databricks, PySpark, Spark SQL, Delta Lake, and Medallion Architecture

# SUMMARY
This project simulates a real-world post-merger data integration scenario.
Atliqon, a multinational sports equipment company, acquired SportsBar, a sports nutrition FMCG company.
While Atliqon already operates a mature analytics platform with standardized dimensional models, SportsBar relies on raw operational CSV files that are inconsistent, fragmented, and unsuitable for enterprise reporting.
To enable consolidated business analytics, an end-to-end data engineering platform was built using AWS S3 and Databricks.
The solution ingests raw operational data into a Medallion Architecture, performs scalable ETL using PySpark, merges parent and subsidiary datasets, creates an enterprise-ready Gold Layer, and exposes a curated SQL View for interactive dashboards.


# Bussines Challenge
| Challenge             | Impact              |
| --------------------- | ------------------- |
| Different Schemas     | Unable to Join Data |
| Missing Values        | Incorrect Reporting |
| Different Product IDs | Duplicate Products  |
| Separate CSV Files    | Difficult ETL       |
| No Data Warehouse     | Slow Reporting      |


# Data Source
| Dataset     | Company | Description       | Layer  |
| ----------- | ------- | ----------------- | ------ |
| Customers   | Parent  | Clean Dimension   | Gold   |
| Products    | Parent  | Clean Dimension   | Gold   |
| Gross Price | Parent  | Clean Dimension   | Gold   |
| Fact Orders | Parent  | Historical Sales  | Gold   |
| Customers   | Child   | Raw Master Data   | Bronze |
| Gross Price | Child   | Raw Master Data   | Bronze |
| Products    | Child   | Raw Master Data   | Bronze |
| Orders      | Child   | Incremental Files | Bronze |

#Solution Architecture
The solution implements a Medallion Architecture consisting of Bronze, Silver, and Gold layers.
Unlike a conventional pipeline, this project contains two ingestion paths.

Parent Company Pipeline
Clean Enterprise Data
↓
Gold Layer
-- Since the parent company's data is already standardized, it is loaded directly into the Gold Layer.

Child Company Pipeline
AWS S3
↓
Bronze
↓
Silver
↓
Gold
↓
Merge with Parent Gold
SportsBar's raw operational data passes through the complete ETL pipeline before being merged with Atliqon's enterprise data.


☁ AWS S3
AWS S3 acts as the landing zone for SportsBar's operational data.
The bucket stores:
Customer Data
Product Data
Pricing Data
Historical Orders
Daily Incremental Orders
Incoming CSV files are automatically extracted into Databricks for further processing.

# ETL Workflow
-- Step 1 — Extract
SportsBar datasets are uploaded into Amazon S3.
PySpark extracts every CSV file into the Bronze Layer.

-- Step 2 — Bronze Layer
Bronze stores raw immutable copies of the original data.
No business transformations are applied.
Purpose:
Data Ingestion
Raw Backup
Traceability

-- Step 3 — Silver Layer
The Silver Layer performs all cleansing and standardization processes.
Transformation includes:
Filling missing values
Standardizing column names
Data type conversion
Schema alignment
Removing duplicate records
Business rule validation

-- Step 4 — Gold Layer
Business-ready analytical tables are created.
Gold consists of:
Customer Dimension
Product Dimension
Gross Price Dimension
Date Dimension
Fact Orders
These tables follow a dimensional model suitable for BI and reporting.

-- Step 5 — Parent & Child Integration
After SportsBar reaches the Gold Layer, every Gold table is merged with the corresponding Atliqon Gold table.
Merged datasets include:
Customers
Products
Gross Pricing
Historical Orders
Incremental Orders
This creates one unified enterprise data model.

-- Step 6 — Incremental Processing
Instead of reprocessing all historical orders, newly arrived CSV files are processed incrementally.
The incremental pipeline automatically:
Reads new order files
Validates records
Appends new transactions
Updates the enterprise Gold Layer
This approach reduces processing time and supports scalable daily ingestion.

# IIncremental Loda Pipeline
AWS S3
↓
Incremental Orders File
↓
Lakeflow Job
↓
PySpark
↓
Validation
↓
Append Gold
↓
Archive

# Dashboard
The interactive dashboard provides:
KPI
Total Revenue
Total Quantity
Total Products
Total Records
Business Insights
Revenue Trend
Revenue by Channel
Top 10 Products
Bottom 5 Products
Top Division Revenue
Product Variant Analysis

Interactive filters include:
Year
Quarter
Month
Channel

# Skills Demonstrated
End-to-End Data Engineering
AWS S3 Data Lake
Medallion Architecture
PySpark ETL
Incremental Data Processing
Delta Lake
Spark SQL
Data Modeling
Star Schema
SQL View Engineering
Dashboard Development
Enterprise Data Integration
Data Quality Engineering
