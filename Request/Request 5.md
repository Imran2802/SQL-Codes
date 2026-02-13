# Request: Products with Highest and Lowest Manufacturing Costs

## 📌 Business Question

Get the products that have the highest and lowest manufacturing costs.

The final output should contain:

- product_code  
- product  
- manufacturing_cost  

---

## 🧠 SQL Query

```sql
SELECT
    m.product_code,
    p.product,
    m.manufacturing_cost
FROM fact_manufacturing_cost m
JOIN dim_product p
    USING (product_code)
WHERE m.manufacturing_cost = (
        SELECT MAX(manufacturing_cost)
        FROM fact_manufacturing_cost
      )
   OR m.manufacturing_cost = (
        SELECT MIN(manufacturing_cost)
        FROM fact_manufacturing_cost
      );
```

---

## 📊 Output

![Request 5 Output](../Images/Request_5_Output.png)

---

## 💡 Approach Explanation

- Joined `fact_manufacturing_cost` with `dim_product` to retrieve product names.
- Used subqueries to identify:
  - Maximum manufacturing cost
  - Minimum manufacturing cost
- Filtered records where manufacturing cost matches either the highest or lowest value.
- This ensures the query returns products at both cost extremes.
