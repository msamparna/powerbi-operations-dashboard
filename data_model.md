# Data Model Structure & Relationships

## 🗂️ Overview

The data model follows a **star schema** design with a central fact table 
surrounded by dimension tables — optimised for Power BI performance and DAX calculations.

## 📊 Fact Tables

| Table | Description | Grain |
|---|---|---|
| `fact_revenue` | Daily revenue transactions | One row per transaction |
| `fact_vendor_invoices` | Vendor invoice records | One row per invoice |
| `fact_project_milestones` | Project delivery tracking | One row per milestone |

## 📋 Dimension Tables

| Table | Description | Key Column |
|---|---|---|
| `dim_date` | Full date table with fiscal calendar | `date_key` |
| `dim_department` | Business unit hierarchy | `department_id` |
| `dim_vendor` | Vendor master data | `vendor_id` |
| `dim_project` | Project metadata | `project_id` |
| `dim_region` | Geographic hierarchy | `region_id` |

## 🔗 Relationships

| From Table | To Table | Join Key | Cardinality |
|---|---|---|---|
| `fact_revenue` | `dim_date` | `date_key` | Many-to-One |
| `fact_revenue` | `dim_department` | `department_id` | Many-to-One |
| `fact_vendor_invoices` | `dim_vendor` | `vendor_id` | Many-to-One |
| `fact_vendor_invoices` | `dim_date` | `date_key` | Many-to-One |
| `fact_project_milestones` | `dim_project` | `project_id` | Many-to-One |
| `fact_project_milestones` | `dim_date` | `date_key` | Many-to-One |

## ⚙️ Modelling Decisions

- **Star schema** preferred over snowflake for query performance
- All date filtering routed through `dim_date` only
- Inactive relationships used for multiple date role scenarios
- Row-level security (RLS) applied at `dim_department` level
