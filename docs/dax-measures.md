# DAX Measures

Key measures used across the SuperStore Data Analysis report. Adjust table/column names below to match your actual data model before reusing.

## Core Aggregations

```dax
Sum of Sales = SUM(Orders[Sales])
```

```dax
Sum of Profit = SUM(Orders[Profit])
```

```dax
Sum of Quantity = SUM(Orders[Quantity])
```

```dax
Average of Discount =
AVERAGE(Orders[Discount])
```
> Used as the "Average of ..." KPI card (e.g. average discount or average sales per order — update the column reference to match what the card is actually averaging).

## Ratios / Derived Measures

```dax
Profit Margin % =
DIVIDE([Sum of Profit], [Sum of Sales], 0)
```

```dax
Sales % of Total =
DIVIDE(
    [Sum of Sales],
    CALCULATE([Sum of Sales], ALL(Orders)),
    0
)
```
> Powers the donut chart percentages (Segment breakdown, Payment Mode breakdown).

## Time Intelligence

```dax
Monthly Sales =
CALCULATE(
    [Sum of Sales],
    DATESMTD('Date'[Date])
)
```

```dax
YoY Sales Growth % =
VAR CurrentSales = [Sum of Sales]
VAR PriorYearSales =
    CALCULATE([Sum of Sales], SAMEPERIODLASTYEAR('Date'[Date]))
RETURN
    DIVIDE(CurrentSales - PriorYearSales, PriorYearSales, 0)
```

## Notes

- All measures assume a dedicated `Date` table marked as the official date table, related to `Orders[Order Date]`.
- The **Sum of Sales by Day** forecast chart on the trend page uses Power BI's built-in **Analytics → Forecast** feature (exponential smoothing), not a custom DAX measure.
- Region filtering (Central / East / South / West) is implemented via a slicer/button bookmark on the `Region` column, not a measure.
