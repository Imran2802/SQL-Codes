# Request: Top 5 Customers with Highest Average Pre-Invoice Discount (India, FY2021)

## 📌 Business Question

Generate a report containing the top 5 customers who received the highest average pre-invoice discount percentage for fiscal year 2021 in the Indian market.

The final output should contain:

- customer_code  
- customer  
- average_discount_percentage  

---

## 🧠 SQL Query

```sql
SELECT
    c.customer_code,
    c.customer,
    AVG(p.pre_invoice_discount_pct) AS average_discount_percentage
FROM dim_customer c
JOIN fact_pre_invoice_deductions p
    USING (customer_code)
WHERE p.fiscal_year = 2021
  AND c.market = "India"
GROUP BY
    c.customer_code,
    c.customer
ORDER BY
    average_discount_percentage DESC
LIMIT 5;
```

---

## 📊 Output

![Request 6 Output](Request_6_Output.png)

---

## 💡 Approach Explanation

- Joined `dim_customer` with `fact_pre_invoice_deductions` using `customer_code`.
- Filtered records for:
  - Fiscal Year = 2021
  - Market = India
- Used `AVG()` to calculate the average pre-invoice discount percentage per customer.
- Grouped results by customer to ensure accurate aggregation.
- Sorted results in descending order to identify customers with the highest average discount.
- Used `LIMIT 5` to retrieve the top 5 customers.
