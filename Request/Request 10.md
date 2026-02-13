# Request: Top 3 Products by Division (FY 2021)

## 📌 Business Question

Get the Top 3 products in each division that have the highest total sold quantity in fiscal year 2021.

The final output should contain:

- division  
- product_code  
- total_sold_quantity  

---

## 🧠 SQL Query

```sql
WITH product_sales AS (
    SELECT
        p.division,
        s.product_code,
        SUM(s.sold_quantity) AS total_sold_quantity
    FROM fact_sales_monthly s
    JOIN dim_product p
        USING (product_code)
    WHERE s.fiscal_year = 2021
    GROUP BY
        p.division,
        s.product_code
),

ranked_products AS (
    SELECT
        *,
        RANK() OVER (
            PARTITION BY division
            ORDER BY total_sold_quantity DESC
        ) AS rnk
    FROM product_sales
)

SELECT
    division,
    product_code,
    total_sold_quantity
FROM ranked_products
WHERE rnk <= 3
ORDER BY division, rnk;
```

---

## 📊 Output

![Request 10 Output](../Images/Request_10_Output.png)

---

## 💡 Approach Explanation

- First aggregated total sold quantity per product within each division.
- Used a CTE (`product_sales`) to calculate total sales per product.
- Applied a window function:

  ```
  RANK() OVER (PARTITION BY division ORDER BY total_sold_quantity DESC)
  ```

  to rank products within each division.
- Filtered top 3 products using `WHERE rnk <= 3`.
- Ordered results by division and rank for structured output.

---

## 📈 Business Insight

This analysis helps:

- Identify best-performing products within each division  
- Support product strategy decisions  
- Focus marketing and supply chain efforts on high-demand items  
