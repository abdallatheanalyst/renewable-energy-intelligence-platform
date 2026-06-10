# Renewable Energy Intelligence Platform

## Overview

This project is an end-to-end data platform built using UK electricity market data. The objective is to centralize electricity demand, generation, and forecasting data into a trusted analytical platform that supports energy monitoring, forecasting, and sustainability insights.

The project follows a Medallion Architecture (Bronze → Silver → Gold), transforming raw source files into trusted datasets for analytics and reporting.

## Technology Stack

* SQL Server
* T-SQL
* Data Warehousing
* ETL (Extract, Transform, Load)
* Data Modeling
* Power BI

## Data Sources

### Electricity Demand

Contains:

* National electricity demand
* Embedded wind generation
* Embedded solar generation
* Interconnector flows

Files:

* demanddata_2024.csv
* demanddata_2025.csv
* demanddata_2026.csv

Records Loaded: **41,662**

### Electricity Generation

Contains:

* Gas generation
* Coal generation
* Nuclear generation
* Wind generation
* Solar generation
* Carbon intensity metrics
* Renewable energy metrics

File:

* df_fuel_ckan.csv

Records Loaded: **305,629**

### Electricity Forecasts

Contains:

* Day-ahead demand forecasts
* Forecast timestamps
* Settlement periods
* Forecast horizons

Files:

* historic_day_ahead_forecast.csv
* current_day_ahead_forecast.csv

Records Loaded: **57,149**

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
 Power BI Analytics
```

## Bronze Layer

The Bronze layer stores source data exactly as received from external systems. No transformations or business rules are applied at this stage.

Tables:

* Bronze_Demand
* Bronze_Generation
* Bronze_Forecast

Total Records Ingested:

**404,440+**

## Silver Layer

The Silver layer transforms raw source data into trusted analytical datasets through data cleansing, standardization, and type conversion.

### Demand Transformations

* Converted settlement dates to DATE
* Converted demand metrics to FLOAT
* Standardized interconnector flow fields

### Generation Transformations

* Converted timestamps to DATETIME2
* Converted generation metrics to FLOAT
* Standardized renewable and carbon metrics

### Forecast Transformations

* Removed quotation marks from source data
* Cleaned timestamp formatting
* Converted forecast timestamps to DATETIME2
* Converted forecast values to numeric types

Tables:

* Silver_Demand
* Silver_Generation
* Silver_Forecast

## Current Data Volume

| Dataset    |      Records |
| ---------- | -----------: |
| Demand     |       41,662 |
| Generation |      305,629 |
| Forecast   |       57,149 |
| **Total**  | **404,440+** |

## Key Data Engineering Concepts Demonstrated

* Data Ingestion
* ETL Pipelines
* Medallion Architecture
* Data Quality Validation
* Data Type Standardization
* Data Warehousing
* SQL-Based Data Transformation
* Business-Oriented Data Modeling
