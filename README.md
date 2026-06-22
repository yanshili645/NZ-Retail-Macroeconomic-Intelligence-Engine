1 Project Overview

This project is an end-to-end data warehousing and business intelligence solution that integrates macroeconomic retail data from Statistics New Zealand (Stats NZ). It combines monthly Electronic Card Transactions (ECT) with quarterly Retail Trade Surveys (RTT) into a centralized SQL database and outputs an interactive 6-page Power BI analytics dashboard.

2 Repository Structure

Plaintext

├── 01_data/                 # Raw and processed data documentation (CSV/Metadata)

├── 02_scripts/              # ETL and Database Engineering scripts

│   ├── 01_etl_extract.sql   # Ingestion and staging setup

│   ├── 02_etl_transform.sql # Data cleaning, date patching, and anomaly filtering

│   └── 03_etl_load.sql      # Fact/Dimension table loading and constraint management

├── 03_views/                # Analytical SQL views for Power BI pre-joins

├── 04_bi_dashboard/         # Power BI desktop files (.pbix) and model layouts

└── README.md                # Project documentation (This file)


3 Tech Stack & Architecture
Database Engine: Microsoft SQL Server / SQL Server Management Studio (SSMS)

BI Platform: Power BI (Advanced DAX modeling, custom sort keys, regional maps)

Pipeline Logic: T-SQL ETL Staging-to-Core Architecture

Data Model: Constellation / Galaxy Schema (Multiple Fact tables sharing conformed dimensions)

4 The Real-World Data Cleaning Challenge

Real-world government economic data is incredibly messy. Before building the dashboard, a massive portion of the engineering effort went into solving deep data-quality issues:

The Aggregation Grain Puzzle: Forcing monthly transactional records (ECT) and quarterly high-level surveys (RTT) into identical time intervals without introducing artificial multipliers.

The "Double-Counting" Risk: Stats NZ includes regional macro summaries (like "TOTAL NORTH ISLAND") right next to individual cities. Left unfiltered, dynamic dashboard filters would accidentally triple the actual economic numbers. We hardcoded a deterministic filter layer into our SQL views to drop macro aggregates from standard charts.

Format Corruption & Hidden Multipliers: Fixing broken text dates (like standardizing "YYYY.1" into October periods), removing hidden Unicode quotation marks, and using metadata parameters to scale coefficients by $10^6$ to reveal true dollar totals.

5 Core Features & Views Created

vw_ECT_Monthly_Industry & vw_RTT_Quarterly_Sales: Pure filtered transactional and survey logs mapping the 22 core industry groups.

vw_ECT_RTT_Aligned: The master cross-survey matching view grouping monthly inputs into synchronized quarterly buckets for direct analysis.

vw_YoY_Growth: Advanced analytical database joins calculating Year-over-Year momentum without slowing down front-end dashboard interactions.


6 How to Run the Project

Clone this repository to your local system.

Run the SQL engineering sequence inside the 02_scripts/ folder on your local SQL instance to set up the data warehouse.
Apply the view definitions inside 03_views/.

Open the .pbix file inside 04_bi_dashboard/ and refresh the data source line to see the interactive interface.

