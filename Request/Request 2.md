# Request: Percentage of Unique Product Increase (2021 vs 2020)

## 📌 Business Question

What is the percentage of unique product increase in 2021 compared to 2020?

The final output should contain:

- unique_products_2020  
- unique_products_2021  
- percentage_chg  

---

## 🧠 SQL Query

```sql
WITH yearly_unique_products AS (
    SELECT
        fiscal_year,
        COUNT(DISTINCT product_code) AS unique_products
    FROM fact_sales_monthly
    WHERE fiscal_year IN (2020, 2021)
    GROUP BY fiscal_year
)

SELECT
    MAX(CASE WHEN fiscal_year = 2020 
             THEN unique_products END) AS unique_products_2020,

    MAX(CASE WHEN fiscal_year = 2021 
             THEN unique_products END) AS unique_products_2021,

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
    ) AS percentage_chg

FROM yearly_unique_products;
```

---

## 📊 Output

![Output Screenshot](../Images/Request_2_Output.png)

---

## 💡 Approach Explanation

- Used a CTE (`yearly_unique_products`) to calculate distinct product counts per fiscal year.
- Applied conditional aggregation using `CASE WHEN` to pivot results into separate columns.
- Calculated percentage change using the formula:

  [
   {(2021 - 2020)}/{2020}* 100
  ]

- Used `ROUND()` to limit the percentage to 2 decimal places.
