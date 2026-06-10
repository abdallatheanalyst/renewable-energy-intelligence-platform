# Renewable Energy Intelligence Platform

## Overview

The Renewable Energy Intelligence Platform is an end-to-end data engineering project built using UK electricity market data. The objective is to centralize electricity demand, generation, and forecasting datasets into a trusted analytical platform capable of supporting reporting, forecasting, and sustainability insights.

The project follows a Medallion Architecture (Bronze → Silver → Gold), transforming raw source files into business-ready datasets optimized for analytics and reporting.

---

## Business Problem

Energy organizations rely on accurate demand, generation, and forecasting data to monitor grid performance, evaluate renewable energy adoption, reduce carbon intensity, and improve forecasting accuracy.

Raw datasets are often fragmented, inconsistent, and unsuitable for direct analytical consumption. This project addresses that challenge by building a structured data platform that ingests, cleans, validates, and models UK energy data before it reaches the reporting layer.

---

## Technology Stack

- SQL Server
- T-SQL
- Data Warehousing
- ETL (Extract, Transform, Load)
- Dimensional Modeling
- Power BI
- GitHub

---

## Architecture

```text
Raw Data Sources
        ↓
Bronze Layer
        ↓
Silver Layer
        ↓
Gold Layer
        ↓
Analytics & Reporting
```

---

## Data Sources

### Electricity Demand

Contains:

- National electricity demand
- Embedded wind generation
- Embedded solar generation
- Interconnector flows

Files:

- demanddata_2024.csv
- demanddata_2025.csv
- demanddata_2026.csv

Records Loaded:

41,662

---

### Electricity Generation

Contains:

- Gas generation
- Coal generation
- Nuclear generation
- Wind generation
- Solar generation
- Hydro generation
- Biomass generation
- Carbon intensity metrics
- Renewable energy metrics

File:

- df_fuel_ckan.csv

Records Loaded:

305,629

---

### Electricity Forecasts

Contains:

- Day-ahead demand forecasts
- Forecast timestamps
- Settlement periods
- Forecast horizons

Files:

- historic_day_ahead_forecast.csv
- current_day_ahead_forecast.csv

Records Loaded:

57,149

---

## Total Data Volume

| Dataset | Records |
|----------|---------:|
| Demand | 41,662 |
| Generation | 305,629 |
| Forecast | 57,149 |
| **Total** | **404,440+** |

---

# Bronze Layer

The Bronze layer stores source data exactly as received from external systems. No transformations or business rules are applied at this stage.

### Tables

- Bronze_Demand
- Bronze_Generation
- Bronze_Forecast

### Purpose

- Preserve raw source data
- Maintain source-of-truth records
- Support auditing and traceability

---

# Silver Layer

The Silver layer transforms raw source data into trusted analytical datasets through cleansing, standardization, and data quality validation.

### Silver_Demand

Transformations:

- Converted settlement dates to DATE
- Converted demand metrics to FLOAT
- Standardized interconnector flow fields
- Preserved source lineage metadata

Rows:

41,662

---

### Silver_Generation

Transformations:

- Converted timestamps to DATETIME2
- Converted generation metrics to FLOAT
- Standardized renewable and carbon metrics
- Validated generation measurements

Rows:

305,629

---

### Silver_Forecast

Transformations:

- Removed quotation marks from source data
- Cleaned timestamp formatting
- Converted forecast timestamps to DATETIME2
- Converted forecast values to numeric types
- Standardized forecast metadata

Rows:

57,149

---

# Gold Layer

The Gold layer contains business-ready analytical tables designed for reporting, dashboarding, and decision-making.

Unlike the Silver layer, which focuses on data quality and standardization, the Gold layer focuses on business consumption and analytical performance.

---

## Dimension Tables

### Dim_Date

Provides centralized calendar intelligence for all analytical models.

Attributes:

- Date Key
- Full Date
- Year
- Quarter
- Month
- Month Name
- Day Number
- Day Name
- Week Number

Rows:

6,369

---

### Dim_Time

Provides settlement-period intelligence and time-based analysis.

Attributes:

- Settlement Period
- Hour Number
- Minute Number
- Time Label

Rows:

48

---

## Fact Tables

### Fact_Demand

Stores electricity demand metrics and interconnector flows.

Rows:

41,662

Measures:

- England & Wales Demand
- Embedded Wind Generation
- Embedded Solar Generation
- Interconnector Flows

---

### Fact_Generation

Stores electricity generation and carbon metrics.

Rows:

305,629

Measures:

- Gas
- Coal
- Nuclear
- Wind
- Solar
- Hydro
- Biomass
- Carbon Intensity
- Renewable Generation
- Fossil Generation
- Generation Mix Percentages

---

### Fact_Forecast

Stores day-ahead electricity demand forecasts.

Rows:

57,149

Measures:

- Forecast Demand
- Forecast Horizon
- Forecast Timestamp
- Settlement Window

---

# Star Schema

```text
                Dim_Date
                    |
                    |
                    |
              Fact_Demand
                    |
                    |
                    |
Dim_Time ------------------------

                Dim_Date
                    |
                    |
                    |
           Fact_Generation

                Dim_Date
                    |
                    |
                    |
            Fact_Forecast
```

---

# Key Data Engineering Concepts Demonstrated

- Data Ingestion
- ETL Pipelines
- Medallion Architecture
- Data Warehousing
- Data Quality Validation
- Data Standardization
- Dimensional Modeling
- Fact and Dimension Tables
- Star Schema Design
- Business-Oriented Analytics
- SQL-Based Data Transformation

---

# Project Outcome

Successfully designed and implemented a multi-layer data warehouse containing over 404,000 UK energy records across demand, generation, and forecasting domains.

The platform transforms raw operational data into trusted analytical datasets that can be used for reporting, forecasting accuracy analysis, renewable energy monitoring, and sustainability insights.
