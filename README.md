EDA Layoffs Analysis
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










