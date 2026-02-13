# Request 1

## 📌 Business Question
Provide the list of markets in which customer "Atliq Exclusive" operates its business in the APAC region.

---

## 🧠 SQL Query

```sql
SELECT DISTINCT market
FROM dim_customer
WHERE customer = "Atliq Exclusive"
AND region = "APAC";
```

---

## 📊 Output

![Output](Request_1_Output.png)


