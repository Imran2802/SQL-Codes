# Request: Channel Contribution to Gross Sales (FY 2021)

## 📌 Business Question

Which channel contributed the most to gross sales in fiscal year 2021?

The final output should contain:

- channel  
- gross_sales_mln  
- percentage  

---

## 🧠 SQL Query

```sql
WITH channel_sales AS (
    SELECT
        c.channel,
        ROUND(SUM(s.sold_quantity * g.gross_price) / 1000000, 2) AS gross_sales_mln
    FROM fact_sales_monthly s
    JOIN fact_gross_price g
        USING (product_code, fiscal_year)
    JOIN dim_customer c
        USING (customer_code)
    WHERE s.fiscal_year = 2021
    GROUP BY c.channel
)

SELECT
    channel,
    gross_sales_mln,
    ROUND(
        gross_sales_mln * 100 
        / SUM(gross_sales_mln) OVER (),
        2
    ) AS percentage
FROM channel_sales
ORDER BY gross_sales_mln DESC;
```

---

## 📊 Output

![Request 9 Output](Request_9_Output.png)

---

## 💡 Approach Explanation

- Joined:
  - `fact_sales_monthly`
  - `fact_gross_price`
  - `dim_customer`
- Calculated gross sales as:

  `sold_quantity * gross_price`


- Converted sales into millions by dividing by 1,000,000.
- Used a CTE to aggregate gross sales by channel.
- Applied a window function:

  ```
  SUM(gross_sales_mln) OVER ()
  ```

  to calculate total sales across all channels.
- Computed percentage contribution for each channel.
- Sorted results in descending order to identify the highest-performing channel.

---

## 📈 Business Insight

This analysis helps:

- Identify the most profitable sales channel  
- Understand revenue distribution  
- Support channel-level strategy and investment decisions  
