# Request: Unique Product Count by Segment

## 📌 Business Question

Provide a report with all the unique product counts for each segment and sort them in descending order of product counts.

The final output should contain:

- segment  
- product_count  

---

## 🧠 SQL Query

```sql
SELECT
    segment,
    COUNT(DISTINCT product_code) AS product_count
FROM dim_product
GROUP BY segment
ORDER BY product_count DESC;
```

---

## 📊 Output

![Request 3 Output](../Images/Request_3_Output.png)

---

## 💡 Approach Explanation

- Used `COUNT(DISTINCT product_code)` to calculate unique products within each segment.
- Grouped results using `GROUP BY segment`.
- Sorted results in descending order using `ORDER BY product_count DESC`.
- This ensures the segment with the highest number of unique products appears first.
