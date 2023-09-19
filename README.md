# SQL Practice Problems
This repository contains my solutions to some SQL practice problems from different authors.

Feel free to contact me on [Linkedin](www.linkedin.com/in/kareem-lemboye-1a0025277) for questions or feedbacks.

## Problem 1  - ["SQL Practice Problems" by Sylvia Moestl Vasilik](http://www.sqlpracticeproblems.com)
This practice problem contains 57 questions separated into three difficulty levels (beginners, intermediate, and advanced). The questions in this book were designed to train its users how to analyze data problems, figure out SQL fundamentals, and to work towards crafting SQL solutions solved using a "learn-by-doing" approach.

**1. Which shippers do we have?**
```
SELECT *
FROM Shippers
```

**2. Certain fields from Categories**
```
SELECT CategoryName, Description
FROM Categories
```

**3. Sales Representatives**
```
SELECT FirstName, LastName, HireDate
FROM Employees
WHERE Title = 'Sales Representative'
```

**4. Sales Representatives in the United States**
```
SELECT FirstName, LastName, HireDate
FROM Employees
WHERE Title = 'Sales Representative' AND Country = 'USA'
```

**5. Orders placed by specific EmployeeID**
```
SELECT OrderID, OrderDate
FROM Orders
WHERE EmployeeID = 5
```

**6. Suppliers and ContactTitles**
```
SELECT SupplierID, ContactName, ContactTitle
FROM Suppliers
WHERE ContactTitle != 'Marketing Manager'
```

**7. Products with “queso” in ProductName**
```
SELECT ProductID, ProductName
FROM Products
WHERE ProductName LIKE '%queso%'
```

**8. 
```
SELECT OrderID, CustomerID, ShipCountry
FROM Orders
WHERE ShipCountry = 'France' OR ShipCountry = 'Belgium'
```

**9. 
```
SELECT OrderID, CustomerID, ShipCountry
FROM Orders
WHERE ShipCountry IN ('Brazil', 'Mexico', 'Argentina', 'Venezula')
```

**10. 
```
SELECT FirstName, LastName, Title, BirthDate
FROM Employees
ORDER BY BirthDate
```

**11. 
```
SELECT FirstName, LastName, Title, CONVERT(DATE, BirthDate) AS DateOnlyBirthDate
FROM Employees
ORDER BY BirthDate
```

**12. 
```
SELECT FirstName, LastName, FirstName + ' ' + LastName AS FullName, CONCAT(FirstName, ' ', LastName) AS FullName
FROM Employees
ORDER BY BirthDate
```

**13. 
```
SELECT OrderID, ProductID, UnitPrice, Quantity, UnitPrice*Quantity AS TotalPrice
FROM OrderDetails
```

**14. 
```
SELECT COUNT(*)
FROM Customers
```

**15. 
```
SELECT MIN(OrderDate)
FROM Orders
```

**16. 
```
SELECT DISTINCT Country
FROM Customers
```

**17. 
```
SELECT ContactTitle, COUNT(ContactTitle) AS TotalContratTitle
FROM Customers
GROUP BY ContactTitle
```

**18. 
```
SELECT P.ProductID, P.ProductName, S.CompanyName
FROM Products AS P
INNER JOIN Suppliers AS S
ON P.SupplierID = S.SupplierID
ORDER BY ProductID
```

**19. 
```
SELECT O.OrderID, CONVERT(DATE, O.OrderDate) AS OrderDateOnly, S.CompanyName
FROM Orders AS O
INNER JOIN Shippers AS S
ON O.ShipVia = S.ShipperID
WHERE OrderID < 10270
ORDER BY OrderID
```

**20. 
```
SELECT C.CategoryName, COUNT(*) AS TotalProduct
FROM Products AS P
INNER JOIN Categories AS C
ON P.CategoryID = C.CategoryID
GROUP BY C.CategoryName
ORDER BY COUNT(*) DESC
```

**21. 
```
SELECT Country, City, COUNT(*)
FROM Customers
GROUP BY Country, City
ORDER BY  COUNT(*) DESC
```

**22. 
```
SELECT ProductID, ProductName, UnitsInStock, ReorderLevel
FROM Products
WHERE UnitsInStock <= ReorderLevel
ORDER BY ProductID
```

**23. 
```
SELECT ProductID, ProductName, UnitsInStock, UnitsOnOrder, ReorderLevel, Discontinued
FROM Products
WHERE ((UnitsInStock + UnitsOnOrder) <= ReorderLevel) AND (Discontinued = 0)
ORDER BY ProductID
```

**24. 
```
SELECT CustomerID, CompanyName, Region
FROM
(SELECT CustomerID, CompanyName, Region,
	CASE
		WHEN Region IS NULL THEN 1
		ELSE 0
	END AS ForNullSort
FROM Customers) AS Temp
ORDER BY ForNullSort, Region, CustomerID
```

**25. 
```
SELECT TOP 3 ShipCountry, AVG(Freight) AS AverageFreight
FROM Orders
GROUP BY ShipCountry
ORDER BY AVG(Freight) DESC
```

**26. 
```
SELECT TOP 3 ShipCountry, AVG(Freight) AS AverageFreight
FROM Orders
WHERE YEAR(OrderDate) = 2015
GROUP BY ShipCountry
ORDER BY AVG(Freight) DESC
```

**27. 
```
SELECT TOP 3 ShipCountry, AVG(Freight) AS AverageFreight
FROM Orders
WHERE OrderDate BETWEEN '20150101' AND '20151231'
GROUP BY ShipCountry
ORDER BY AVG(Freight) DESC
```

**28. 
```
SELECT TOP 3 ShipCountry, AVG(Freight) AS AverageFreight
FROM Orders
WHERE OrderDate
	BETWEEN
		(SELECT DATEADD(MONTH, -12, MAX(OrderDate))
		FROM Orders)
	AND
		(SELECT MAX(OrderDate)
		FROM Orders)
GROUP BY ShipCountry
ORDER BY AVG(Freight) DESC
```

**29. 
```
SELECT E.EmployeeID, E.LastName, OD.OrderID, P.ProductName, OD.Quantity
FROM Employees AS E
INNER JOIN Orders AS O
ON E.EmployeeID = O.EmployeeID
INNER JOIN OrderDetails AS OD
ON O.OrderID = OD.OrderID
INNER JOIN Products AS P
ON OD.ProductID = P.ProductID
```

**30. 
```
SELECT C.CustomerID AS Customer_CustomerID, O.CustomerID AS Orders_CustomerID
FROM Customers AS C
LEFT JOIN Orders AS O
ON C.CustomerID = O.CustomerID
WHERE O.CustomerID IS NULL
```

**31. 
```
SELECT DISTINCT O.CustomerID AS Customer_CustomerID, NO.CustomerID AS Customer_CustomerID
FROM Orders AS O
LEFT JOIN (
	SELECT DISTINCT CustomerID
	FROM Orders
	WHERE EmployeeID = 4
	)AS NO
ON O.CustomerID = NO.CustomerID
WHERE NO.CustomerID IS NULL
```

**32. 
```
SELECT O.CustomerID, C.CompanyName, O.OrderID, SUM(OD.Quantity*OD.UnitPrice) AS TOtalOrderAmount
FROM Orders AS O
INNER JOIN Customers AS C
ON O.CustomerID = C.CustomerID
INNER JOIN OrderDetails AS OD
ON O.OrderID = OD.OrderID
WHERE DATENAME(YEAR, O.OrderDate) = 2016
GROUP BY O.CustomerID, C.CompanyName, O.OrderID
HAVING COUNT(O.OrderID) >= 1 AND SUM(OD.Quantity*OD.UnitPrice) >= 10000
ORDER BY TOtalOrderAmount DESC
```

**33. 
```
SELECT C.CustomerID, C.CompanyName, SUM(OD.Quantity*OD.UnitPrice) AS TotalOrderAmount
FROM Orders AS O
INNER JOIN Customers AS C
ON O.CustomerID = C.CustomerID
INNER JOIN OrderDetails AS OD
ON O.OrderID = OD.OrderID
WHERE DATENAME(YEAR, O.OrderDate) = 2016
GROUP BY C.CustomerID, C.CompanyName
HAVING SUM(OD.Quantity*OD.UnitPrice) >= 15000 
ORDER BY TotalOrderAmount DESC
```

**34. 
```
SELECT C.CustomerID, C.CompanyName, SUM(OD.Quantity*OD.UnitPrice) AS 'Total Without Discount', SUM(OD.Quantity*OD.UnitPrice*(1-OD.Discount)) AS 'Total With Discount'
FROM Orders AS O
INNER JOIN Customers AS C
ON O.CustomerID = C.CustomerID
INNER JOIN OrderDetails AS OD
ON O.OrderID = OD.OrderID
WHERE DATENAME(YEAR, O.OrderDate) = 2016
GROUP BY C.CustomerID, C.CompanyName
HAVING SUM(OD.Quantity*OD.UnitPrice) >= 15000 
ORDER BY 'Total With Discount' DESC
```

**35. 
```
SELECT EmployeeID, OrderID, OrderDate
FROM Orders
WHERE OrderDate = EOMONTH(OrderDate)
ORDER BY EmployeeID
```

**36. 
```
SELECT TOP 10 OrderID, COUNT(*) AS TotalOrderDetails
FROM OrderDetails
GROUP BY OrderID
ORDER BY TotalOrderDetails DESC
```

**37. 
```
SELECT TOP 2 PERCENT OrderID
FROM Orders
ORDER BY NEWID()
```

**38. 
```
SELECT OrderID
FROM OrderDetails
WHERE Quantity >= 60
GROUP BY OrderID, Quantity
HAVING COUNT(OrderID) > 1
ORDER BY OrderID
```

**39. 
```
SELECT OrderID, ProductID, UnitPrice, Quantity, Discount
FROM OrderDetails
WHERE OrderID IN (
	SELECT OrderID
	FROM OrderDetails
	WHERE Quantity >= 60
	GROUP BY OrderID, Quantity
	HAVING COUNT(OrderID) > 1)
ORDER BY OrderID, Quantity
```

**40. 
```
Select Distinct OrderDetails.OrderID ,ProductID ,UnitPrice ,Quantity ,Discount
From OrderDetails
	Join (
		Select OrderID
		From OrderDetails
		Where Quantity >= 60
		Group By OrderID, Quantity
		Having Count(*) > 1 ) PotentialProblemOrders
	on PotentialProblemOrders.OrderID = OrderDetails.OrderID
Order by OrderID, ProductID
```

**41. 
```
SELECT OrderID, CONVERT(DATE,OrderDate) AS OrderDate, CONVERT(DATE,RequiredDate) AS RequiredDate, CONVERT(DATE, ShippedDate) ShippedDate
FROM Orders
WHERE CONVERT(DATE, ShippedDate) >= CONVERT(DATE,RequiredDate)
ORDER BY OrderID
```

**42. 
```
SELECT E.EmployeeID, E.LastName, COUNT(O.OrderID) AS TotalLateOrders
FROM Orders AS O
INNER JOIN Employees AS E
ON O.EmployeeID = E.EmployeeID
WHERE ShippedDate >= RequiredDate
GROUP BY E.EmployeeID, E.LastName
ORDER BY COUNT(O.OrderID) DESC, E.EmployeeID
```

**43. 
```
SELECT E.EmployeeID, E.LastName, NT.AllOrders, COUNT(O.OrderID) AS LateOrders
FROM Orders AS O
INNER JOIN Employees AS E
ON O.EmployeeID = E.EmployeeID
INNER JOIN
	(SELECT ENT.EmployeeID, ENT.LastName, COUNT(ONT.OrderID) AS AllOrders
	FROM Orders AS ONT
	INNER JOIN Employees AS ENT
	ON ONT.EmployeeID = ENT.EmployeeID
	GROUP BY ENT.EmployeeID, ENT.LastName) AS NT
ON NT.EmployeeID = O.EmployeeID
WHERE ShippedDate >= RequiredDate
GROUP BY E.EmployeeID, E.LastName, NT.AllOrders
ORDER BY E.EmployeeID
```

**44. 
```
SELECT E.EmployeeID, E.LastName, COUNT(O.OrderID) AS AllOrders, NT.LateOrders
FROM Orders AS O
INNER JOIN Employees AS E
ON O.EmployeeID = E.EmployeeID
LEFT JOIN
	(SELECT ENT.EmployeeID, ENT.LastName, COUNT(ONT.OrderID) AS LateOrders
	FROM Orders AS ONT
	INNER JOIN Employees AS ENT
	ON ONT.EmployeeID = ENT.EmployeeID
	WHERE ShippedDate >= RequiredDate
	GROUP BY ENT.EmployeeID, ENT.LastName) AS NT
ON NT.EmployeeID = O.EmployeeID
GROUP BY E.EmployeeID, E.LastName, NT.LateOrders
ORDER BY E.EmployeeID
```

**45. 
```
SELECT E.EmployeeID, E.LastName, COUNT(O.OrderID) AS AllOrders, COALESCE(NT.LateOrders, 0) AS LateOrders
FROM Orders AS O
INNER JOIN Employees AS E
ON O.EmployeeID = E.EmployeeID
LEFT JOIN
	(SELECT ENT.EmployeeID, ENT.LastName, COUNT(ONT.OrderID) AS LateOrders
	FROM Orders AS ONT
	INNER JOIN Employees AS ENT
	ON ONT.EmployeeID = ENT.EmployeeID
	WHERE ShippedDate >= RequiredDate
	GROUP BY ENT.EmployeeID, ENT.LastName) AS NT
ON NT.EmployeeID = O.EmployeeID
GROUP BY E.EmployeeID, E.LastName, NT.LateOrders
ORDER BY E.EmployeeID
```

**46. 
```
SELECT E.EmployeeID, E.LastName, COUNT(O.OrderID) AS AllOrders, COALESCE(NT.LateOrders, 0) AS LateOrders, CAST(COALESCE(NT.LateOrders, 0) AS FLOAT)/CAST(COUNT(O.OrderID) AS FLOAT) AS 'Percenta Late Orders'
FROM Orders AS O
INNER JOIN Employees AS E
ON O.EmployeeID = E.EmployeeID
LEFT JOIN
	(SELECT ENT.EmployeeID, ENT.LastName, COUNT(ONT.OrderID) AS LateOrders
	FROM Orders AS ONT
	INNER JOIN Employees AS ENT
	ON ONT.EmployeeID = ENT.EmployeeID
	WHERE ShippedDate >= RequiredDate
	GROUP BY ENT.EmployeeID, ENT.LastName) AS NT
ON NT.EmployeeID = O.EmployeeID
GROUP BY E.EmployeeID, E.LastName, NT.LateOrders
ORDER BY E.EmployeeID
```

**47. 
```
SELECT E.EmployeeID, E.LastName, COUNT(O.OrderID) AS AllOrders, COALESCE(NT.LateOrders, 0) AS LateOrders, ROUND(CAST(COALESCE(NT.LateOrders, 0) AS FLOAT)/CAST(COUNT(O.OrderID) AS FLOAT), 2) AS 'Percenta Late Orders'
FROM Orders AS O
INNER JOIN Employees AS E
ON O.EmployeeID = E.EmployeeID
LEFT JOIN
	(SELECT ENT.EmployeeID, ENT.LastName, COUNT(ONT.OrderID) AS LateOrders
	FROM Orders AS ONT
	INNER JOIN Employees AS ENT
	ON ONT.EmployeeID = ENT.EmployeeID
	WHERE ShippedDate >= RequiredDate
	GROUP BY ENT.EmployeeID, ENT.LastName) AS NT
ON NT.EmployeeID = O.EmployeeID
GROUP BY E.EmployeeID, E.LastName, NT.LateOrders
ORDER BY E.EmployeeID
```

**48. 
```
SELECT C.CustomerID, C.CompanyName, SUM(OD.Quantity*OD.UnitPrice) AS 'Total Order Amount',
	CASE
		WHEN SUM(OD.Quantity*OD.UnitPrice) < 1000 THEN 'Low'
		WHEN SUM(OD.Quantity*OD.UnitPrice) BETWEEN 1000 AND 5000 THEN 'Medium'
		WHEN SUM(OD.Quantity*OD.UnitPrice) BETWEEN 5000 AND 10000 THEN 'High'
		WHEN SUM(OD.Quantity*OD.UnitPrice) > 5000 THEN 'Very High'
	END AS 'Customer Group'
FROM Orders AS O
INNER JOIN Customers AS C
ON O.CustomerID = C.CustomerID
INNER JOIN OrderDetails AS OD
ON O.OrderID = OD.OrderID
WHERE DATEPART(YEAR, OrderDate) = 2016
GROUP BY C.CustomerID, C.CompanyName
ORDER BY C.CustomerID
```

**49. 
```
SELECT C.CustomerID, C.CompanyName, SUM(OD.Quantity*OD.UnitPrice) AS 'Total Order Amount',
	CASE
		WHEN SUM(OD.Quantity*OD.UnitPrice) >= 0 AND SUM(OD.Quantity*OD.UnitPrice) < 1000 THEN 'Low'
		WHEN SUM(OD.Quantity*OD.UnitPrice) >= 1000 AND SUM(OD.Quantity*OD.UnitPrice) < 5000 THEN 'Medium'
		WHEN SUM(OD.Quantity*OD.UnitPrice) >= 5000 AND SUM(OD.Quantity*OD.UnitPrice) < 10000 THEN 'High'
		WHEN SUM(OD.Quantity*OD.UnitPrice) > 5000 THEN 'Very High'
	END AS 'Customer Group'
FROM Orders AS O
INNER JOIN Customers AS C
ON O.CustomerID = C.CustomerID
INNER JOIN OrderDetails AS OD
ON O.OrderID = OD.OrderID
WHERE DATEPART(YEAR, OrderDate) = 2016
GROUP BY C.CustomerID, C.CompanyName
ORDER BY C.CustomerID
```

**50. 
```
SELECT CustomerGroup, COUNT(CustomerGroup) AS TotalInGroup, (CAST(COUNT(CustomerGroup) AS FLOAT)/CAST(
	(SELECT COUNT(DISTINCT O.CustomerID)
	FROM Orders AS O
	INNER JOIN OrderDetails AS OD
	ON O.OrderID = OD.OrderID
	WHERE DATEPART(YEAR, OrderDate) = 2016) AS FLOAT)) AS PercentageInGroup
FROM
	(SELECT O.CustomerID, SUM(OD.Quantity*OD.UnitPrice) AS 'Total Order Amount',
		CASE
			WHEN SUM(OD.Quantity*OD.UnitPrice) >= 0 AND SUM(OD.Quantity*OD.UnitPrice) < 1000 THEN 'Low'
			WHEN SUM(OD.Quantity*OD.UnitPrice) >= 1000 AND SUM(OD.Quantity*OD.UnitPrice) < 5000 THEN 'Medium'
			WHEN SUM(OD.Quantity*OD.UnitPrice) >= 5000 AND SUM(OD.Quantity*OD.UnitPrice) < 10000 THEN 'High'
			WHEN SUM(OD.Quantity*OD.UnitPrice) > 5000 THEN 'Very High'
		END AS 'CustomerGroup'
	FROM Orders AS O
	INNER JOIN OrderDetails AS OD
	ON O.OrderID = OD.OrderID
	WHERE DATEPART(YEAR, OrderDate) = 2016
	GROUP BY O.CustomerID) AS CustomerGroupTable
GROUP BY CustomerGroup
ORDER BY PercentageInGroup DESC
```

**51. 
```
SELECT CustomerID, CompanyName, [Total Order Amount], CGT.CustomerGroupName AS 'Customer Group Name'
FROM
	(
	SELECT C.CustomerID, C.CompanyName, SUM(OD.Quantity*OD.UnitPrice) AS 'Total Order Amount'
	FROM Orders AS O
	INNER JOIN Customers AS C
	ON O.CustomerID = C.CustomerID
	INNER JOIN OrderDetails AS OD
	ON O.OrderID = OD.OrderID
	WHERE DATEPART(YEAR, OrderDate) = 2016
	GROUP BY C.CustomerID, C.CompanyName
	) AS NT
INNER JOIN CustomerGroupThresholds AS CGT
ON NT.[Total Order Amount] > CGT.RangeBottom AND NT.[Total Order Amount] < CGT.RangeTop
ORDER BY CompanyName
```

**Or**
```
SELECT CustomerID, CompanyName, [Total Order Amount], 
	(
	SELECT CustomerGroupName
	FROM CustomerGroupThresholds AS CGT
	WHERE NT.[Total Order Amount] > CGT.RangeBottom AND NT.[Total Order Amount] < CGT.RangeTop
	) AS 'Customer Group Name'
FROM
	(
	SELECT C.CustomerID, C.CompanyName, SUM(OD.Quantity*OD.UnitPrice) AS 'Total Order Amount'
	FROM Orders AS O
	INNER JOIN Customers AS C
	ON O.CustomerID = C.CustomerID
	INNER JOIN OrderDetails AS OD
	ON O.OrderID = OD.OrderID
	WHERE DATEPART(YEAR, OrderDate) = 2016
	GROUP BY C.CustomerID, C.CompanyName
	) AS NT
ORDER BY CompanyName
```

**52. 
```
SELECT Country
FROM Customers
UNION
SELECT Country
FROM Suppliers;
```

**53. 
```
SELECT DISTINCT ST.Country AS SupplierCountry, CT.Country AS CustomerCountry
FROM Customers AS CT
FULL JOIN Suppliers AS ST
ON CT.Country = ST.Country
ORDER BY ST.Country, CT.Country
```

**Or**
```
WITH
CNT
AS (
	SELECT DISTINCT Country
	FROM Customers
	),
SNT
AS (
	SELECT DISTINCT Country
	FROM Suppliers
	)
SELECT SNT.Country AS SupplierCountry, CNT.Country AS CustomerCountry
FROM CNT
FULL JOIN SNT
ON CNT.Country = SNT.Country
ORDER BY CNT.Country
```

**54. 
```
SELECT
	(
	SELECT NNT.Country
	FROM
		(
		SELECT Country
		FROM Customers
		UNION
		SELECT Country
		FROM Suppliers
		) AS NNT
	WHERE NNT.Country = NT.SupplierCountry OR NNT.Country = NT.CustomerCountry
	) AS Country, TotalSupplier, TotalCustomer
FROM
(
SELECT ST.Country AS SupplierCountry, COUNT(DISTINCT ST.CompanyName) AS TotalSupplier, CT.Country AS CustomerCountry, COUNT(DISTINCT CT.CompanyName) AS TotalCustomer
FROM Customers AS CT
FULL JOIN Suppliers AS ST
ON CT.Country = ST.Country
GROUP BY CT.Country, ST.Country
) AS NT
ORDER BY Country
```

**Or**
```
WITH
CNT
AS (
	SELECT Country, COUNT(CompanyName) AS TotalCustomer
	FROM Customers
	GROUP BY Country
	),
SNT
AS (
	SELECT Country, COUNT(CompanyName) AS TotalSupplier
	FROM Suppliers
	GROUP BY Country
	),
CUS
AS (
	SELECT Country
	FROM Customers
	UNION
	SELECT Country
	FROM Suppliers
	)
SELECT CUS.Country, COALESCE(TotalSupplier, 0), COALESCE(TotalCustomer, 0)
FROM CNT
FULL JOIN SNT
ON CNT.Country = SNT.Country
INNER JOIN CUS
ON CUS.Country = CNT.Country OR CUS.Country = SNT.Country
ORDER BY CUS.Country
```

**55. 
```
SELECT ShipCountry, CustomerID, OrderID, CONVERT(DATE, OrderDate) AS OrderDate
FROM Orders
WHERE OrderDate IN (
	SELECT MIN(OrderDate)
	FROM Orders
	GROUP BY ShipCountry)
ORDER BY ShipCountry, OrderDate
```

**56. 
```
SELECT INO.CustomerID, INO.OrderID AS InitialOrderID, CONVERT(DATE, INO.OrderDate) AS InitialOrderDate, NWO.OrderID AS NextOrderID, CONVERT(DATE, NWO.OrderDate) AS NextOrderDate, DATEDIFF(DAY, INO.OrderDate, NWO.OrderDate) AS DaysBetweenOrders
FROM Orders AS INO, Orders AS NWO
WHERE 
	INO.CustomerID = NWO.CustomerID AND 
	INO.OrderID < NWO.OrderID AND
	DATEDIFF(DAY, INO.OrderDate, NWO.OrderDate) <= 5
ORDER BY CustomerID, INO.OrderID, NWO.OrderID
```

**57. 
```
SELECT CustomerID, InitialOrderID, CONVERT(DATE, InitialOrderDate) AS InitialOrderDate, CONVERT(DATE, NextOrderDate) AS NextOrderDate, (
	SELECT OrderID
	FROM ORDERS
	WHERE NT.CustomerID = CustomerID AND NT.NextOrderDate = OrderDate) AS NextOrderID
FROM (
	SELECT CustomerID, OrderID AS InitialOrderID, OrderDate AS InitialOrderDate,
		LEAD(OrderDate, 1) 
			OVER(ORDER BY CustomerID, OrderID, OrderDate) AS NextOrderDate
	FROM Orders) AS NT
WHERE
	InitialOrderDate < NextOrderDate AND
	DATEDIFF(DAY, InitialOrderDate, NextOrderDate) <= 5
ORDER BY CustomerID
```
