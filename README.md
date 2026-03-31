# Fudgemart Inc. Data Warehouse & BI 

## Overview
This builds a unified data warehouse for **Fudgemart Inc.**, a company formed by merging:
- **Fudgemart** (online retail)
- **Fudgeflix** (video-on-demand subscription service)

The goal was to integrate data from both systems into a single, consistent model to support analytics and business intelligence.

---

## Business Objective
Provide a unified view of company performance and enable insights into:
- Sales performance
- Product trends
- Customer behavior

---

## Tech Stack
- **Snowflake** – Data warehouse
- **dbt (Data Build Tool)** – Data transformations & modeling
- **Azure SQL** – Source systems
- **Power BI** – Data visualization and analytics

---

## Data Architecture

### Star Schema Design
The warehouse is built using a **star schema**:

<img width="664" height="428" alt="Screenshot 2026-03-31 at 12 12 44 PM" src="https://github.com/user-attachments/assets/5ef90ae2-9777-4007-b6a5-6236cf5a7224" />



- **FACT_SALES**
  - Transaction-level data
  - Measures:
    - `sales_quantity`
    - `sales_total`
    - `sales_productprice`

- **DIM_CUSTOMER**
  - Unified customer data from both systems
  - Includes `source_system` to distinguish origins

- **DIM_PRODUCT**
  - Combines:
    - Fudgemart products
    - Fudgeflix subscription plans
  - Includes derived field:
    - `product_type` (fudgemart vs fudgeflix)

- **DIM_DATE**
  - Time dimension for analysis (year, month, quarter, day)
 
## Fudgemart + Fudgeflix → Azure SQL → Snowflake (Staging)
## → Star Schema → Analytics / BI​

​

---

## Data Pipeline (ETL / ELT)
<img width="1148" height="407" alt="Screenshot 2026-03-31 at 12 51 51 PM" src="https://github.com/user-attachments/assets/9aa9c835-ae93-43c8-98d5-6e6b0d2b0d1b" />

<img width="1148" height="529" alt="Screenshot 2026-03-31 at 12 52 48 PM" src="https://github.com/user-attachments/assets/206946e1-b0b4-465d-936f-0514b67fde73" />


1. **Extract**
   - Data pulled from Azure SQL source systems

2. **Load (Staging)**
   - Raw data loaded into Snowflake staging tables

3. **Transform (dbt)**
   - Cleaned and standardized data
   - Built dimension and fact tables
   - Applied business logic and derived fields

4. **Modeling**
   - Star schema implemented for analytics

---

## Key Design Decisions

- **Unified Fact Table**
  - Combined retail transactions and subscription billing into `FACT_SALES`

- **Derived Fields**
  - `product_type` created to distinguish product origins across systems

- **Handling Different Data Structures**
  - Fudgeflix transactions modeled as:
    - `sales_quantity = 1` per billing event

- **Surrogate Keys**
  - Used across all dimensions for consistency and scalability

---

## Data Validation

- Verified row counts across staging and final tables
- Ensured referential integrity between fact and dimension tables
- Checked for:
  - Missing joins
  - Null foreign keys
  - Data consistency across systems

---

## BI & Insights

Power BI dashboards were built to analyze:
- Sales trends over time
- Product and category performance
- Customer behavior patterns

### Key Findings
- Electronics dominate Fudgemart sales (~66%)
- Fudgeflix contributes a small share of total revenue (~0.3%)
- Slight downward trend in year-over-year sales

<img width="618" height="407" alt="Screenshot 2026-03-31 at 12 48 29 PM" src="https://github.com/user-attachments/assets/e362c7a7-732e-4781-ae08-016673aea261" />



---

## Future Improvements

- Improve customer matching across systems
- Add additional fact tables (e.g., rentals, reviews)
- Expand BI dashboards with advanced analytics
- Incorporate additional data sources (e.g., employee data)

---



