# DAX Documentation

All measures, calculated columns, and calculated tables used in `Olist_Project.pbix`.

## Measures

### Total Sales
```dax
Total Sales = SUM(olist_order_items_dataset[price])
```
Sum of item prices across all orders. Excludes freight. This is the 13.59M headline figure.

### Total Freight
```dax
Total Freight = SUM(olist_order_items_dataset[freight_value])
```
Sum of shipping fees charged on items.

### Total Orders
```dax
Total Orders = DISTINCTCOUNT(olist_orders_dataset[order_id])
```
Number of unique orders. Uses `DISTINCTCOUNT` because one order can have several item rows.

### Average Order Value
```dax
Average Order Value = DIVIDE([Total Sales], [Total Orders])
```
Revenue per order. `DIVIDE` returns blank instead of an error when orders are zero (for example, when a slicer filters everything out).

### Avg Review Score
```dax
Avg Review Score = AVERAGE(olist_order_reviews_dataset[review_score])
```
Average customer rating on a 1 to 5 scale.

## Calculated Column

### Delivery Status
```dax
Delivery Status =
IF(
    ISBLANK(olist_orders_dataset[order_delivered_customer_date]),
    BLANK(),
    IF(
        olist_orders_dataset[order_delivered_customer_date] <= olist_orders_dataset[order_estimated_delivery_date],
        "On-time",
        "Late"
    )
)
```
Classifies each delivered order as "On-time" (delivered on or before the estimated date) or "Late". Orders without a delivery date (not yet delivered, canceled, and so on) stay blank so they do not distort the on-time rate. This column drives the 4.3 vs 2.3 review comparison.

## Calculated Table

### Calendar
```dax
Calendar =
ADDCOLUMNS(
    CALENDAR(DATE(2016, 1, 1), DATE(2018, 12, 31)),
    "Year", YEAR([Date]),
    "Quarter", "Q" & QUARTER([Date]),
    "Month No", MONTH([Date]),
    "Month", FORMAT([Date], "MMM")
)
```
Date table covering 2016 to 2018, used for time-based analysis. The dataset ends in August 2018, so late 2018 dates have no orders.

## Notes

- Metrics such as the on-time delivery rate (93.2%) and average delivery time (12.5 days) are built on top of the `Delivery Status` column and the order date fields.
