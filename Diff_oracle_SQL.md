
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

## --Write a query to get records in XML format:-
```
SELECT * 
FROM EMP
 FOR XML AUTO;
```

## -- Date Function :-
```
SELECT CURRENT_TIMESTAMP;
SELECT GETDATE();
```

## -- Extract Day / Month / Year from date field:-
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
## --  Select Random Rows from a Table
```
SELECT TOP 1 *
FROM Employees
ORDER BY NEWID();


-- Random integer between 1 and 100
SELECT FLOOR(RAND() * 100) + 1 AS RandomInt;


```

## -- Union, Union all Except concept:- 
```
SELECT TOP 10 BUSINESSENTITYID FROM HUMANRESOURCES.EMPLOYEE
EXCEPT		-- LIKE MINUS IN ORACLE
SELECT top 10 BUSINESSENTITYID FROM PERSON.PERSON
```

## Truncate concupt in MSQL:-
In T-SQL, TRUNCATE TABLE does release memory at the **page level**, but the ****buffer memory may not be cleared immediately**. **


SQL Server will reuse the freed pages as needed, which differs slightly from Oracle’s explicit memory cleanup.

## 🔍 Comparison: `TRUNCATE TABLE` in SQL Server vs Oracle

| Feature                       | SQL Server `TRUNCATE`                         | Oracle `TRUNCATE`                           |
|------------------------------|-----------------------------------------------|---------------------------------------------|
| **Deallocates Data Pages**   | ✅ Yes                                         | ✅ Yes                                       |
| **Minimal Logging**          | ✅ Yes (deallocation only, not row-level)      | ✅ Yes (also minimally logged)              |
| **Transactional (Can Rollback)** | ✅ Yes (if used inside `BEGIN TRAN`)     | ❌ No (auto-commits; DDL is not transactional) |
| **Frees Memory Immediately** | ⚠️ Not guaranteed (buffer pages remain until needed) | ✅ Usually clears memory (SGA)        |
| **Identity Column Reset**    | ✅ Yes (resets identity seed by default)       | ❌ Not applicable (handled differently)      |
| **Triggers Fired**           | ❌ No                                          | ❌ No                                        |
| **Faster Than DELETE**       | ✅ Yes (especially for large datasets)         | ✅ Yes                                       |
| **Can Be Rolled Back?**      | ✅ Yes (if in a transaction)                   | ❌ No                                        |

> ⚠️ In SQL Server, memory pages may remain in the buffer pool until reclaimed by other operations. Use `DBCC DROPCLEANBUFFERS` for manual memory clearing (for testing only).




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

## Check fragmentation in table :-
```
SELECT 
    OBJECT_NAME(ips.OBJECT_ID) AS TableName,
    i.name AS IndexName,
    i.type_desc AS IndexType,
    ips.index_level,
    ips.avg_fragmentation_in_percent,
    ips.page_count
FROM sys.dm_db_index_physical_stats (
    DB_ID(),                     -- Current DB
    OBJECT_ID('EMPLOYEES'),     -- Table name
    NULL,                       -- All indexes
    NULL, 
    'LIMITED'                   -- Fast mode
) AS ips
JOIN sys.indexes i 
    ON ips.object_id = i.object_id AND ips.index_id = i.index_id
ORDER BY avg_fragmentation_in_percent DESC;
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










