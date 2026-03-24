# Databases
Retail sales analytics database redesigned from a denormalized schema into a star schema with 540K+ records, 7 business views, stored procedures, and index optimization — built in SQL Server.

# Retail Sales Analytics Database

A complete database redesign and implementation project built in SQL Server — transforming a denormalized source schema into a clean, normalized analytical database optimized for business intelligence queries.

## Project Overview

The original `LIY26` schema contained wide, denormalized tables across customers, products, promotions, and sales. This project redesigned it from scratch into a **star schema** with one central fact table and eight dimension tables, then built a full analytics layer on top.

## Schema Design

| Table | Type | Description |
|---|---|---|
| `MY_SALES` | Fact | 540,611 sales transactions |
| `MY_CUSTOMERS` | Dimension | Customer demographics |
| `MY_PRODUCTS` | Dimension | Product catalog with pricing |
| `MY_CHANNELS` | Dimension | Sales channels |
| `MY_PROMOTIONS` | Dimension | Promotion campaigns |
| `MY_TIMES` | Dimension | Date dimension |
| `MY_COUNTRIES` | Dimension | Country/region lookup |
| `MY_PRODUCT_CATEGORIES` | Dimension | Top-level category |
| `MY_PRODUCT_SUBCATEGORIES` | Dimension | Mid-level subcategory |

Normalization achieved: **3NF** — no partial or transitive dependencies across any table.

## Business Views (BV01–BV07)

Each view answers a real stakeholder question and is implemented as a reusable SQL view:

| View | Purpose | Stakeholder |
|---|---|---|
| BV01 | Revenue by Product | Sales & Marketing |
| BV02 | Revenue by Sales Channel | Sales & Marketing |
| BV03 | High-Value Customer Identification | CRM / Customer Insights |
| BV04 | Product Performance by Category | Sales & Marketing |
| BV05 | Sales Trend by Year | Executive Leadership |
| BV06 | Promotion Effectiveness | Marketing |
| BV07 | Data Quality Validation | Data Governance |

## Stored Procedures

Four stored procedures enforce business rules and control all UPDATE/DELETE operations — end users are granted EXECUTE permissions rather than direct table access:

- `usp_GetProductDetails` — read-only product lookup with category joins
- `sp_UpdateCustomer` — upsert logic for customer records with FK validation
- `sp_UpdatePromotion` — partial update support with date range validation
- `sp_FixOrDeleteSalesRecord` — controlled sales correction or deletion for data governance roles

## Data Integrity & Constraints

Physical design constraints implemented across all tables:

- **Pricing rules**: `RETAIL_PRICE >= UNIT_PRICE`, both must be positive
- **Transaction rules**: quantity > 0, amount ≥ 0, ship/payment date ≥ sale date
- **Domain rules**: marital status and income level restricted to predefined categories
- **Referential integrity**: full PK/FK enforcement across all fact-dimension relationships

## Query Optimization

Nonclustered indexes added on high-frequency join and grouping columns (`PROD_ID`, `SALE_DATE`, `PROMO_ID`). Execution plan analysis confirmed:

- Clustered index scans replaced by nonclustered index seeks on BV06
- Logical reads reduced significantly after indexing
- Composite index `IDX_MY_SALES_PRODID_SALEDATE` improved category revenue queries by filtering before aggregation

## Data Loading

Multi-year sales data (`LI_SALES_12_13` and `LI_SALES_14`) merged into a single unified fact table. Dimension tables loaded in dependency order before fact data to prevent FK violations. Final row count: **540,611**.

## Tools

- Microsoft SQL Server
- Azure Data Studio
- T-SQL (DDL, DML, Views, Stored Procedures, Indexes)

## Report

Full project report included — covers conceptual design, ER diagram, logical schema, normalization justification, physical constraints, implementation, and execution plan analysis.
