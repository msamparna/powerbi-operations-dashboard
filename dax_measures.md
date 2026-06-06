# DAX Measures

## 💰 Revenue Measures

```dax
-- Total Revenue
Total Revenue = SUM(fact_revenue[revenue_amount])

-- Revenue vs Prior Year
Revenue PY = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(dim_date[date]))

-- YoY Revenue Growth %
Revenue Growth % = 
DIVIDE([Total Revenue] - [Revenue PY], [Revenue PY], 0) * 100

-- YTD Revenue
Revenue YTD = TOTALYTD([Total Revenue], dim_date[date])

-- Monthly Revenue Target Achievement %
Target Achievement % = 
DIVIDE([Total Revenue], SUM(fact_revenue[target_amount]), 0) * 100
```

## 📉 Variance Measures

```dax
-- Budget Variance
Budget Variance = 
SUM(fact_revenue[actual_amount]) - SUM(fact_revenue[budget_amount])

-- Budget Variance %
Budget Variance % = 
DIVIDE([Budget Variance], SUM(fact_revenue[budget_amount]), 0) * 100

-- Variance Flag
Variance Flag = 
IF([Budget Variance %] >= 0, "Favourable", "Unfavourable")
```

## 🏭 Vendor Measures

```dax
-- Total Vendor Spend
Total Vendor Spend = SUM(fact_vendor_invoices[invoice_amount])

-- On-Time Payment Rate
On Time Payment Rate % = 
DIVIDE(
    COUNTROWS(FILTER(fact_vendor_invoices, 
        fact_vendor_invoices[payment_date] <= fact_vendor_invoices[due_date])),
    COUNTROWS(fact_vendor_invoices), 0) * 100
```

## 📁 Project Measures

```dax
-- Project Delivery Rate
Delivery Rate % = 
DIVIDE(
    COUNTROWS(FILTER(fact_project_milestones,
        fact_project_milestones[actual_date] <= fact_project_milestones[planned_date])),
    COUNTROWS(fact_project_milestones), 0) * 100

-- RAG Status
RAG Status = 
SWITCH(TRUE(),
    [Delivery Rate %] >= 95, "🟢 Green",
    [Delivery Rate %] >= 85, "🟡 Amber",
    "🔴 Red")
```
