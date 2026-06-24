# SQL Complete Query Reference

## SQL SELECT

```sql
SELECT * FROM Customers;
SELECT Name, City FROM Customers;
```

## SQL SELECT DISTINCT

```sql
SELECT DISTINCT City FROM Customers;
```

## SQL WHERE

```sql
SELECT * FROM Customers
WHERE City = 'Delhi';
```

## SQL ORDER BY

```sql
SELECT * FROM Customers
ORDER BY Name ASC;

SELECT * FROM Customers
ORDER BY Name DESC;
```

## SQL AND

```sql
SELECT * FROM Customers
WHERE City='Delhi' AND Country='India';
```

## SQL OR

```sql
SELECT * FROM Customers
WHERE City='Delhi' OR City='Mumbai';
```

## SQL NOT

```sql
SELECT * FROM Customers
WHERE NOT City='Delhi';
```

## SQL INSERT INTO

```sql
INSERT INTO Customers(Name, City)
VALUES('Kishan', 'Ajmer');
```

## SQL NULL VALUES

```sql
SELECT * FROM Customers
WHERE Email IS NULL;

SELECT * FROM Customers
WHERE Email IS NOT NULL;
```

## SQL UPDATE

```sql
UPDATE Customers
SET City='Jaipur'
WHERE CustomerID=1;
```

## SQL DELETE

```sql
DELETE FROM Customers
WHERE CustomerID=1;
```

## SQL SELECT TOP

```sql
SELECT TOP 5 * FROM Customers;

-- MySQL
SELECT * FROM Customers LIMIT 5;
```

## SQL AGGREGATE FUNCTIONS

### MIN()

```sql
SELECT MIN(Salary) FROM Employees;
```

### MAX()

```sql
SELECT MAX(Salary) FROM Employees;
```

### COUNT()

```sql
SELECT COUNT(*) FROM Employees;
```

### SUM()

```sql
SELECT SUM(Salary) FROM Employees;
```

### AVG()

```sql
SELECT AVG(Salary) FROM Employees;
```

## SQL LIKE

```sql
SELECT * FROM Customers
WHERE Name LIKE 'K%';
```

## SQL WILDCARDS

```sql
SELECT * FROM Customers
WHERE Name LIKE '%an%';
```

## SQL IN

```sql
SELECT * FROM Customers
WHERE City IN ('Delhi','Jaipur','Mumbai');
```

## SQL BETWEEN

```sql
SELECT * FROM Products
WHERE Price BETWEEN 1000 AND 5000;
```

## SQL ALIASES

```sql
SELECT Name AS CustomerName
FROM Customers;
```

## SQL INNER JOIN

```sql
SELECT Orders.OrderID, Customers.Name
FROM Orders
INNER JOIN Customers
ON Orders.CustomerID = Customers.CustomerID;
```

## SQL LEFT JOIN

```sql
SELECT Customers.Name, Orders.OrderID
FROM Customers
LEFT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;
```

## SQL RIGHT JOIN

```sql
SELECT Customers.Name, Orders.OrderID
FROM Customers
RIGHT JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;
```

## SQL SELF JOIN

```sql
SELECT A.Name, B.Name
FROM Employees A
INNER JOIN Employees B
ON A.ManagerID = B.EmployeeID;
```

## SQL UNION

```sql
SELECT City FROM Customers
UNION
SELECT City FROM Suppliers;
```

## SQL UNION ALL

```sql
SELECT City FROM Customers
UNION ALL
SELECT City FROM Suppliers;
```

## SQL GROUP BY

```sql
SELECT City, COUNT(*)
FROM Customers
GROUP BY City;
```

## SQL HAVING

```sql
SELECT City, COUNT(*)
FROM Customers
GROUP BY City
HAVING COUNT(*) > 5;
```

## SQL EXISTS

```sql
SELECT Name
FROM Customers
WHERE EXISTS (
  SELECT OrderID
  FROM Orders
  WHERE Orders.CustomerID = Customers.CustomerID
);
```

## SQL ANY

```sql
SELECT ProductName
FROM Products
WHERE Price > ANY(
  SELECT Price FROM Products
  WHERE CategoryID = 1
);
```

## SQL ALL

```sql
SELECT ProductName
FROM Products
WHERE Price > ALL(
  SELECT Price FROM Products
  WHERE CategoryID = 1
);
```

## SQL SELECT INTO

```sql
SELECT *
INTO CustomersBackup
FROM Customers;
```

## SQL INSERT INTO SELECT

```sql
INSERT INTO CustomersBackup
SELECT * FROM Customers;
```

## SQL CASE

```sql
SELECT Name,
CASE
  WHEN Salary > 50000 THEN 'High'
  WHEN Salary > 25000 THEN 'Medium'
  ELSE 'Low'
END AS SalaryGrade
FROM Employees;
```

## SQL COALESCE / ISNULL

```sql
SELECT COALESCE(Phone,'N/A')
FROM Customers;
```

## STORED PROCEDURE

```sql
CREATE PROCEDURE GetCustomers
AS
SELECT * FROM Customers;
GO

EXEC GetCustomers;
```

## SQL COMMENTS

```sql
-- Single line comment

/*
Multi line
comment
*/
```

## SQL OPERATORS

```sql
SELECT * FROM Products
WHERE Price > 1000;

SELECT * FROM Products
WHERE Quantity <= 50;
```

# DATABASE COMMANDS

## CREATE DATABASE

```sql
CREATE DATABASE CompanyDB;
```

## DROP DATABASE

```sql
DROP DATABASE CompanyDB;
```

## BACKUP DATABASE

```sql
BACKUP DATABASE CompanyDB
TO DISK = 'D:\Backup\CompanyDB.bak';
```

## CREATE TABLE

```sql
CREATE TABLE Customers(
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    City VARCHAR(50)
);
```

## DROP TABLE

```sql
DROP TABLE Customers;
```

## ALTER TABLE

```sql
ALTER TABLE Customers
ADD Phone VARCHAR(20);

ALTER TABLE Customers
DROP COLUMN Phone;
```

## NOT NULL

```sql
CREATE TABLE Employees(
    EmployeeID INT,
    Name VARCHAR(100) NOT NULL
);
```

## UNIQUE

```sql
CREATE TABLE Users(
    UserID INT,
    Email VARCHAR(100) UNIQUE
);
```

## PRIMARY KEY

```sql
CREATE TABLE Students(
    StudentID INT PRIMARY KEY
);
```

## FOREIGN KEY

```sql
CREATE TABLE Orders(
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    FOREIGN KEY(CustomerID)
    REFERENCES Customers(CustomerID)
);
```

## CHECK

```sql
CREATE TABLE Employees(
    Age INT CHECK(Age >= 18)
);
```

## DEFAULT

```sql
CREATE TABLE Orders(
    Status VARCHAR(20) DEFAULT 'Pending'
);
```

## CREATE INDEX

```sql
CREATE INDEX idx_customer_name
ON Customers(Name);
```

## AUTO INCREMENT

### MySQL

```sql
CREATE TABLE Users(
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100)
);
```

### SQL Server

```sql
CREATE TABLE Users(
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(100)
);
```

## DATES

```sql
SELECT GETDATE();

SELECT *
FROM Orders
WHERE OrderDate >= '2026-01-01';
```

## VIEWS

```sql
CREATE VIEW CustomerView AS
SELECT Name, City
FROM Customers;

SELECT * FROM CustomerView;
```

## SQL INJECTION (Safe Query)

```sql
SELECT *
FROM Users
WHERE Username = @Username
AND Password = @Password;
```

## PARAMETERS

```sql
DECLARE @City VARCHAR(50);
SET @City='Jaipur';

SELECT *
FROM Customers
WHERE City=@City;
```

## PREPARED STATEMENTS

### MySQL

```sql
PREPARE stmt FROM
'SELECT * FROM Customers WHERE City=?';

SET @city='Jaipur';

EXECUTE stmt USING @city;
```
