CREATE DATABASE PracticeDecember2022;

USE PracticeDecember2022;

CREATE SCHEMA PatientAdminstration;

CREATE TABLE PatientAdminstration.Patient
(
  PatientId INT PRIMARY KEY IDENTITY(1,1),
  PatientName VARCHAR(20) NOT NULL,
  BirthDate DATE NOT NULL CHECK(BirthDate <= GETDATE()),
  Gender VARCHAR(1) CHECK(Gender IN('M','F','O','U')),
  LastModified DATE DEFAULT GETDATE()
);

CREATE TABLE PatientAdminstration.Patient
(
  PatientId INT PRIMARY KEY IDENTITY(1001,1),
  PatientName VARCHAR(20) NOT NULL,
  BirthDate DATE NOT NULL CHECK(BirthDate <= GETDATE()),
  Gender VARCHAR(1) CHECK(Gender IN('M','F','O','U')),
  LastModified DATE DEFAULT GETDATE()
);

DROP TABLE PatientAdminstration.Patient;

INSERT INTO PatientAdminstration.Patient (PatientName, BirthDate, Gender) VALUES ('Tanmay', '14-Jul-1997', 'M');

INSERT INTO PatientAdminstration.Patient (PatientName, BirthDate, Gender) VALUES 
('Vishal', '14-Nov-1997', 'M'),
('Vaidehi', '24-Nov-2000', 'F'),
('Prasad', '20-Nov-1997', 'M');

INSERT INTO PatientAdminstration.Patient (PatientName, BirthDate, Gender) VALUES 
('James', '14-Nov-1997', NULL),
('Kim', '24-Nov-2000', NULL),
('John', '20-Nov-1997', 'M');

SELECT * FROM PatientAdminstration.Patient;

CREATE TABLE PatientAdminstration.PatientRelative
(
   RelativeId INT PRIMARY KEY IDENTITY(1,1),
   RelativeName VARCHAR(20) NOT NULL,
   Relation VARCHAR(20),
   PatientId INT REFERENCES PatientAdminstration.Patient(PatientId) ON DELETE CASCADE NOT NULL
);

INSERT INTO PatientAdminstration.PatientRelative (RelativeName, Relation, PatientId) VALUES 
('Shirish', 'Father', 1001),
('Harshal', 'Brother', 1002),
('Tanmay', 'Brother', 1001);

SELECT * FROM PatientAdminstration.PatientRelative;

UPDATE PatientAdminstration.Patient SET Gender = 'M'
WHERE PatientID = 1006;

DELETE FROM PatientAdminstration.Patient
WHERE PatientID = 1005;
--------------------------------------------------------------------------------------------------------------------------------
USE AdventureWorks2019;

SELECT * FROM Production.Product;

SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product;

SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product
WHERE StandardCost > 1000;

SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product
WHERE Color = 'Black';

SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product
WHERE Color IN ('Red', 'Green', 'Blue');
-- Alternative Query
SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product
WHERE Color = 'Red' OR Color = 'Red' OR Color = 'Red';

SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product
WHERE Color IN ('Red', 'Green', 'Blue') AND  StandardCost > 1000;

SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product
WHERE Color IN ('Red', 'Blue') OR  StandardCost > 1500;




--------------------------------------------------------------------------------------------------------------------------------
USE PracticeDecember2022;

CREATE SCHEMA Person;

CREATE TABLE Person.Person
(
   PersonID INT PRIMARY KEY IDENTITY(1,1),
   Title VARCHAR(10) NOT NULL CHECK(Title IN('Mr.', 'Mrs.', 'Dr.')),
   FristName VARCHAR(20) NOT NULL,
   MiddleName VARCHAR(20) NOT NULL,
   LastName VARCHAR(20) NOT NULL,
   Gender VARCHAR(1) CHECK(Gender IN('M', 'F', 'O', 'U')),
   ModifiedDate DATE DEFAULT GETDATE()
);

CREATE SCHEMA Sales;

CREATE TABLE Sales.Country
(
   CountryID INT PRIMARY KEY IDENTITY(1,1),
   CountryName VARCHAR(20) NOT NULL
);

CREATE TABLE Sales.Territory
(
   TerritoryID INT PRIMARY KEY IDENTITY(1,1),
   TerritoryName VARCHAR(20) NOT NULL,
   CountryID INT REFERENCES Sales.Country(CountryID)
);

CREATE TABLE Sales.Customer
(
   CustomerID INT PRIMARY KEY IDENTITY(1,1),
   PersonID INT REFERENCES Person.Person(PersonID),
   TerritoryID INT REFERENCES Sales.Territory(TerritoryID),
   CutomerGrade VARCHAR(1) CHECK(CutomerGrade IN('A','B','C','D','O'))
);

CREATE TABLE Sales.SalesOrderHeader
(
   SalesOrderHeaderID INT PRIMARY KEY IDENTITY(1,1),
   OrderDate DATE DEFAULT GETDATE(),
   CustomerID INT REFERENCES Sales.Customer(CustomerID),
   SalesPersonId INT REFERENCES HumanResources.Employee(EmployeeID)
);

CREATE TABLE Sales.SalesOrderDetails
(
    SalesOrderDetails INT PRIMARY KEY IDENTITY(1,1),
	SalesOrderHeaderID INT REFERENCES Sales.SalesOrderHeader(SalesOrderHeaderID),
	ProductID INT REFERENCES Production.Product(ProductID),
	OrderQuantity INT NOT NULL,
);

CREATE SCHEMA HumanResources;

CREATE TABLE HumanResources.Department
(
    DepartmentID INT PRIMARY KEY IDENTITY(1,1),
	DepartmentName VARCHAR(20) NOT NULL
);

CREATE TABLE HumanResources.Employee
(
    EmployeeID INT PRIMARY KEY IDENTITY(1,1),
	Designation VARCHAR(20) NOT NULL,
	MangerID INT REFERENCES HumanResources.Employee(EmployeeID),
	DateOfJoining DATE NOT NULL CHECK(DateOfJoining <= GETDATE()),
	DepartmentID INT REFERENCES HumanResources.Department(DepartmentID),
	PersonId INT REFERENCES Person.Person(PersonId)
);

CREATE SCHEMA Production;

CREATE TABLE Production.ProductionCategory
(
    ProductionCategoryID INT PRIMARY KEY IDENTITY(1,1),
	ProductionCategoryName VARCHAR(50) NOT NULL
);

CREATE TABLE Production.ProductionSubCategory
(
    ProductionSubCategoryID INT PRIMARY KEY IDENTITY(1,1),
    ProductionSubCategoryName VARCHAR(50) NOT NULL,
	ProductionCategoryID INT REFERENCES Production.ProductionCategory(ProductionCategoryID)
);

CREATE TABLE Production.Product
(
    ProductID INT PRIMARY KEY IDENTITY(1,1),
	ProductName VARCHAR(20) NOT NULL,
	ProductCost MONEY NOT NULL,
	QuatityInStock INT NOT NULL,
	ProductionSubCategoryID INT REFERENCES Production.ProductionSubCategory(ProductionSubCategoryID)
);

--------------------------------------------------------------------------------------------------------------------

SELECT COUNT(*) FROM EMPLOYEE
GROUP BY (SELECT DEPARTMENTID FROM DEPARTMENT WHERE DEPARTMENTNAME = 'IT');
---------------------------------------------------------------------------------------------------------------------
USE AdventureWorks2019;

SELECT * FROM Production.Product;

SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Color = 'Black' AND ProductSubcategoryID IN (6, 12);
-------------------------------------------------------------------------------------------------------------
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE ProductSubcategoryID <> 12;

SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE ProductSubcategoryID != 12;

SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE ProductSubcategoryID NOT IN (12);
-------------------------------------------------------------------------------------------------------------
--Handling Null values
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Color IS NULL AND ProductSubcategoryID IS NULL;

SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Color IS NOT NULL;
-------------------------------------------------------------------------------------------------------------
--Get all the records which starts with letter Ch
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Name LIKE 'Ch%';
--Get all the records which ends with letter d
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Name LIKE '%d';
--Get all the products which have ball in its name
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Name LIKE '%Ball%';
--Get all the products which starts with letter a to c
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Name LIKE '[a-c]%';
------------------------------------------------------------------------------------------------------------
--Get all the black products costing more than 1000 and less than 2000
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Color = 'Black' AND (StandardCost > 1000 AND StandardCost < 2000);

SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
WHERE Color = 'Black' AND (StandardCost BETWEEN 1000 AND 2000);
------------------------------------------------------------------------------------------------------------

SELECT * FROM HumanResources.Employee;

SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee;

--Get all the single employees
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE MaritalStatus = 'S';

--Get all the non single employees
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE MaritalStatus <> 'S';

--Get all the single female employees
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE Maritalstatus = 'S' AND Gender = 'F';

--Get all the single male employees taken sick leaves more than 50 hrs
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE MaritalStatus = 'S' AND Gender = 'M' AND SickLeavehours > 50;

--Get all the employees born on 7th of jan 1983
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE BirthDate = '7-Jan-1983';

--get all the employees born before 1983
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE BirthDate < '1980';

SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
FROM HumanResources.Employee
WHERE YEAR(BirthDate) < '1980';

--CASE
SELECT BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, 
CASE Gender 
     WHEN 'M' THEN 'Male'
	 WHEN 'F' THEN 'Female'
	 ELSE 'Unknown'
END AS [Gender], HireDate, SickLeaveHours
FROM HumanResources.Employee;
----------------------------------------------------------------------------------------------------------
SELECT ProductNumber + '**' + Name + ' (' + CONVERT(VARCHAR(20), StandardCost) + ')' AS [Product Details]
FROM Production.Product;

SELECT CONCAT(ProductNumber, '**', Name, ' (', StandardCost, ')') AS [Products]
FROM Production.Product;
------------------------------------------------------------------------------------------------------------
--sorting
SELECT ProductID, Name, ProductNumber, Color, StandardCost FROM Production.Product

--sort by name by default ASC
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
ORDER BY Name;
--sort by name IN DESC
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID FROM Production.Product
ORDER BY Name DESC;
--------------------------------------------------------------------------------------------------------------
--Get total cost of all products
SELECT SUM(StandardCost) AS [Total Cost] FROM Production.Product;
-- count total products
SELECT COUNT(*) FROM Production.Product 
WHERE Color = 'Red';
-------------------------------------------------------------------------------------------------------------
--Get colorwise sum of standard cost for the products
SELECT COLOR, SUM(StandardCost) AS [Total Cost] FROM Production.Product
GROUP BY Color;
--Get colorwise count of product
SELECT Color, COUNT(*) AS [Color Count] FROM Production.Product
GROUP BY Color;
--------------------------------------------------------------------------------------------------------------
--get distict count of employees based on gender
--BusinessEntityID, NationalIDNumber, LoginID, JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours
SELECT Gender, COUNT(*)
FROM HumanResources.Employee
GROUP BY Gender;
--get distict count of employees based on gender and marital status
SELECT Gender, MaritalStatus, COUNT(*)
FROM HumanResources.Employee
GROUP BY Gender, MaritalStatus;
--------------------------------------------------------------------------------------------------------------
--OVER clause
--List all the products with total cost for all products
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID,
SUM(StandardCost) OVER() AS [Total cost]
FROM Production.Product;
--List all the products colorwise total cost of all the products
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID,
SUM(StandardCost) OVER(PARTITION BY Color) AS [Total cost]
FROM Production.Product;
--------------------------------------------------------------------------------------------------------------
-- Get costliest product
--Solution 1:: SubQuesry
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID
FROM Production.Product
WHERE StandardCost = (
    SELECT MAX(StandardCost) FROM Production.Product
);
--Solution 2:: limit -- Not a appropriate solution
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID
FROM Production.Product
ORDER BY StandardCost DESC
OFFSET 0 ROWS
FETCH NEXT 5 ROWS ONLY;
--------------------------------------------------------------------------------------------------------------
--Ranking Fuctions
SELECT ProductID, Name, ProductNumber, Color, StandardCost,
ROW_NUMBER() OVER(ORDER BY StandardCost DESC) AS [ROW Number],
RANK() OVER(ORDER BY StandardCost DESC) AS [RANK],
DENSE_RANK() OVER(ORDER BY StandardCost DESC) AS [DENSE RANK]
FROM Production.Product;
--
SELECT ProductID, Name, ProductNumber, Color, StandardCost,
DENSE_RANK() OVER(ORDER BY StandardCost DESC) AS [DENSE RANK]
FROM Production.Product;

--------------------------------------------------------------------------------------------------------------
--Get the costliest product and nth costliest product
SELECT * FROM (
SELECT ProductID, Name, ProductNumber, Color, StandardCost,
DENSE_RANK() OVER(ORDER BY StandardCost DESC) AS [Dense Rank]
FROM Production.Product
) AS Products
WHERE [Dense Rank] = 3;

--------------------------------------------------------------------------------------------------------------
--get the employees who have taken least number of leaves
SELECT BusinessEntityID,JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours,
DENSE_RANK()OVER(PARTITION BY Jobtitle ORDER BY SickLeaveHours) AS [Dense Rank]
FROM HumanResources.Employee;

SELECT * FROM (
SELECT BusinessEntityID,JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours,
DENSE_RANK()OVER(ORDER BY SickLeaveHours) AS [Dense Rank]
FROM HumanResources.Employee) AS Employees
WHERE [Dense Rank] = 1;
--get the employees titlewise who have taken least number of leaves
SELECT * FROM (
SELECT BusinessEntityID,JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours,
DENSE_RANK()OVER(PARTITION BY Jobtitle ORDER BY SickLeaveHours) AS [Dense Rank]
FROM HumanResources.Employee) AS Employees
WHERE [Dense Rank] = 1;
--get all the male and female employees who have taken least number of leaves
SELECT * FROM (
SELECT BusinessEntityID,JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours,
DENSE_RANK()OVER(PARTITION BY Gender ORDER BY SickLeaveHours) AS [Dense Rank]
FROM HumanResources.Employee) AS Employees
WHERE [Dense Rank] = 1;
-----------------------------------------------------------------------------------------------------------------
--NTILE
SELECT BusinessEntityID,JobTitle, BirthDate, MaritalStatus, Gender, HireDate, SickLeaveHours,
NTILE(4)OVER(ORDER BY SickLeaveHours) AS [Dense]
FROM HumanResources.Employee;
--------------------------------------------------------------------------------------------------------------------------
--/* JOIN */
SELECT ProductID, Name, ProductNumber, Color, StandardCost, ProductSubcategoryID
FROM Production.Product 
WHERE ProductSubcategoryID IS NOT NULL;

SELECT ProductSubcategoryID, Name 
FROM Production.ProductSubcategory;

-- GET THE details of products with respective to name of ProductSubcategory only if the product belongs to any subcategory	(inner join)

SELECT ProductID,PRD.Name, ProductNumber, Color, StandardCost, PSC.Name FROM
Production.Product PRD INNER JOIN Production.ProductSubcategory PSC
ON PRD.ProductSubcategoryID = PSC.ProductSubcategoryID;
--

SELECT ProductID,PRD.Name, ProductNumber, Color, StandardCost, PSC.Name,PRD.ProductSubcategoryID,PSC.ProductSubcategoryID FROM
Production.Product PRD INNER JOIN Production.ProductSubcategory PSC
ON PRD.ProductSubcategoryID = PSC.ProductSubcategoryID
WHERE PSC.rowguid = '000310C0-BCC8-42C4-B0C3-45AE611AF06B';
-- GET THE details of products with respective to name of ProductSubcategory (outer join)
SELECT ProductID,PRD.Name, ProductNumber, Color, StandardCost, PSC.Name,PRD.ProductSubcategoryID,PSC.ProductSubcategoryID FROM
Production.Product PRD LEFT OUTER JOIN Production.ProductSubcategory PSC
ON PRD.ProductSubcategoryID = PSC.ProductSubcategoryID;
----------------------------------------------------------------------------------------------------------------------------------
--Having clause
SELECT Color, Count(*)
FROM Production.Product
GROUP BY Color
ORDER BY Color;
-----------------------------------------------------------------------------------------------------------------------
SELECT BusinessEntityID, FirstName, MiddleName, LastName, ModifiedDate FROM Person.Person
WHERE ModifiedDate > '29-Dec-2000';

SELECT BusinessEntityID, FirstName, MiddleName, LastName, ModifiedDate FROM Person.Person
WHERE ModifiedDate NOT BETWEEN '1-December-2000' AND '31-December-2000';

SELECT ProductID, Name FROM Production.Product
WHERE Name LIKE 'Chain%';

SELECT BusinessEntityID, FirstName, MiddleName, LastName FROM Person.Person
WHERE MiddleName IN ('E','B'); 

SELECT SalesOrderID, OrderDate, TotalDue FROM Sales.SalesOrderHeader
WHERE OrderDate BETWEEN '1-September-2001' AND '30-September-2001' AND TotalDue = 1000;

SELECT * FROM Sales.SalesOrderDetail;

SELECT * FROM Sales.SalesOrderHeader
WHERE TotalDue > 1000 AND SalesPersonID = 279 OR TerritoryID = 6;

SELECT ProductID, Name, Color FROM Production.Product
WHERE Color <> 'Blue';

SELECT BusinessEntityID, FirstName, MiddleName, LastName FROM Person.Person
ORDER BY LastName, MiddleName, FirstName;

SELECT CONCAT(AddressLine1,' (', City, PostalCode,')') FROM Person.Address;

SELECT ProductID, Name, ISNULL(Color,'No Color') FROM Production.Product;

SELECT ProductID, CONCAT('"', Name, ': ', Color, '"') AS 'Product name with color' FROM Production.Product;

SELECT SpecialOfferID, Description, MaxQty - MinQty AS 'Difference' FROM Sales.SpecialOffer;

SELECT SpecialOfferID, Description, ISNULL(MaxQty,10) * DiscountPct AS 'Result' FROM Sales.SpecialOffer;

SELECT SUBSTRING(AddressLine1,1,10) FROM Person.Address;

SELECT SalesOrderID, OrderDate, ShipDate, DATEDIFF(D, OrderDate,ShipDate) AS [NUMBER OF DAYS] FROM Sales.SalesOrderHeader;

SELECT SalesOrderID, CONVERT(VARCHAR(10), OrderDate,120) AS [Order Date], CONVERT(VARCHAR(10), ShipDate,120) AS [Ship Date] FROM Sales.SalesOrderHeader;

SELECT SalesOrderID, OrderDate, DATEADD(M, 6, OrderDate) AS [UPDATED DATE] FROM Sales.SalesOrderHeader;

SELECT SalesOrderID, OrderDate, MONTH(OrderDate) AS [month], YEAR(OrderDate) AS [Year] FROM Sales.SalesOrderHeader;

SELECT CAST(RAND() * 10 AS INT) +1;

SELECT SalesOrderID, OrderDate FROM Sales.SalesOrderHeader
WHERE YEAR(OrderDate) = 2011;
  
SELECT SalesOrderID, OrderDate FROM Sales.SalesOrderHeader
ORDER BY MONTH(OrderDate) , YEAR(OrderDate);

--The HumanResources.Employee table does not contain the employee names. Join that table to the Person.
--Person table on the BusinessEntityID column. Display the job title, birth date, first name, and last name.  
SELECT EMP.JobTitle, EMP.BirthDate, PER.FirstName, PER.LastName 
FROM HumanResources.Employee EMP INNER JOIN Person.Person PER
ON EMP.BusinessEntityID = PER.BusinessEntityID;
--WHERE PER.FirstName IS NULL OR PER.LastName IS NULL;
--WHERE PER.MiddleName IS NULL;

SELECT BusinessEntityID, FirstName, MiddleName,LastName FROM Person.Person;

SELECT CustomerID, StoreID, TerritoryID, FirstName, MiddleName,LastName FROM
Person.Person PER INNER JOIN Sales.Customer CUS
ON PER.BusinessEntityID = CUS.PersonID;

SELECT SalesPersonID FROM Sales.SalesOrderHeader
SELECT BusinessEntityID FROM Sales.SalesPerson

SELECT SalesOrderID, SalesQuota, Bonus FROM 
Sales.SalesOrderHeader SOH INNER JOIN Sales.SalesPerson SP
ON SOH.SalesPersonID = SP.BusinessEntityID
--
SELECT Color, Size, PRDM.CatalogDescription FROM
Production.Product PRD INNER JOIN Production.ProductModel PRDM
ON PRD.ProductModelID = PRDM.ProductModelID;
--
SELECT FirstName, MiddleName, LastName, PRD.Name FROM 
Sales.Customer C
INNER JOIN Person.Person P ON P.BusinessEntityID = C.PersonID 
INNER JOIN Sales.SalesOrderHeader SOH ON C.CustomerID = SOH.CustomerID
INNER JOIN Sales.SalesOrderDetail SOD ON SOH.SalesOrderID = SOD.SalesOrderID
INNER JOIN Production.Product PRD ON PRD.ProductID = SOD.ProductID;
--
SELECT SalesOrderID, P.ProductID, P.Name FROM Sales.SalesOrderDetail SOD RIGHT OUTER JOIN Production.Product P
ON SOD.ProductID = P.ProductID;
--
--The Sales.SalesOrderHeader table contains foreign keys to the Sales.CurrencyRate and Purchasing.ShipMethod tables. 
--Write a query joining all three tables, making sure it contains all rows from Sales.SalesOrderHeader. 
--Include the CurrencyRateID, AverageRate, SalesOrderID, and ShipBase columns.  

SELECT * FROM Sales.SalesOrderHeader;

SELECT CR.CurrencyRateID, CR.AverageRate, SOH.SalesOrderID, SM.ShipBase FROM 
Sales.SalesOrderHeader SOH 
LEFT OUTER JOIN Sales.CurrencyRate CR ON SOH.CurrencyRateID = CR.CurrencyRateID
LEFT OUTER JOIN Purchasing.ShipMethod SM ON SOH.ShipMethodID = SM.ShipMethodID;
--
SELECT COUNT(*) AS [Number Of Customers] FROM Sales.Customer;
--
SELECT Min(ListPrice) AS [Minimum Price], MAX(ListPrice) AS [Maximum Price], AVG(ListPrice) AS [Average Price] FROM Production.Product;
--
SELECT ProductID, SUM(OrderQty) AS [Total Number of items for each product] FROM Sales.SalesOrderDetail
GROUP BY ProductID;
--Write a query using the Sales.SalesOrderDetail table that displays a count of the detail lines for each SalesOrderID. 
SELECT SalesOrderID, COUNT(*) AS [Count] FROM Sales.SalesOrderDetail
GROUP BY SalesOrderID;
--Write a query using the Production.Product table that lists a count of the products in each product line. 
SELECT COUNT(*) AS [Count of products], ProductLine FROM Production.Product
GROUP BY ProductLine;
--Write a query that displays the count of orders placed by year for each customer using the Sales.SalesOrderHeader table.  
SELECT YEAR(OrderDate) AS [Year], CustomerID, COUNT(*) AS [Order Placed Per Year By Customer] FROM Sales.SalesOrderHeader
GROUP BY YEAR(OrderDate), CustomerID;
--Write a query that creates a sum of the LineTotal in the Sales.SalesOrderDetail table grouped by the SalesOrderID. 
--Include only those rows where the sum exceeds 1,000.  
SELECT SalesOrderID, SUM(LineTotal) AS [Total Line] FROM Sales.SalesOrderDetail
GROUP BY SalesOrderID
HAVING SUM(LineTotal) > 1000;
--Write a query that groups the products by ProductModelID along with a count. 
--Display the rows that have a count that equals 1. 
SELECT ProductModelID, COUNT(*) AS [Group Of Product Per Model] FROM Production.ProductModel
GROUP BY ProductModelID
HAVING COUNT(*) = 1;
--Write a query using the Sales.SalesOrderHeader, Sales.SalesOrderDetail, and Production.
--Product tables to display the total sum of products by ProductID and OrderDate.  
SELECT * FROM Sales.SalesOrderDetail
SELECT * FROM Sales.SalesOrderHeader
SELECT * FROM Production.Product

SELECT P.ProductID, OrderDate, SUM(SOD.ProductID) AS [Total Product] FROM Sales.SalesOrderHeader SOH
INNER JOIN Sales.SalesOrderDetail SOD ON SOH.SalesOrderID = SOD.SalesOrderID
INNER JOIN Production.Product P ON P.ProductID = SOD.ProductID
GROUP BY P.ProductID, OrderDate;
--Display the 3rd joined employee. 
SELECT BusinessEntityID, Gender, BirthDate, HireDate FROM HumanResources.Employee
ORDER BY HireDate;

SELECT BusinessEntityID, Gender, BirthDate, HireDate FROM (
SELECT  BusinessEntityID, Gender, BirthDate, HireDate ,ROW_NUMBER()OVER(ORDER BY HireDate) AS [ROW NUMBER]
FROM HumanResources.Employee) AS [Employee]
WHERE [ROW NUMBER] = 3;

--Display the customer who has placed 2nd highest orders
SELECT * FROM Sales.Customer;
SELECT * FROM Sales.SalesOrderHeader;
SELECT * FROM Sales.SalesOrderDetail;

SELECT SalesOrderID, OrderQty, CustomerID,[Rank] FROM (
SELECT SOH.SalesOrderID AS [SalesOrderID], OrderQty, C.CustomerID AS [CustomerID], ROW_NUMBER()OVER(ORDER BY OrderQty DESC) AS [Rank] FROM 
Sales.Customer C INNER JOIN Sales.SalesOrderHeader SOH ON C.CustomerID = SOH.CustomerID 
INNER JOIN Sales.SalesOrderDetail SOD ON SOH.SalesOrderID = SOD.SalesOrderID) AS [Cutomer]
WHERE [Rank] = 2;

--------------------
--Get all the products that belong to subcategory named Bikes
SELECT ProductSubcategoryID, Name FROM Production.ProductSubcategory WHERE Name = 'Road Bikes';
SELECT * FROM Production.ProductCategory;
SELECT * FROM Production.Product WHERE ProductSubcategoryID = 
(SELECT ProductSubcategoryID FROM Production.ProductSubcategory WHERE Name = 'Road Bikes');
--MULTI ROW SUB QUERY
--Get all the products in Category named 'Bikes'
SELECT * FROM Production.ProductSubcategory 
SELECT * FROM Production.ProductCategory;
SELECT * FROM Production.Product;
SELECT * FROM Production.Product WHERE ProductSubcategoryID IN
(
  SELECT ProductSubcategoryID FROM Production.ProductSubcategory WHERE ProductCategoryID = 
   (SELECT ProductCategoryID FROM Production.ProductCategory WHERE Name = 'Bikes')
);
--MULTI COLUMN SUB QUERY
CREATE TABLE #BlackProducts
(
    ProdID INT,
	ProdName VARCHAR(50),
	Color VARCHAR(20)
);

INSERT INTO #BlackProducts
SELECT ProductID, Name, Color FROM Production.Product WHERE Color = 'Black';

SELECT * FROM #BlackProducts;
--CORELATED SUB QUERY
-- Get all the non null colored products which have standard cost greater than the half of sum of standard cost against's its respective color
SELECT ProductID, Name, Color, StandardCost FROM Production.Product P
WHERE Color IS NOT NULL AND StandardCost > 
(
   SELECT SUM(StandardCost)/2 FROM Production.Product WHERE Color = P.Color
);

SELECT ProductID, Name, Color, StandardCost FROM Production.Product P
WHERE Color IS NOT NULL AND StandardCost <
(
  SELECT SUM(StandardCost)/2 FROM Production.Product WHERE Color = P.Color
);
--VIEW
SELECT * INTO MyProducts FROM Production.Product WHERE Color IS NOT NULL

SELECT * FROM MyProducts;

CREATE VIEW V_MyProducts AS 
SELECT * FROM MyProducts WHERE ProductID BETWEEN 700 AND 800;

SELECT * FROM V_MyProducts;

-----PL/SQL-----
/*
PL/SQL
Procedures
UDFs
Triggers
*/
--Anonymous Blocks
--Declare variables and assigning values

DECLARE @empId INT;
DECLARE @empName VARCHAR(20);
DECLARE @empNameFromTable VARCHAR(50);
SET @empId = 5;
SET @empName = 'Tanmay';
SELECT @empNameFromTable = CONCAT(FirstName, ' ', LastName) FROM Person.Person WHERE BusinessEntityID = @empId;
PRINT CONCAT('EMP ID: ',@empId);
PRINT 'User Assigned EMP Name: ' + @empName;
PRINT 'EMP Name FROM TABLE: ' + @empNameFromTable;

SELECT * FROM Person.Person WHERE BusinessEntityID=1

--IF ELSE
DECLARE @Num INT;
SET @Num = 12;
IF (@Num % 2) = 0
    BEGIN 
	   PRINT 'EVEN NUMBER';
	END
ELSE 
    BEGIN
	   PRINT 'ODD NUMBER';
	END

----IF ELSE IF
DECLARE @Num INT;
SET @Num = 13;
IF (@Num < 0)
  BEGIN 
     PRINT 'NEGATIVE NUMBER';
  END
ELSE IF (@Num = 0)
  BEGIN
     PRINT 'ZERO';
  END
ELSE
  BEGIN
     PRINT 'POSITIVE NUMBER';
  END
----------------------
-------LOOP
--WHILE
DECLARE @Num INT;
SET @Num = 1;
WHILE (@Num <= 10)
   BEGIN
     PRINT @Num;
	 SET @Num = @Num + 1;
   END
-------------------------
--STORED PROCEDURE--
-- SP WITH INPUT--

CREATE TABLE #Employee
(
  EmpID INT PRIMARY KEY IDENTITY(1,1),
  EmpName VARCHAR(20),
  Gender VARCHAR(1)
);

CREATE TABLE #EmployeeSalary
(
  EmpID INT REFERENCES #Employee(EmpID),
  BasicSalary MONEY,
  HRA MONEY,
  DA MONEY
);

SELECT * FROM #Employee;

SELECT * FROM #EmployeeSalary;

CREATE PROCEDURE SP_CreateNewEmployee
(
  @pEmpName VARCHAR(20),
  @pGender VARCHAR(1),
  @pBasicSalary MONEY,
  @pHRA MONEY,
  @pDA MONEY
)
AS
BEGIN
      DECLARE @pLatestEmpID INT;
      INSERT INTO #Employee (EmpName, Gender) VALUES (@pEmpName, @pGender);
	  SELECT @pLatestEmpID = MAX(EmpID) FROM #Employee;
	  INSERT INTO #EmployeeSalary (EmpID, BasicSalary, HRA, DA) VALUES (pLatestEmpID, @pBasicSalary, @pHRA, @pDA);
END;

EXECUTE SP_CreateNewEmployee 'Tanmay Patil', 'M', 89999.899, 6799.99, 789.99;

--Create a SP to register new employee only if the data is valid. e.g. Gender must be either 'M','F','O','U' AND Salary > 0;

CREATE PROCEDURE SP_CreateNewEmployee
(
  @pEmpName VARCHAR(20),
  @pGender VARCHAR(1),
  @pBasicSalary MONEY,
  @pHRA MONEY,
  @pDA MONEY
)
AS
BEGIN
     IF @pGender IN ('M','F','O','U') AND @pBasicSalary>0 AND @pHRA > 0 AND @pDA > 0
	   BEGIN
	         DECLARE @pLatestEmpID INT;
             INSERT INTO #Employee (EmpName, Gender) VALUES (@pEmpName, @pGender);
	         SELECT @pLatestEmpID = MAX(EmpID) FROM #Employee;
	         INSERT INTO #EmployeeSalary (EmpID, BasicSalary, HRA, DA) VALUES (@pBasicSalary, @pHRA, @pDA);
		     PRINT 'RECORD SAVED';
	   END;
     ELSE
	   BEGIN
	         PRINT 'ENTER VALID INPUT';
	   END;
END;

EXECUTE SP_CreateNewEmployee 'Tanmay Patil', 'M', 89999.899, 6799.99, 789.99;

--------------------------------------
CREATE TABLE #Employee
(
  EmpID INT PRIMARY KEY IDENTITY(1,1),
  EmpName VARCHAR(20),
  Gender VARCHAR(1)
);

CREATE TABLE #EmployeeSalary
(
  EmpID INT REFERENCES #Employee(EmpID),
  BasicSalary MONEY,
  HRA MONEY,
  DA MONEY
);

DROP TABLE #Employee
DROP TABLE #EmployeeSalary

SELECT * FROM #Employee;
SELECT * FROM #EmployeeSalary;

--
DROP PROCEDURE SP_CreateNewEmployee
CREATE PROCEDURE SP_CreateNewEmployee
(
  @pEmpName VARCHAR(20),
  @pGender VARCHAR(1),
  @pBasicSalary MONEY,
  @pHRA MONEY,
  @pDA MONEY
)
AS
BEGIN
      DECLARE @pLatestEmpID INT;
      INSERT INTO #Employee (EmpName, Gender) VALUES (@pEmpName, @pGender);
	  SELECT @pLatestEmpID = MAX(EmpID) FROM #Employee;
	  INSERT INTO #EmployeeSalary (EmpID, BasicSalary, HRA, DA) VALUES (@pLatestEmpID, @pBasicSalary, @pHRA, @pDA);
END;

EXECUTE SP_CreateNewEmployee 'Tanmay Patil', 'M', 89999.99, 6799.9, 789.9;


--EXECUTE SP_CreateNewEmployee 'Tanmay Patil', 'M', 89999.899, 6799.99, 789.99;
EXECUTE SP_CreateNewEmployee 'Jyoti Patil', 'F', 69999.899, 5799.99, 489.99;

--Display employee name and salary by empID

DROP PROCEDURE SP_GetEmployeeById
CREATE PROCEDURE SP_GetEmployeeById (@pEmpID INT) AS
BEGIN 
      SELECT emp.EmpID, emp.EmpName, sal.BasicSalary+sal.HRA+sal.DA AS [Total Salary] 
	  FROM #Employee emp JOIN #EmployeeSalary sal ON emp.EmpID = sal.EmpID
	  WHERE emp.EmpID = @pEmpID
END;

EXECUTE SP_GetEmployeeById 2;
--
-----SP WITH OUTPUT PARAMS

--Return employee name and salary by empID using OUTPUT PARAMS of procedure
DROP PROCEDURE SP_GetEmployeeDetails
CREATE PROCEDURE SP_GetEmployeeDetails
(
   @pEmpID INT ,
   @poEmpName VARCHAR(20) OUTPUT,
   @poSalary MONEY OUTPUT
) AS
BEGIN
      SELECT @poEmpName = emp.EmpName, @poSalary = (sal.BasicSalary + sal.HRA + sal.DA)
	  FROM #Employee emp JOIN #EmployeeSalary sal ON emp.EmpID = sal.EmpID
	  WHERE emp.EmpID = @pEmpID      
END;

DECLARE @resEmpName VARCHAR(20);
DECLARE @resTotalSalary MONEY;
EXECUTE SP_GetEmployeeDetails 2, @resEmpName OUTPUT, @resTotalSalary OUTPUT;
PRINT @resEmpName;
PRINT @resTotalSalary;
--------------------------------------------------------------------------------------------
SELECT (YEAR(GETDATE())- YEAR(BirthDate)) AS [AGE] FROM HumanResources.Employee WHERE BusinessEntityID = 1;

SELECT DATEDIFF(YYYY, BirthDate, GETDATE()) AS [AGE] FROM HumanResources.Employee WHERE BusinessEntityID = 1;
------------------------------------------------------------------------------------------------------------
----------------USER DEFINED FUNCTIONS
--SCALAR FUNCTION
ALTER FUNCTION udf_ValidateAge
( @pDate DATE)
RETURNS INT
AS
BEGIN
      DECLARE @res INT;
      IF DATEDIFF(YYYY, @pDate, GETDATE()) BETWEEN 10 AND 50
	     SET @res = 0;
	  ELSE 
	     SET @res = 1;

	     RETURN DATEDIFF(YYYY, @pDate, GETDATE());
END;

PRINT dbo.udf_ValidateAge ('24-Dec-2000');

CREATE TABLE #Customer
(
    CustomerID INT PRIMARY KEY IDENTITY(1,1),
	CustomerName VARCHAR(20),
	BirthDate DATE,
	Age INT
);

DECLARE @pCustomerName VARCHAR(20);
DECLARE @pBirthDate DATE;
DECLARE @pAge INT;
SET @pCustomerName = 'Tanmay Patil';
SET  @pBirthDate = '14-Jul-1997';
SELECT @pAge = dbo.udf_ValidateAge(@pBirthDate);
INSERT INTO #Customer(CustomerName, BirthDate, Age) VALUES (@pCustomerName, @pBirthDate, @pAge);

SELECT * FROM #Customer;

------------------------------------------------------------
--------Table Valued Funcion
--1. Inline Function
CREATE FUNCTION dbo.ColorBasedProducts
(@pColor VARCHAR(20))
RETURNS TABLE
AS
RETURN SELECT * FROM Production.Product WHERE Color = @pColor;

SELECT * FROM dbo.ColorBasedProducts ('Red');
----	  
--------------------------------------------------------
---2.Multi Statement Function

CREATE FUNCTION dbo.ColorBaseProductsVer2
(
    @pColor VARCHAR(20)
)
RETURNS @prods TABLE
(
   ProdID INT,
   ProdName VARCHAR(50),
   Color VARCHAR(20),
   StandardCost MONEY
)
AS
BEGIN
      INSERT INTO @prods
      SELECT ProductID, Name, Color, StandardCost FROM Production.Product WHERE Color = @pColor;
	  RETURN;
END;

SELECT * FROM dbo.ColorBaseProductsVer2 ('Black');
------------------------------------------------------------------------------------------------------------
----Create a Function that takes CategoryName as a parameter and gets the products associated with that category.
SELECT * FROM Production.Product;
SELECT * FROM Production.ProductSubcategory;
SELECT * FROM Production.ProductCategory;

SELECT P.ProductID, P.Name AS [Product Name], PSC.Name AS [Product Sub Category Name], PC.Name AS [Product Category Name], P.ProductNumber, P.Color, P.StandardCost, P.Size  FROM 
Production.Product P JOIN Production.ProductSubcategory PSC ON P.ProductSubcategoryID = PSC.ProductSubcategoryID
JOIN Production.ProductCategory PC ON PC.ProductCategoryID = PSC.ProductCategoryID
WHERE PC.ProductCategoryID=3;

CREATE FUNCTION dbo.GetProductCategories 
(@pCategoryId INT)
RETURNS @tablu TABLE
(
    ProdId INT,
	ProdName VARCHAR(50),
	ProdSCName VARCHAR(50),
	ProdCName VARCHAR(50),
	ProdNumber VARCHAR(50),
	Color VARCHAR(20),
	ProdCost MONEY,
	ProdSize VARCHAR(10)
)AS
BEGIN
      INSERT INTO @tablu
	  SELECT P.ProductID, P.Name AS [Product Name], PSC.Name AS [Product Sub Category Name], PC.Name AS [Product Category Name], P.ProductNumber, P.Color, P.StandardCost, P.Size  FROM 
      Production.Product P JOIN Production.ProductSubcategory PSC ON P.ProductSubcategoryID = PSC.ProductSubcategoryID
      JOIN Production.ProductCategory PC ON PC.ProductCategoryID = PSC.ProductCategoryID
	  WHERE PC.ProductCategoryID=@pCategoryId;
	  RETURN;
END;

SELECT * FROM dbo.GetProductCategories (2);

---------------------------------------------------------------------------------------------------------------
--Indexes
SET STATISTICS IO ON
SELECT * FROM MyProducts WHERE ProductID = 991;
SET STATISTICS IO OFF

CREATE CLUSTERED INDEX idx_MyProds_ProdId ON MyProducts (ProductID);

------------------------------------------------------------------------
-----TRIGGER:::
--Create trigger to backup the deleted records from MyProducts

SELECT * FROM MyProducts;
--
--DROP TRIGGER tr_del_myprods;

CREATE TRIGGER tr_del_myprods
ON MyProducts
AFTER DELETE
AS
BEGIN
    PRINT 'Trigger Executed';
END;
--
DELETE FROM MyProducts WHERE ProductID = 998;
drop table #MyProductsBackup
--
SELECT * INTO #MyProductsBackup  FROM MyProducts WHERE 1=2;


--
SELECT * FROM #MyProductsBackup;
--
CREATE TRIGGER tr_del_myprods
ON MyProducts
AFTER DELETE
AS
BEGIN
    INSERT INTO #MyProductsBackup
	SELECT * FROM deleted;
END;

-----

SELECT * INTO Sampleproducts FROM MyProducts;

SELECT * FROM Sampleproducts;

CREATE TRIGGER tr_backupproducts ON Sampleproducts
INSTEAD OF DELETE
AS
BEGIN
     PRINT 'HaHaHaHa.....!'
END;

DELETE FROM SampleProducts WHERE ProductID = 997;
------------------------------------------------------
--DDL TRIGGERES

CREATE TRIGGER tr_PrevetDropSP ON DATABASE
FOR DROP_PROCEDURE
AS
   PRINT 'Dropping procedure is not allowed DDL triger preventing this from happening. To drop stored procedure diable the trigger first..!';
ROLLBACK;
GO
--------------------------------------------------------
CREATE TABLE BankAccounts.BankAccounts(
	AccountID  INT PRIMARY KEY IDENTITY(1,1),
	CustomerName VARCHAR(100) NOT NULL,
	AccountType  VARCHAR(10) NOT NULL CHECK (AccountType IN('Current ','Saving')),
	BALANCE MONEY CHECK (BALANCE>0),
	LastModified DATE DEFAULT GETDATE()
);


INSERT INTO BankAccounts.BankAccounts (CustomerName,AccountType,BALANCE) VALUES
('Tanmay', 'Saving', 6000),
('Vishal', 'Saving', 12780),
('Manish', 'Saving', 4300),
('Tejas', 'Saving', 9870),
('Pranav', 'Current', 140990),
('Kalpesh', 'Current', 160005),
('Anicket', 'Current', 1400000),
('Navoday', 'Current', 350000);

CREATE TABLE BankAccounts.BankTransactions(
	TransactionID INT PRIMARY KEY IDENTITY(1,1),
	AccountID INT REFERENCES BankAccounts.BankAccounts(AccountID) NOT NULL,
	TransactionDate DATE CHECK (TransactionDate<=GETDATE()),
	TransactionType VARCHAR(10) NOT NULL CHECK (TransactionType IN('Debit ','Credit')),
	TransactionAmount MONEY CHECK (TransactionAmount>0)
)


CREATE TRIGGER update_bankAccount
ON BankAccounts.BankTransactions
AFTER INSERT	
AS
BEGIN
	IF(SELECT TransactionType FROM inserted)='Credit'
		UPDATE BankAccounts.BankAccounts SET BALANCE=BALANCE+(SELECT TransactionAmount FROM inserted) WHERE AccountID=(SELECT AccountID FROM inserted)
	ELSE
		UPDATE BankAccounts.BankAccounts SET BALANCE=BALANCE-(SELECT TransactionAmount FROM inserted) WHERE AccountID=(SELECT AccountID FROM inserted)
	Print 'Balance updated in account table for the customer!'
END

INSERT INTO BankAccounts.BankTransactions (AccountID,TransactionDate,TransactionType,TransactionAmount) 
VALUES (1,'02-APR-2022','Debit',1000);

INSERT INTO BankAccounts.BankTransactions (AccountID,TransactionDate,TransactionType,TransactionAmount) 
VALUES (2,'02-APR-2022','Credit',1000);

SELECT * FROM BankAccounts.BankAccounts;

SELECT * FROM BankAccounts.BankTransactions;
