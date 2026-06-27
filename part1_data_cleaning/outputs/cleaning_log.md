# Cleaning Log – Part 1 Data Cleaning

## 1. Issues Found

The raw order dataset contained the following issues:

- Extra spaces and inconsistent capitalization in text fields.
- Missing values in `region`, `ship_mode`, and `discount`.
- Similar category/status values written in different ways, such as uppercase, lowercase, and extra internal spaces.
- Mixed date formats in `order_date` and `ship_date`.
- Ship dates earlier than order dates.
- Exact duplicate rows.
- Duplicate `order_id` values with conflicting information.
- Missing, negative, and above-range discount values.
- Sales and profit calculation mismatches.
- Cancelled, failed, refunded, pending, and returned records mixed with completed sales records.

## 2. Cleaning Actions Performed

- The original dataset was preserved unchanged as `data/raw_orders.xlsx`.
- A separate cleaned workbook was created as `data/cleaned_orders.xlsx`.
- Exact duplicate copies were removed while keeping the first occurrence.
- Conflicting duplicate order IDs were retained and flagged instead of being deleted.
- Text fields were standardized by trimming spaces, cleaning case inconsistencies, and removing unwanted characters where applicable.
- `region` and `ship_mode` missing values were filled as `Unknown`.
- Date fields were parsed and standardized into a consistent Excel date format.
- `shipping_delay_days`, `order_month`, and `order_year` were created.
- `cleaned_discount`, `calculated_sales`, `calculated_profit`, and `profit_margin` were created.
- `data_quality_flag` was added to classify rows as `Clean`, `Warning`, or `Invalid`.
- Non-completed or non-paid orders were excluded from completed-sales pivot summaries.
- Refunded, cancelled, failed, and returned records were summarized separately.

## 3. Business Rules Applied

| Rule Area | Decision |
|---|---|
| Missing region | Filled as `Unknown` and flagged. |
| Missing ship_mode | Filled as `Unknown` and flagged. |
| Missing discount | Treated as 0 only when quantity, unit price, sales, cost, and profit were valid. |
| Negative discount | Flagged as invalid. |
| Discount above allowed range | Discounts above 50% were flagged as invalid. |
| Cancelled orders | Excluded from final completed-sales summaries. |
| Failed payments | Excluded from final completed-sales summaries. |
| Refunded orders | Summarized separately. |
| Ship date before order date | Flagged as invalid shipping record. |
| Exact duplicate rows | Removed after preserving first occurrence. |
| Conflicting duplicate order IDs | Retained and flagged for review. |

## 4. Assumptions Made

- Slash dates were interpreted as `MM/DD/YYYY`.
- Hyphen dates were interpreted as `DD-MM-YYYY`.
- Text dates such as `21 Jul 2024` were interpreted as `DD MMM YYYY`.
- Discount values were assumed to be decimals unless shown with `%`.
- The allowed discount range was assumed to be 0% to 50%.
- Completed-sales summaries include only records with `payment_status = Paid`, `order_status = Completed`, and no invalid data quality flag.
- Returned, cancelled, refunded, failed, and pending records are not treated as completed sales.

## 5. Records Removed

Exact duplicate rows removed: **20**

Conflicting duplicate order IDs removed: **0**

Conflicting duplicate records flagged for review: **24**

## 6. Records Flagged

Clean records: **505**

Warning records: **334**

Invalid records: **73**

## 7. Key Counts

| Issue | Count |
|---|---:|
| Missing region filled as Unknown | 25 |
| Missing ship mode filled as Unknown | 21 |
| Missing discounts treated as 0 | 18 |
| Negative discounts | 15 |
| Discounts above allowed range | 15 |
| Ship date before order date | 21 |
| Sales/profit mismatches | 83 |

## 8. Limitations

The cleaning process was based only on the fields available in the dataset. Some decisions required assumptions because source-system confirmation was unavailable. Conflicting duplicate order IDs were not deleted because the correct version could not be confirmed. These records were retained and clearly flagged for business review.
