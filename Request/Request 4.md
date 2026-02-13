# Request: Segment with Highest Increase in Unique Products (2021 vs 2020)

## 📌 Business Question

Which segment had the most increase in unique products in 2021 compared to 2020?

The final output should contain:

- segment  
- product_count_2020  
- product_count_2021  
- difference  

---

## 🧠 SQL Query

```sql
WITH unique_products_segment AS (
    SELECT
        p.segment,
        s.fiscal_year,
        COUNT(DISTINCT s.product_code) AS unique_products
    FROM dim_product p
    JOIN fact_sales_monthly s
        USING (product_code)
    WHERE s.fiscal_year IN (2020, 2021)
    GROUP BY p.segment, s.fiscal_year
)

SELECT
    segment,

    MAX(CASE WHEN fiscal_year = 2020 
             THEN unique_products END) AS product_count_2020,

    MAX(CASE WHEN fiscal_year = 2021 
             THEN unique_products END) AS product_count_2021,

    MAX(CASE WHEN fiscal_year = 2021 
             THEN unique_products END)
    -
    MAX(CASE WHEN fiscal_year = 2020 
             THEN unique_products END) AS difference,

    ROUND(
        (
            MAX(CASE WHEN fiscal_year = 2021 
                     THEN unique_products END)
            -
            MAX(CASE WHEN fiscal_year = 2020 
                     THEN unique_products END)
        ) * 100.0
        /
        MAX(CASE WHEN fiscal_year = 2020 
                 THEN unique_products END),
        2
    ) AS pct_change

FROM unique_products_segment
GROUP BY segment
ORDER BY pct_change DESC;
```

---

## 📊 Output

![Request 4 Output](Request_4_Output.png)

---

## 💡 Approach Explanation

- Used a CTE to calculate unique product counts per segment per fiscal year.
- Applied conditional aggregation using `CASE WHEN` to pivot fiscal years into columns.
- Calculated absolute difference between 2021 and 2020.
- Computed percentage change using:

  `((value_2021 - value_2020) / value_2020) * 100`


- Sorted results in descending order of percentage change to identify the segment with the highest growth.
