# Request: Monthly Gross Sales Report for "Atliq Exclusive"

## 📌 Business Question

Generate a complete report of the gross sales amount for the customer **"Atliq Exclusive"** for each month.

This analysis helps identify high-performing and low-performing months to support strategic decision-making.

The final report should contain:

- Month  
- Year  
- Gross_sales_amount  

---

## 🧠 SQL Query

```sql
SELECT
    MONTHNAME(s.date) AS month,
    YEAR(s.date) AS year,
    ROUND(SUM(s.sold_quantity * g.gross_price), 2) AS gross_sales_amount

FROM fact_sales_monthly s

JOIN fact_gross_price g
    ON s.product_code = g.product_code
   AND s.fiscal_year = g.fiscal_year

JOIN dim_customer c
    USING (customer_code)

WHERE c.customer = 'Atliq Exclusive'

GROUP BY
    YEAR(s.date),
    MONTH(s.date),
    MONTHNAME(s.date)

ORDER BY
    YEAR(s.date),
    MONTH(s.date);
```

---

## 📊 Output

![Request 7 Output](../Images/Request_7_Output.png)

---

## 💡 Approach Explanation

- Joined `fact_sales_monthly` with `fact_gross_price` using:
  - `product_code`
  - `fiscal_year`
- Multiplied `sold_quantity` with `gross_price` to compute gross sales.
- Joined `dim_customer` to filter for customer **"Atliq Exclusive"**.
- Aggregated results monthly using `SUM()`.
- Used `MONTHNAME()` and `YEAR()` for clear time-based reporting.
- Ordered results chronologically using `YEAR()` and `MONTH()`.

---

## 📈 Business Insight

This report helps identify:
- Peak sales months  
- Low-performing months  
- Seasonal trends  
- Strategic opportunities for sales optimization  
