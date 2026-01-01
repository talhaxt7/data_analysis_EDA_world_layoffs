# 1. EDA Layoffs Analysis
Exploratory Data Analysis (EDA) of global layoffs data using SQL. This project analyzes trends, patterns, and insights from company layoffs across different dimensions including industry, geography, time periods, and company characteristics.
Overview
This repository contains SQL queries that perform comprehensive analysis on the world_layoffs.layoffs_staging2 dataset, examining layoff trends and patterns to answer key business questions about workforce reductions globally.
Features
The analysis covers the following dimensions:
Basic Statistics

Maximum layoffs (total and percentage) across the dataset
Companies with 100% workforce reduction
Date range coverage of the dataset

Aggregated Analysis

By Company: Total and average layoffs per company
By Industry: Industry-wide layoff trends
By Geography: Country-level layoff analysis
By Time: Yearly layoff patterns
By Stage: Layoffs by company funding stage

Advanced Analysis

Rolling Totals: Month-over-month cumulative layoffs using CTEs
Year-over-Year Rankings: Top companies with highest layoffs per year using window functions

Prerequisites

MySQL 5.7+ or compatible database
Access to the world_layoffs database with layoffs_staging2 table

Database Schema
The analysis assumes a table with the following structure:
sqlworld_layoffs.layoffs_staging2 (
    company VARCHAR,
    total_laid_off INT,
    percentage_laid_off DECIMAL,
    date DATE,
    industry VARCHAR,
    country VARCHAR,
    stage VARCHAR,
    -- additional columns...
)
Usage

Ensure you have access to the world_layoffs.layoffs_staging2 table
Execute queries sequentially or select specific analyses based on your needs
Queries are designed to run independently unless they build on CTEs

Key Queries
Rolling Total Analysis
Calculates cumulative layoffs by month using Common Table Expressions (CTEs):
sqlWITH Rolling_Total AS (...)
SELECT `MONTH`, total_off,
SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total 
FROM Rolling_Total;
Top Companies Per Year
Ranks companies by total layoffs for each year using DENSE_RANK:
sqlWITH company_year AS (...),
Company_Year_Rank AS (...)
SELECT * FROM Company_Year_rank;
Insights Generated

Companies most affected by layoffs
Industries hit hardest by workforce reductions
Geographic distribution of layoffs
Temporal trends and seasonal patterns
Company stage correlation with layoff events
Month-over-month layoff momentum

Notes

All queries use the layoffs_staging2 table, suggesting this is cleaned/processed data
The analysis handles NULL values appropriately in date and aggregation queries
Window functions and CTEs are used for complex time-series analysis




# 2. World Layoffs Analysis (2022-2023)

A comprehensive SQL-based analysis of global corporate layoffs during 2022-2023. This project includes complete data cleaning, processing, and exploratory data analysis to uncover trends and insights about workforce reductions worldwide.

## Dataset

**Source**: [Kaggle - Layoffs 2022 Dataset](https://www.kaggle.com/datasets/swaptr/layoffs-2022)

## Project Structure

This project consists of two main components:

1. **Data Cleaning & Processing** - Prepares raw data for analysis
2. **Exploratory Data Analysis (EDA)** - Analyzes cleaned data for insights

## Data Cleaning Process

### Workflow Steps

1. **Duplicate Removal**
   - Identify duplicates using `ROW_NUMBER()` window function
   - Partition by all relevant columns to find true duplicates
   - Remove duplicate entries while preserving unique records

2. **Data Standardization**
   - Clean industry classifications
   - Populate NULL industry values from matching company records
   - Standardize country names (remove trailing periods)
   - Convert date strings to proper DATE format

3. **NULL Value Handling**
   - Preserve NULLs in `total_laid_off`, `percentage_laid_off`, and `funds_raised_millions` for analytical flexibility
   - Remove records where both `total_laid_off` AND `percentage_laid_off` are NULL (unusable data)

4. **Table Optimization**
   - Remove temporary helper columns (`row_num`)
   - Create staging tables to preserve raw data

### Key Techniques Used

- Window functions (`ROW_NUMBER() OVER PARTITION BY`)
- Self-joins for data enrichment
- String manipulation (`TRIM`, `STR_TO_DATE`)
- Data type conversions
- Staging table pattern for safe data cleaning

## Exploratory Data Analysis

### Analysis Dimensions

#### Basic Statistics
- Maximum layoffs (total and percentage)
- Companies with 100% workforce reduction
- Dataset date range coverage

#### Aggregated Analysis
- **By Company**: Total and average layoffs per company
- **By Industry**: Industry-wide layoff trends
- **By Geography**: Country-level layoff analysis
- **By Time**: Yearly layoff patterns
- **By Stage**: Layoffs by company funding stage

#### Advanced Analysis
- **Rolling Totals**: Month-over-month cumulative layoffs using CTEs
- **Year-over-Year Rankings**: Top companies with highest layoffs per year using `DENSE_RANK()`

### Key Queries

#### Rolling Total by Month
```sql
WITH Rolling_Total AS (
    SELECT SUBSTRING(`date`,1,7) AS `MONTH`, 
           SUM(total_laid_off) AS total_off
    FROM world_layoffs.layoffs_staging2
    WHERE SUBSTRING(`date`,1,7) IS NOT NULL
    GROUP BY `MONTH`
    ORDER BY 1 ASC
)
SELECT `MONTH`, total_off,
       SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total 
FROM Rolling_Total;
```

#### Top Companies Per Year
```sql
WITH company_year AS (
    SELECT company, YEAR(`date`) AS years, 
           SUM(total_laid_off) AS total_laid_off
    FROM world_layoffs.layoffs_staging2
    GROUP BY company, YEAR(`date`)
),
Company_Year_Rank AS (
    SELECT *, 
           DENSE_RANK() OVER(PARTITION BY years 
                            ORDER BY total_laid_off DESC) AS Ranking
    FROM company_year
    WHERE years IS NOT NULL
)
SELECT * FROM Company_Year_Rank;
```

## Database Schema

### Source Table: `world_layoffs.layoffs`
Raw data imported from Kaggle

### Staging Tables

**layoffs_staging** - Initial copy of raw data

**layoffs_staging2** - Cleaned and processed data
```sql
- company (TEXT)
- location (TEXT)
- industry (TEXT)
- total_laid_off (INT)
- percentage_laid_off (TEXT)
- date (DATE)
- stage (TEXT)
- country (TEXT)
- funds_raised_millions (INT)
```

## Prerequisites

- MySQL 5.7+ or compatible database
- MySQL Workbench (recommended) or any MySQL client

## Setup Instructions

1. Download the dataset from [Kaggle](https://www.kaggle.com/datasets/swaptr/layoffs-2022)
2. Create the `world_layoffs` database
3. Import the raw data into `world_layoffs.layoffs` table
4. Run the data cleaning script
5. Execute EDA queries on the cleaned `layoffs_staging2` table

## Key Insights Generated

- Companies most affected by layoffs
- Industries hit hardest by workforce reductions
- Geographic distribution of layoffs globally
- Temporal trends and seasonal patterns
- Correlation between company funding stage and layoff events
- Month-over-month layoff momentum and trends

## SQL Techniques Demonstrated

- Common Table Expressions (CTEs)
- Window Functions (`ROW_NUMBER()`, `DENSE_RANK()`, `SUM() OVER`)
- Self-Joins
- Data type conversions
- String manipulation
- Aggregate functions
- Subqueries
- Table creation and alteration

## Author

Data cleaning and analysis by aka

## License

Dataset sourced from Kaggle. Please refer to the original dataset for licensing information.

---

**Note**: This project demonstrates end-to-end SQL data analysis workflow from raw data ingestion through cleaning to exploratory analysis.









