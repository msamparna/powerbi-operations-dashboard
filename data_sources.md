# Data Sources & Refresh Logic

## 🔌 Data Source Connections

| Source | Type | Connection Method | Refresh Frequency |
|---|---|---|---|
| Revenue Data | Excel (.xlsx) | SharePoint folder | Daily |
| Vendor Invoices | SQL Server | DirectQuery | Real-time |
| Project Milestones | CSV | SharePoint folder | Weekly |
| Budget Data | Excel (.xlsx) | Local file | Monthly |
| HR & Headcount | SQL Server | Import mode | Weekly |

## ⚙️ Power Query Transformations

### Revenue Data
- Remove duplicate transaction IDs
- Standardise date format to YYYY-MM-DD
- Split region and department from combined field
- Replace null values with 0 for revenue amounts
- Filter out cancelled and reversed transactions

### Vendor Invoices
- Merge vendor master data on vendor_id
- Calculate payment delay in days
- Flag invoices overdue by >30 days
- Categorise spend by vendor type

### Project Milestones
- Unpivot milestone columns to rows
- Calculate variance between planned and actual dates
- Derive RAG status based on delivery performance
- Join project metadata from dim_project

## 🔄 Refresh Schedule

| Dashboard Page | Refresh Mode | Schedule |
|---|---|---|
| Executive Summary | Import | Daily 6:00 AM |
| Revenue Analytics | Import | Daily 6:00 AM |
| Vendor Operations | DirectQuery | Real-time |
| Project Performance | Import | Weekly Monday 7:00 AM |

## 🔒 Data Gateway

- On-premises data gateway configured for SQL Server connections
- SharePoint connector used for Excel and CSV sources
- Credentials stored securely in Power BI Service
- Row-level security enforced at department level
