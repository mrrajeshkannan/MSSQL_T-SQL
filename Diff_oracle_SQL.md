
# --Data Base Name / Server Name:-
```
SELECT @@SERVERNAME;
```

## --VERSION:-
```
SELECT @@VERSION;
```

## --connecting adventureworks transaction table to sql server

SETP 1: download and Copy and paste the adventureworks bk to backup folder.

Step 2: Then database->restore database->map the file-> ok and apply


If you want to use database and with in the table 
USE [AdventureWorks2016];



## ---Table description 
```
SELECT * 
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME ='EMPLOYEES';

```



##  --Alter table column :-
```
EXEC sp_rename 'TableName.OldColumnName', 'NewColumnName', 'COLUMN';
```


## --Concodinatation :-
```
SELECT FIRSTNAME, LASTNAME, (FIRSTNAME + ' '+ LASTNAME),
CONCAT(FIRSTNAME, ' ', MIDDLENAME, ' ', LASTNAME)
FROM Person.Person;
```

## -- To find exact charater in the cell:-
```
SELECT FIRSTNAME, LASTNAME, FIRSTNAME + ' '+ LASTNAME
FROM Person.Person
where firstname like 'A___';
```

## -- INSTR / EXTRACT VALUE FROM STRING:-
```
SELECT FIRSTNAME, LEFT(FIRSTNAME,3), RIGHT(FIRSTNAME,3)
FROM Person.Person;
```

## -- SUBSTR / EXTRACT VALUE FROM STRING:-
```
SELECT FIRSTNAME, SUBSTRING(FIRSTNAME,3,4), SUBSTRING(FIRSTNAME,1,4)
FROM Person.Person;
```



## -- Date Function :-
```
SELECT CURRENT_TIMESTAMP;
```

-- Extract Day / Month / Year from date field:-
```
SELECT OrderDate, DueDate,
Day(OrderDate) as day,
Month(OrderDate) as Month,
year(OrderDate) as Year,
CAST(OrderDate AS DATE),  --Removes time portion
EOMONTH(OrderDate) end_of_month, 
DATEADD(MONTH, 2, OrderDate),  -- Add/subtract months
DATEDIFF(MONTH, OrderDate, DueDate), -- mONTH DIFFERENCE
DATEDIFF(DAY, OrderDate, DueDate),	-- DAY DIFFERENCE
FORMAT(OrderDate, 'MM-yyyy'),
CONVERT(DATE, '2024-06-01', 120)	--Convert string to date

FROM SALES.SALESORDERHEADER;
```


## -- Union, Union all Except concept:- 
```
SELECT TOP 10 BUSINESSENTITYID FROM HUMANRESOURCES.EMPLOYEE
EXCEPT		-- LIKE MINUS IN ORACLE
SELECT top 10 BUSINESSENTITYID FROM PERSON.PERSON
```





## 🧱 Types of Indexes in MS SQL Server

| **Index Type**        | **Description**                                                                 |
|-----------------------|----------------------------------------------------------------------------------|
| **Clustered Index**   | Physically sorts the table rows. Only one per table.                            |
| **Non-Clustered Index** | Separate structure pointing to table rows. You can have many of these.         |
| **Unique Index**      | Ensures values in indexed column(s) are unique.                                 |
| **Composite Index**   | Index on multiple columns (e.g., `FirstName`, `LastName`).                      |
| **Filtered Index**    | Index on subset of rows using a `WHERE` clause.                                 |
| **Full-Text Index**   | Supports complex text searches (`CONTAINS`, `FREETEXT`).                        |



## 🔍 How to Check Current Statistics:-
```
-- View statistics details for a table
EXEC sp_helpstats 'TableName';

-- View stats info (when last updated, etc.)
DBCC SHOW_STATISTICS ('TableName', 'StatisticName');
```


## Update all stats in the table
🛠️ Syntax
```
-- Update all stats in the table
UPDATE STATISTICS TableName;

-- Update stats on a specific index or column stat
UPDATE STATISTICS TableName IndexOrStatisticName;

-- With FULLSCAN (reads all rows)
UPDATE STATISTICS TableName WITH FULLSCAN;

-- With sample size
UPDATE STATISTICS TableName WITH SAMPLE 50 PERCENT;
```











##  ---- PROCEDURE CREATATION in MSSQL:-
```
IF OBJECT_ID('EMP_DEPT', 'P') IS NOT NULL
    DROP PROCEDURE EMP_DEPT;
GO

CREATE PROCEDURE EMP_DEPT @DEPT_ID INT
AS
BEGIN
    SELECT * 
    FROM EMPLOYEES
    WHERE DEPARTMENT_ID = @DEPT_ID;
END;
GO

EXEC EMP_DEPT @DEPT_ID = 60;
```










