# For each loops with LATERAL Joins

When joining multiple tables, the rows of both tables are linked together based on some conditions. However, when the result should e.g. be limited to the last three bought products for every customer, the standard join clause will not work: The joined bought products table can not be limited to only include three rows for every customer. That is not how joins do work, all rows matching the join criteria are included. But with lateral joins, a subquery will be joined instead of a table which is executed once for every customer resulting in a for-each loop equivalent in SQL.

```
SELECT customers.*, recent_sales.*
FROM customers
LEFT JOIN LATERAL (
  SELECT *
  FROM sales
  WHERE sales.customer_id = customers.customer_id
  ORDER BY created_at DESC
  LIMIT 3
) AS recent_sales ON true;
```

