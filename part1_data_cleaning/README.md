# Part 1: Business Data Cleaning, Validation & Excel Reporting

## Problem Summary

This repository contains the completed Part 1 submission for business data cleaning, validation, and Excel reporting. The goal was to clean a messy retail order-level dataset, validate business rules, document data quality issues, and create Excel summary reports for business review.

## Dataset Description

The dataset contains retail order-level sales data exported from multiple internal systems. It includes order IDs, dates, customer details, product categories, shipping mode, quantity, price, discount, sales, cost, profit, payment status, and order status.

The original raw file is preserved unchanged at:

`data/raw_orders.xlsx`

The cleaned analysis-ready file is stored at:

`data/cleaned_orders.xlsx`

## Tools Used

- Microsoft Excel-compatible `.xlsx` workbooks
- Excel formulas
- Excel tables and filters
- Pivot-style summary tables
- Conditional formatting
- GitHub

## Cleaning Steps Performed

1. Preserved the original raw file without modification.
2. Created a separate cleaned workbook.
3. Standardized text fields including customer name, segment, region, state, city, category, sub-category, ship mode, payment status, and order status.
4. Converted mixed date formats into standard Excel dates.
5. Created shipping delay, month, and year columns.
6. Removed exact duplicate rows.
7. Flagged conflicting duplicate order IDs.
8. Cleaned and validated discount values.
9. Created calculated sales, calculated profit, and profit margin columns.
10. Classified each record using a data quality flag.
11. Created a data quality report workbook.
12. Created pivot-style business summary reports.

## Business Rules Applied

- Missing `region` values were filled as `Unknown` and flagged.
- Missing `ship_mode` values were filled as `Unknown` and flagged.
- Missing discounts were treated as 0 only when other sales fields were valid.
- Negative discounts were flagged as invalid.
- Discounts above 50% were flagged as invalid.
- Cancelled orders were excluded from final completed-sales summaries.
- Failed payments were excluded from final completed-sales summaries.
- Refunded orders were summarized separately.
- Ship dates earlier than order dates were flagged as invalid.
- Exact duplicate rows were removed.
- Conflicting duplicate order IDs were retained and flagged for review.

## Summary of Data Quality Issues Found

| Issue Type | Count |
|---|---:|
| Raw records | 932 |
| Records after exact duplicate removal | 912 |
| Exact duplicate rows removed | 20 |
| Duplicate order IDs found in raw file | 31 |
| Conflicting duplicate order IDs | 12 |
| Conflicting duplicate records flagged | 24 |
| Missing region values | 25 |
| Missing ship mode values | 21 |
| Missing discounts treated as 0 | 18 |
| Invalid discounts | 30 |
| Ship date before order date | 21 |
| Sales/profit mismatches | 83 |
| Cancelled orders | 145 |
| Failed payments | 69 |
| Refunded payments | 71 |

## Final Record Summary

| Record Type | Count |
|---|---:|
| Clean records | 505 |
| Warning records | 334 |
| Invalid records | 73 |
| Records removed | 20 |
| Records flagged for review | 24 |
| Completed-sales eligible records | 562 |

## Pivot Reports Created

The workbook `outputs/pivot_summary.xlsx` includes the following reports:

1. Sales and profit by region.
2. Sales and profit by category and sub-category.
3. Order count by ship mode.
4. Profit margin by customer segment.
5. Refunded, cancelled, failed, and returned orders by region.
6. Monthly sales trend.
7. Key business insights.

At least two reports are sorted or filtered:

- Sales and profit by region is sorted by calculated sales in descending order.
- Category and sub-category summary is sorted by calculated sales in descending order.
- Monthly sales trend is filtered to completed, paid, non-invalid records.

## Key Business Insights

- The completed-sales summaries only include paid, completed, non-invalid records.
- The top sales region is **South** with completed calculated sales of **₹1,458,576.91**.
- The highest sales category/sub-category combination is **Technology - Copiers**.
- The strongest weighted profit-margin segment is **Home Office**.
- **North** has the highest combined count of refunded, cancelled, failed, and returned records.
- Invalid records were retained in the cleaned workbook for audit visibility but excluded from completed-sales summaries.

## Assumptions and Limitations

Slash date formats were interpreted as `MM/DD/YYYY`, hyphen dates as `DD-MM-YYYY`, and text dates as `DD MMM YYYY`. The allowed discount range was assumed to be 0% to 50%. Conflicting duplicate order IDs were flagged instead of deleted because the correct record could not be confirmed without business approval.

## Screenshots Included

The `screenshots/` folder includes:

- `raw_data_preview.png`
- `cleaned_data_preview.png`
- `pivot_summary_1.png`
- `pivot_summary_2.png`
