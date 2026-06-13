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

* SQL Server
* T-SQL
* Data Warehousing
* ETL (Extract, Transform, Load)
* Dimensional Modeling
* Power BI
* GitHub

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

* National electricity demand
* Embedded wind generation
* Embedded solar generation
* Interconnector flows

Files:

* demanddata_2024.csv
* demanddata_2025.csv
* demanddata_2026.csv

Records Loaded:

41,662

---

### Electricity Generation

Contains:

* Gas generation
* Coal generation
* Nuclear generation
* Wind generation
* Solar generation
* Hydro generation
* Biomass generation
* Carbon intensity metrics
* Renewable energy metrics

File:

* df_fuel_ckan.csv

Records Loaded:

305,629

---

### Electricity Forecasts

Contains:

* Day-ahead demand forecasts
* Forecast timestamps
* Settlement periods
* Forecast horizons

Files:

* historic_day_ahead_forecast.csv
* current_day_ahead_forecast.csv

Records Loaded:

57,149

---

## Total Data Volume

| Dataset    |      Records |
| ---------- | -----------: |
| Demand     |       41,662 |
| Generation |      305,629 |
| Forecast   |       57,149 |
| **Total**  | **404,440+** |

---

# Bronze Layer

The Bronze layer stores source data exactly as received from external systems. No transformations or business rules are applied at this stage.

### Tables

* Bronze_Demand
* Bronze_Generation
* Bronze_Forecast

### Purpose

* Preserve raw source data
* Maintain source-of-truth records
* Support auditing and traceability

---

# Silver Layer

The Silver layer transforms raw source data into trusted analytical datasets through cleansing, standardization, and data quality validation.

### Silver_Demand

Transformations:

* Converted settlement dates to DATE
* Converted demand metrics to FLOAT
* Standardized interconnector flow fields
* Preserved source lineage metadata

Rows:

41,662

---

### Silver_Generation

Transformations:

* Converted timestamps to DATETIME2
* Converted generation metrics to FLOAT
* Standardized renewable and carbon metrics
* Validated generation measurements

Rows:

305,629

---

### Silver_Forecast

Transformations:

* Removed quotation marks from source data
* Cleaned timestamp formatting
* Converted forecast timestamps to DATETIME2
* Converted forecast values to numeric types
* Standardized forecast metadata

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

* Date Key
* Full Date
* Year
* Quarter
* Month
* Month Name
* Day Number
* Day Name
* Week Number

Rows:

6,369

---

### Dim_Time

Provides settlement-period intelligence and time-based analysis.

Attributes:

* Settlement Period
* Hour Number
* Minute Number
* Time Label

Rows:

48

---

## Fact Tables

### Fact_Demand

Stores electricity demand metrics and interconnector flows.

Rows:

41,662

Measures:

* England & Wales Demand
* Embedded Wind Generation
* Embedded Solar Generation
* Interconnector Flows

---

### Fact_Generation

Stores electricity generation and carbon metrics.

Rows:

305,629

Measures:

* Gas
* Coal
* Nuclear
* Wind
* Solar
* Hydro
* Biomass
* Carbon Intensity
* Renewable Generation
* Fossil Generation
* Generation Mix Percentages

---

### Fact_Forecast

Stores day-ahead electricity demand forecasts.

Rows:

57,149

Measures:

* Forecast Demand
* Forecast Horizon
* Forecast Timestamp
* Settlement Window

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

# Business Views

To support reporting and analytical consumption, business-focused views were created from the Gold Layer.

### vw_Daily_Energy_Mix

Provides daily generation mix metrics including:

* Renewable Generation
* Fossil Generation
* Renewable Percentage
* Fossil Percentage
* Wind Generation
* Solar Generation
* Nuclear Generation
* Gas Generation

### vw_Daily_Carbon_Intensity

Provides sustainability-focused metrics including:

* Carbon Intensity
* Low Carbon Generation
* Zero Carbon Generation
* Renewable Generation
* Fossil Generation

### vw_Demand_Trends

Provides demand intelligence metrics including:

* Electricity Demand
* Settlement Period Analysis
* Time-of-Day Trends
* Embedded Wind Generation
* Embedded Solar Generation
* Interconnector Flow Analysis

---

# Business Insights

The analytical layer was designed to transform raw energy data into actionable business insights. By leveraging the Gold Layer and business-oriented views, several key trends were identified across electricity demand, renewable energy adoption, and sustainability performance.

---

## Electricity Demand Patterns

Analysis of national demand data identified clear daily consumption patterns across the UK electricity grid.

### Key Findings

* Peak electricity demand consistently occurs between **17:00 and 20:00**.
* The highest average demand period occurs at approximately **18:30**.
* Winter periods, particularly **January**, experience the highest overall electricity demand levels.

These findings provide insight into grid utilization patterns and daily energy consumption behavior.

---

## Renewable Energy Transition

The UK energy mix has undergone a significant transformation throughout the period covered by the dataset.

### Key Findings

* Renewable generation increased from approximately **3.3%** of total generation in **2009** to more than **41%** by **2026**.
* Fossil-fuel generation declined from approximately **74%** to less than **24%** during the same period.
* Renewable generation exceeded **65% of total generation** on several days between **2024 and 2026**.

These results demonstrate the accelerating transition toward renewable energy sources and reduced dependence on fossil fuels.

---

## Growth of Renewable Sources

Further analysis identified the primary technologies driving renewable energy growth across the UK electricity market.

### Key Findings

* Wind generation increased from approximately **380 MW** in **2009** to more than **9,200 MW** by **2026**.
* Solar generation expanded from effectively zero contribution in the early years of the dataset to more than **2,200 MW** by **2026**.
* Wind generation emerged as the dominant contributor to renewable energy growth across the study period.

The results highlight the significant role of wind energy in supporting national decarbonization efforts.

---

## Carbon Intensity and Sustainability

The transition toward renewable generation has had a measurable impact on the environmental performance of the UK electricity grid.

### Key Findings

* Average carbon intensity declined from approximately **445 gCO₂/kWh** in **2009** to nearly **124 gCO₂/kWh** by **2024**.
* The cleanest recorded day achieved a carbon intensity of approximately **40.5 gCO₂/kWh**.
* The most carbon-intensive day exceeded **600 gCO₂/kWh**.

These findings demonstrate a substantial reduction in carbon emissions associated with electricity generation and reflect long-term sustainability improvements achieved through renewable energy adoption.

---

# Key Data Engineering Concepts Demonstrated

* Data Ingestion
* ETL Pipelines
* Medallion Architecture
* Bronze, Silver, and Gold Layers
* Data Warehousing
* Data Quality Validation
* Data Standardization
* Dimensional Modeling
* Fact and Dimension Tables
* Star Schema Design
* Business-Oriented Analytics
* SQL-Based Data Transformation
* Analytical Data Modeling
* Sustainability Analytics
* Energy Data Intelligence

---

# Project Outcome

Designed and implemented a multi-layer Renewable Energy Intelligence Platform containing more than **404,000 UK energy records** across electricity demand, generation, and forecasting domains.

The platform transforms raw operational data into trusted analytical datasets through a Bronze → Silver → Gold architecture, enabling reliable reporting, energy transition analysis, sustainability monitoring, and business intelligence.

The resulting data warehouse provides a scalable foundation for advanced analytics, dashboarding, forecasting initiatives, and future cloud-based data engineering enhancements including Python automation, Azure storage integration, Airflow orchestration, and data quality monitoring.

---

# Future Enhancements

The next phase of the project focuses on evolving the platform into a fully automated cloud-based data engineering solution.

Planned enhancements include:

```text
UK Energy Data
      ↓
Python
      ↓
Azure Blob Storage
      ↓
Airflow
      ↓
SQL Warehouse
      ↓
Data Quality Checks
      ↓
Gold Layer
      ↓
Power BI
```

Future improvements will include:

* Python-based ingestion and transformation pipelines
* Azure Blob Storage for cloud-based raw data storage
* Apache Airflow for workflow orchestration
* Automated data quality monitoring and validation
* Power BI executive dashboards and reporting
* End-to-end pipeline automation and scheduling

These enhancements will transform the project from a warehouse prototype into a production-style data platform aligned with modern data engineering practices.
