---
title:  "MySQL JOIN"
excerpt: ""

categories:
  - MySQL
tags:
  - JOIN

  

toc: true
toc_sticky: true
---

#### JOIN(INNER JOIN)
```sql
SELECT * FROM Categories C
JOIN Products P
  ON C.CategoryID = P.CategoryID;
```

```sql
SELECT 
  C.CategoryID, C.CategoryName, 
  P.ProductName, 
  O.OrderDate,
  D.Quantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID;
```

**JOINしたテーブルグループ化する**
```sql
SELECT 
  C.CategoryName, P.ProductName,
  MIN(O.OrderDate) AS FirstOrder,
  MAX(O.OrderDate) AS LastOrder,
  SUM(D.Quantity) AS TotalQuantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID
GROUP BY C.CategoryID, P.ProductID;
```
#### SELF JOIN

#### LEFT/RIGHT OUTER JOIN
```sql
SELECT
  C.CustomerName, S.SupplierName,
  C.City, C.Country
FROM Customers C
LEFT JOIN Suppliers S
ON C.City = S.City AND C.Country = S.Country;
```
#### CROSS JOIN
```sql
SELECT
  E1.LastName, E2.FirstName
FROM Employees E1
CROSS JOIN Employees E2
ORDER BY E1.EmployeeID;
```
---