# Request: Quarter with Maximum Total Sold Quantity (FY 2020)

## 📌 Business Question

In which quarter of fiscal year 2020 did the company achieve the maximum total sold quantity?

The final output should contain:

- Quarter  
- total_sold_quantity  

Results should be sorted by total_sold_quantity in descending order.

---

## 🧠 SQL Query

```sql
SELECT
    CONCAT("Q", CEIL((MONTH(DATE_ADD(date, INTERVAL 4 MONTH))) / 3)) AS quarter,
    SUM(sold_quantity) AS total_sold_quantity

FROM fact_sales_monthly

WHERE fiscal_year = 2020

GROUP BY
    CONCAT("Q", CEIL((MONTH(DATE_ADD(date, INTERVAL 4 MONTH))) / 3))

ORDER BY
    total_sold_quantity DESC;
```

---

## 📊 Output

![Request 8 Output](Request_8_Output.png)

---

## 💡 Approach Explanation

- Since the fiscal year starts in **September**, the date was shifted forward by 4 months using:
  
  `DATE_ADD(date, INTERVAL 4 MONTH)`

- Extracted the month using `MONTH()` and calculated the fiscal quarter using:

  \[
  CEIL(MONTH / 3)
  \]

- Used `CONCAT("Q", ...)` to label quarters as Q1, Q2, Q3, Q4.
- Aggregated total sold quantity using `SUM()`.
- Sorted results in descending order to identify the quarter with the highest sales.

---

## 📈 Business Insight

This analysis helps identify:

- Peak-performing fiscal quarters  
- Seasonal demand patterns  
- Strategic planning opportunities  
