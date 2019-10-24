## Why use SQL server?
1. Security
2. Parallel access
3. Back up
4. History
5. Generate reports
6. Encryption
7. Masking

## Tools
- SQL Server Management Studio: Clinet tool to connect data server, mostly connect to development 

## Machine and environment
- Machine
	- OS
	- Processor
	- Memory
- Development Environment

## Basic Concepts
- Instance  
It is an installation of SQL Server on a machine (Windows/Linux).
	- 2 Types of Instances are possible 
		1. Default 
Only 1 Default per machine
		2. Named 
Up to 50 instances per machine are possible 
	
- Session  
Session can be defined as an **interaction** of an application with SQL Server for a specific time (usually sessions time out if there is no activity after certain period) to perform an operation. User sessions start from 50 and above, system session will use SPID < 50.

- Authentication
	1. Windows 
Information is passed from Windows system to SQL Server. This is most commonly used type.
	2. SQL Server 
 Has to provide Login and Password		

 - SCHEMA  
	- It is a DB object focused specifically for security. It can b econsidered as a subset of a DB. It provides convenience of managing different user groups to have access only to certain DB objects.  
	- Every object (which is under a DB) should have a schema, if no schema is defined while creating the object default schema is assigned for the object. Default schema is DBO (database owner)
	```sql
	ALTER TABLE [dbo].[B32_Candidates]
	ALTER COLUMN [Candidate_ID] INT

	ALTER TABLE [dbo].[B32_Candidates]
	ALTER COLUMN [Candidate_ID] INT NOT NULL
	```
## Connecting to different Server Categories
1. Local Server 
This is an instance of server that is installed on a local machine like laptop or desktop.
	- Default Local Instance 
		(local)  
		.   
(dot)  
		localhost  
		MachineName
	- Named Local Instance
		(local)\<Name of the Instance>  
		.\<Name of the Instance>  
		localhost\<Name of the Instance>  
			MachineName\<Name of the Instance>  
2. Remote Server 
This is an instance of server that is installed on a remote machine like a blade server, rack server etc. You would connect to it through network like Internet.
	- IP Address
	- Server Name Alias 

## Types of DBs
1. User Databases 
Created by user
2. System Databases 
These are created when SQL Server is installed.	

- Master   
It holds **security** related information and it is very important DB from migration and security perspective. It holds **server level information** such DB users, Server logins, passwords, server level settings. All user defined DBs are authenticated using this DB. It can be considered as heart of SQL Server.
- Model   
It is like a **template**, which is used by SQL server. When user creates a DB SQL server internally creates a copy of Model DB and renames to the name given by user. 
- MSDB   
**Schedules** used by or created for **SQL Job Agent** are **managed** and stored in MSDB. Developers can **store/deploy** their **SSIS** packages in MSDB.   
- TempDB   
It is used by SQL Server to handle/manage **temp objects** such as **Temp Tables, snapshots, cursors** etc.  
- Resource DB   
This is a **hidden** system DB. It holds all **meta data** and **resource management info**.

	
		
## Shortcuts for operation
C+n : open new query  
C+e : run  
A+x : run  
F5 : runkk  
		
## DB objects
Store data:  
- TABLE
- INDEXES
STORE SQL CODE:  
- VIEW
- PROCEDURES
- FUNCTIONS
- TRIGGERS

## Query
- DDL
DDL are used to create, alter, or drop any database objects
	- CREATE
	```sql
	CREATE TABLE B32_Candidates
	(
	Candidate_ID NUMERIC(3, 0),
	Candidate_Name VARCHAR(100)
	)
	```
	- ALTER
	Can not change column name
		- add constraints
		```sql
		ALTER TABLE sales
		ALTER COLUMN salseID INT NOT NULL;

		ALTER TABLE sales
		ADD CONSTRAINT pk_sales PRIMARY KEY (SalesID)
		

		ALTER TABLE sales
		ADD CONSTRAINT fk_sales_clinet FOREIGN KEY (clientID) REFERENCES client(clientID)

		```
	- DROP
		```sql
		ALTER TABLE client
		DROP CONSTRAINT FK_Sales_Clinet
		```
	- SELECT INTO
		- DDL comman to create a table and optional data entry to it
		- copies data from existing tables and can insert them to newly created table in the same syntax.
		- Can copy across databases with 3 part naming: DB.SCHEMA.OBJECT 
		- Can derive new columns and change columns names
		 (4 part naming: LinkedServer.DB.SHEMA.OBJECT)
		 ```sql
		 SELECT * INTO Emplyee
		 FROM advanworks.humanresource.employee
		 
		 SELECT * INTO traning.dbo.employee
		 FROM h.humanresource.employee

		 SELECT gender as 'sex', title, COUNT(*) 'cnt'
		 INTO newtable
		 FROM oldtable
		 GROUP BY gender, title


		 ```
	- constraints
		- key constraints
			- PRIMARY KEY  
				- By default SQL server creates a clustered index on the primary key column
				- Only one
				- Not allow NULL

			- UNIQUE KEY  
				- By default SQL server creates a Non clustered Index on the unique key column
				- Can have 999
				- Allow NULL
			

			- FOREIGN KEY
				1. must have unique key in parent
				2. FK must have the same datatype
		- other constraints
			- NULL, NOT NULL
			- CHECK
				
				```sql
				-- This is not work, because in columns level
				CREATE TABLE patient(
				patID INT NOT NULL,
				adminDate DATE NOT NULL,
				DischargeDate DATE CHECK( AdminDate <= DischargeDate)
				)


				-- Tihs will work
				CREATE TABLE patient(
				patID INT NOT NULL,
				adminDate DATE NOT NULL,
				DischargeDate DATE,
				CHECK( AdminDate <= DischargeDate)
				)
				```
			- DEFAULT  
			only **one** default value 
				

		- method to add constraint
			1. create tables and add constraints later with ALTER
			2. create tables and add constraints at same time
				```sql
				CREATE TABLE test
				(salesID int PRIMAR KEU,
				prodID int UNIQUE,
				clientID int NOT NULL,
				qty INT DEFAULT 0,
				total MONEY);

				DROP TABLE test;

				CREATE TABLE test
				(salesID int CONSTRAINT pk_test PRIMAR KEY,
				prodID int CONSTRAINT uk_test UNIQUE,
				clientID int NOT NULL,
				qty INT DEFAULT 0,
				total MONEY)
				
				INSERT INTO test VALUES (1, 1, 1, null, 0)

				INSERT INTO test (salesID, prodID, clientID, Total)VALUES (2, 1, 1, 0)

				```
			3. create tables with contraints in different sentence
				```sql
				CREATE TABLE test
				(salesID INT NOT NULL,
				prodID INT NOT NULL,
				clientID INT NOT NULL,
				qty INT,
				total MONEY)

				CONSTRAINT pk_test PRIMARY KEY (salesID)
				CONSTRAINT fk_test FOREIGN KEY (prodID) REFERENCE prod(prodID)
				```

- DML
	- DELETE
	```sql
	DELETE FROM B32_Candidates
	WHERE Candidate_ID = 1 and Candidate_Name IS NULL
	```
	- INSERT
	```sql
	-- Method 1
	INSERT INTO B32_Candidates VALUES
	(1, 'Yiping'),
	(2, 'Mounir'),
	(3, 'Swornim')

	-- Method 2 ( when run to ' ; ', it will end
	INSERT INTO B32_Candidates VALUES (1, 'Yiping');
	GO
	INSERT INTO B32_Candidates VALUES ('A', 'Mounir');
	GO
	INSERT INTO B32_Candidates VALUES (3, 'Swornim')

	-- Method 3 (this way the number of value can less than number of columns, the columns not show here will be NULL or default.
	INSERT INTO B32_Candidates (Candidate_Name, Candidate_ID) VALUES ('Yiping', 1);

		-- Not work because number not match
		INSERT INTO B32_Candidates VALUES (1); 
		
		-- Work and if the not show columns can be NULL, and if so, the not show columns will be NULL
		INSERT INTO B32_Candidates(Candidate_ID) VALUES (1); 

	CREATE TABLE B32_Backup (ID INT, CName VARCHAR(100))

	INSERT INTO B32_Backup
	SELECT * FROM B32_Candidates

	SELECT * FROM B32_Candidates
	WHERE Candidate_ID = 1 and Candidate_Name IS NULL

	```



		
	- UPDATE
	```sql
	UPDATE B32_Candidates
	SET Candidate_Name = 'May'

	 CREATE TABLE Salary (ID INT, CName VARCHAR(10), Salary INT)

	INSERT INTO Salary VALUES
	(1, 'Yiping', 25000),
	(2, 'Mounir', 45000),
	(3, 'Swornim', 40000)

	SELECT * FROM Salary

	UPDATE Salary
	SET Salary = Salary*1.05

	UPDATE Salary
	SET Salary = Salary+(5/100)*Salary

	UPDATE Salary
	SET Salary += 0.05*Salary
	```

	- TRUNCATE
![Different between delete and truncate](Pictures/SQL/Diff_Del-Trun.JPG) 
	```sql
	TRUNCATE TABLE B32_Candidates
	```

	- INSERT INTO
![Different between SELECT INTO and INSERT INTO](Pictures/SQL/Diff_SelectInto-Insert.JPG) 
		- copies data into a existing table  
	```sql
	INSERT INTO newtale
	SELECT *
	FROM oldtable
	```
		
- DQL
Data query language
	- Precedence   
	The order is following:  
	1. FROM
	2. WHERE
	3. GROUP BY
	4. HAVING
	5. SELECT 
	6. ORDER BY
	- SELECT  
		- Filter columns in SELECT statement
		- Column expressions in SELECT
		```sql
		SELECT E.Business_ID, 'I have to go home'
		FROM humanresource.Emplyee E
		/*
		1. select rows one by oneneeded E.Business_ID as one columns (select one row, do the logical in select, and then process next row)
		2. Each row, add another row as 'I have to go home'
		*/
		```

	- FROM
	- WHERE
		- row filters
		- logical operators: AND, OR, NOT, BETWEEN, IN
		- run conditions from left to right with same operators
			- For AND, if the first is not meet, will not go the second condition, thus well affect the **performance running time**
			- For OR, change the condition order will only impact the running time
		- Operator precedence:

			1. ~ (bitwise NOT)
			2. 
		- The most clear mothed is to use () brackets
	- GROUP BY
		- use to show distinct values
		- NULLs are considered as same in GROUP BY
		- SELECT with group by, the columsn must be part of GROUP BY clause or aggregate functions

	- HAVING
		- Filter groups with conditions (must be able to compare)
		- Data is fultered in buffer after all required data is pulled
		- Commonly use with a group by clause, but can use without GROUP BY
		- After group by in logical order
		- Use columns in group by clause or aggregate funtion
![different between where and having](Pictures/SQL/Diff_Where-Having1.JPG) 		
	- ORDER BY
		- It is the only that guarantees a result set which sorted other wise the data is not guaranteed to be displayed  in sorted order
		- run after SELECT (so can and the only one clause use alias name in SELECT)
		- Use as less as possible for save resource
		- Default in ASC; DESC can be use
		- ORDER BY 1 (sort in the first column) (not recommend because databse can be edit)
		- The columns order in GROUP BY will not have any affect the result set
		- Be used in 2 scenarios:
			- Find duplicate
			- Get distinct 
	- AS   
	Following kind of code might cuase issues
		- only can be used in SELECT, FROM
	- BETWEEN	
		- Can be used in int, charactor, date
			```sql
			SELECT *
			FROM person
			WHERE FirstName BETWEEN 'A' AND 'C'
			/*
			 will not show start with c
			 EX. only show till C ('C' is included)  but without any like Cadey
			 */

			 SELECT *
			 FROM test
			 WHERE testdate BETWEEN '01/01/1061' AND '12/31/1970'

			```
		- is included  
			> EX.
			```sql
			SELECT * FROM TEST
			WHERE BETWEEN 1 and 100 == >=1 and <=100  
			>\>1 AND <100 == BETWEEN AND !=1 AND != 100 (just for INT)
			```
		- Not good to use in decimal
		
	- IN	
		```sql
		MiddleName NOT IN ('A', 'B') --This will not includ NULL values in MiddleName
		MiddleName IN ('A', 'B')
		-- the two reults cannot union to full set because cannot compare NULL with other values
		MiddleName NOT IN ('A', 'B') OR MiddleName IS NULL
		-- NULL cannot compare with eqaul or uneqaul
		```
		


	- LIKE  
	will internal convert to string to compare  
	for like, SQL server cannot use index efficiently and thus for some case will use all records to compare if there is % at first, is not an efficient way
		- use to string
		- use to number
		```sql
		SELECT *
		FROM TestWC
		WHERE TotalDue NOT LIKE  '%.00'
		```
		- use to date (not recommanded)
		```sql
		SELECT HireDate
		FROM Employee
		WHERE HireDate LIKE '2009%'

	- Wild Card  
	A wildcard character is used to substitute one or more characters in a string.
		- %
		- ^ (carat) or ! (not)
		- [] (Square brackets)
			- ( LIKE '[afs]%', 
			- [a-m]% , 
			- [a-M-]% the last one will inclueded ' - ')
		- _  ( Underline, stands for any 1 charactor)
		- ESCAPE change the charactor sel meaning
		Can sign any un special character as ESCAPE charactor
		```sql
		SELECT *
		FROM wildcards
		WHERE descp LIKE '%*%%' ESCAPE '*'
		-- Here we want strings with ' % '
		
		-- Alternaltively, we can use  [ ]
		SELECT *
		FROM wildcards
		WHERE descp LIKE '%[%]%' ESCAPE '*'
		
		```

	
	- Aggregate  
		1. With GROUP BY, after GROUP BY clause (HAVING, SELECT, ORDER BY) have only in GROUP BY columns and AggF(columns)
		2. Create derive column, should give name with AS alis
		3. Can oly filtered in HAVING and not limited to only what in SELECT
		4. What data types does aggregate functions support:
		5. Aggregate funtion cannot be **nested**: Ex: MAX(SUM(Sales)) --this not work
		6. AggF is not mandatory to be included in GROUP BY
			- Numeric
			- INT
			- Float
			- Money
			- Real
			- Date ( Can use AggF except SUM, AVG)
			- String ( Can use AggF except SUM, AVG)
		7. Can be used in ORDER BY clause but not recommanded (recommand to aggF in select and alias a name
		```sql
		-- Work but not recommand
		SELECT SalesPersonID
		FROM Sales.SalesOrderHeader H
		GROUP BY SalesPersonID
		ORDER BY SUM(TotalDue)

		-- recommand
		SELECT SalesPersonID, SUM(TotalDue) 'Total'
		FROM Sales.SalesOrderHeader H
		GROUP BY SalesPersonID
		ORDER BY Total
		```
		```sql
		-- in SELECT with out group by 
		SELECT MAX(TotalDue) as 'Max sales'
		FROM Sales;

		-- in SELECT with GROUP BY and HAVING
		SELECT custID, SUM(TotalDue)
		FROM Sales
		GROUP BY custID
		HAVING SUM(TotalDue) > 5000
		ORDER BY custID
		```
		- SUM
		- MIN  
		Can use ORDER BY and TOP to get the min and max result
		- MAX
		- AVG
		- COUNT
		COUNT is the only aggregate function that will not ignore NULL values. With null values, avg() and sum()/count() is not equal, the formal one is larger
		- WITH
		```SQL
		```

 	- JOIN
		- SELECT should indicate the the columns are in which table (mostly with ALIAS in FROM)
		- Will not match NULL values at join
		- Equal join
		- Non-Equal Join  

			AID > BID; AID < BID; AID < > BID
		- Can use metadata sys.foreign keys to find relationships
		- Type
			- INNER JOIN
			- OUTER JOIN
			Display every possible combination of all values in the designated / Cartesian product
			- CROSS JOIN  
				The following are derived not a real JOIN type
				- Restricted Left/Right Outer  
				Only unique left/right values (not match values)
				```sql
				-- Find movies without any reviews
				-- Restricted Left Outer Join
				SELECT *
				FROM Moive M
					LEFT JOIN Review R
					ON R.MovieID = M.MovieID
				WHERE R.MovieID IS NULL


				```
				- Self Join  
				join a table to itself in some regard
				```sql
				/*
				CREATE TABLE Eomployee(
				EmpID INT PRIMARY KEY,
				Name VARCHAR,
				ManagerID INT
				)

				-- Find who do not manage anyone
				SELECT E1.EmpID 'Manager", E1.Name "Manager Name" 
				FROM Employee E1
					LEFT JOIN Employee E2
					ON E1.EmpID = E2.MangagerID
					WHERE E2.EmpID is NULL

				*/
				```


		- Inner Join  
		No matter table order
		- Outer Join  
		' a LEFT JOIN b' == ' b RIGHT JOIN a'
			- LEFT JOIN  
			'LEFT JOIN' AND 'LEFT OUTER JOIN' are same
			- RIGHT JOIN
		- Full Join

		Exercise:
		Find records in table A which are not available in B
		Methods:
		1. OUTER JOINS (The best way, best performance)
		2. Sub queries
		3. Set operator



## Datatype
The right datatype will imporve the performance
- String
- NUMBERIC
NUMBERIC(8, 2) ~999999.99
- FLOAT(N)
Will round at the end
- REAL
Will round numbers
subset of float
- DATETIMEOFFSET
include the position about date zone
- XML
- IMAGE
- GEOGRAPHY
- VARBINARY
- USER DEFINED DATA TYPE
- DATE  
	'02/29/1961' as date will get erro because SQL will automatic convert string to date. This date is not exsist and thus cannot successfully internal convert to date.

|Category|Data Type|Size|Max Val|
|--------|---------|----|------|
String|
|CHAR|1 Byte Per Char|8000 CHARS|
|VARCHAR|1 Byte Per Char|8000 CHARS|
|NCHAR|2 Byte Per Char|4000 CHARS|
|NVARCHAR|2 Byte Per Char|4000 CHARS|
|NVARCHAR(MAX)|2147483648 (2 GB)|1073741824 CHARS|
|VARCHAR(MAX)|2147483648 (2 GB)|2147483648 CHARS|
Number|
|TINY INT|1|0-255|
|SMALL INT|2|
|INT|4|
|BIG INT|8|
|NUMERIC(P,S)|5, 9, 13, 17|
|FLOAT(N)|4,8|
|REAL|4|
|MONEY|8|
|SMALL MONEY|4|
|BIT*|1 BIT|0/1|
Date|
|DATE|3|
|DATETIME|8|
|DATETIME2|6,7,8|
|DATETIMEOFFSET|10|
|SMALL DATETIME|4|
|TIME|5|


### DataType procedure
1. user-defined data types (highest)
4. datetimeoffset
5. datetime2
6. datetime
7. smalldatetime
8. date
9. time
10. float
11. real
12. decimal
13. money
14. smallmoney
15. bigint
16. int
17. smallint
18. tinyint
19. bit
20. ntext
25. nvarchar (including nvarchar(max) )
26. nchar
27. varchar (including varchar(max) )
28. char
3
		

		
db\_test
create two files:
1. db\_test.MDF 

2. db\_test.LDF
	- log shipping
	- improve only  perations for 
	- helps in back up
	- disastor recorvery

DML, DDL --> db\_test --> .LDF --> .MDF

in delete, each delete will create one .LDF
in truncate, only one or two .LDF (regarding to size)

## Functions

- Cateory
	- System
	- User
- Date functions  
Can be nested
	- DATEADD(interval, number, date)
	- DATEDIFF(interval, date1, date2)
	```sql
	SELECT DATEDIFF(YY,'2019-12-31','2017-12-21') 'Date diff inYears',
	DATEDIFF(MM,'2015-8-16','2017-12-21') 'Date diff in Months',
	DATEDIFF(DD,'2015-8-16','2017-12-21') 'Date diff in Days',
	DATEADD(MM,12,GETDATE()) 'Adding Months',
	DATEADD(YY,-2,GETDATE()) 'Adding -ve Years'
	-- This add 10 days to the current day. Only add on days
	SELECT GETDATE() + 10
	```
	- DATEFROMPARTS(year, month, day)
	```sql
	--SQL Server 2012 and later versions
	SELECT DATEFROMPARTS(2012,2,28) 'Date from Parts'
	--DATETIMEFROMPARTS, SMALLDATETIMEFROMPARTS
	```
	- DATENAME(interval, date)
	- DATEPART(interval, date)
	```sql
	DATENAME(MONTH, DemoDate) 'Month Name',
	DATENAME(DD, DemoDate) 'Day of Month',
	DATENAME(WEEKDAY, DemoDate) 'Name of Day',
	DATENAME(DY, DemoDate) 'Day of Year',
	DATEPART(M, DemoDate) 'Month of Year',
	DATEPART(MI, DemoDate) 'Minutes of DateTime',
	DATEPART(WEEK, DemoDate) 'Week of the Year',
	```
	- DAY(date)
	- MONTH(date)
	- YEAR(date)
	```sql
	SELECT DemoDate 'Current Date',
	DAY(DemoDate) 'Day from Date',
	MONTH(DemoDate) 'Month from Date',
	YEAR(DemoDate) 'Year from Date'
	FROM DateFnDemo
	```
	- GETDATE()
	- GETUTCDATE()
	- SYSDATETIME()
	- ISDATE(expression)
	```sql
	SELECT
	ISDATE('2015-8-31') 'Validation Correct',
	ISDATE('2015-2-35') 'Validation Wrong'
	--SQL Server 2012 and later
	SELECT EOMONTH ('2015-8-16'), EOMONTH ('2015-8-16',-1)

	SELECT EOMONTH (GETDATE(), 12)
	--Gives the datetime value of server
	SELECT SYSDATETIME(), GETDATE()
	```
	- EOMONTH ( StartDate [, MonthToAdd ] )  
	```sql
	DECLARE @date DATETIME = GETDATE();
	SELECT EOMONTH ( @date ) AS 'This Month';
	SELECT EOMONTH ( @date, 1 ) AS 'Next Month';
	SELECT EOMONTH ( @date, -1 ) AS 'Last Month';
	```
- String Function  
Can be nested
	- LEN()  
	Will count the space before the last non character but not space after the last non-space charactor
	- UPPER()
	- LOWER()
	- LTRIM()
	- RTRIM() RTRIM( LTRIM() )
	- CONCAT() 
	- Difference between CONCAT() and +
		1.  ++ will return NULL if any is NULL; CONCAT() will ignore NULL and retur other values
		2. ++ can not plus number and string (should conversion number to string first); CONCAT() will automatically convert number to string
	- SUBSTRING(string, start position, number of charactors)
	- LEFT(string, number of charactor to be sub)  
	From left
	- RIGHT (String, number of charactor to be sub)  
	From right
	- REPLACE(String, OldString, NewString)
	```sql
	-- number of apperances of character(s)
	SELECT LEN('RAJA') - LEN( REPLACE('RAJA', 'A',''))
	```
	- CHARINDEX(charactor to be find, string, start position (optional,this position is included)  
	Return the first appearance of a character in string, if cannot find, return 0, will count space. If the start position is <= 0, looks as 0 to find from all;
	CHARINDEX('P',PerFName)  
	Return the index location (start with 1) of  
	```sql
	-- Find the second apperance of a character
	CHARINDEX('P', string, CHARINDEX('P', string)+1)
	```
	- PARTINDEX(pattern, string)  
	Return the index of a pattern; Can use wild card
	- RVERSE(string)
	- REPLICATE(repeat string, times of repeat)
	- ASCII(character)  
	IF more than 2 characters, only return the ASCII value of 1st one
	- CHAR(ASCII value)
	- STUFF(string, start, length, NewString) 
### Exercise
```sql
--Find Employees who are retiring on Weekend assuming age of retirement is 60
--Using DATENAME function
SELECT BusinessEntityID, BirthDate, DATENAME(WEEKDAY, DATEADD(YY, 60, BirthDate)) 'Day of Retiring'
FROM AdventureWorks2012.HumanResources.Employee
WHERE DATENAME(WEEKDAY, DATEADD(YY, 60, BirthDate)) IN ('Saturday','Sunday')
--Using DATEPART function
SELECT BusinessEntityID, BirthDate, DATENAME(WEEKDAY, DATEADD(YY, 60, BirthDate)) 'Day of Retiring'
FROM AdventureWorks2012.HumanResources.Employee
WHERE DATEPART(WEEKDAY, DATEADD(YY, 60, BirthDate)) IN (1, 7)

--People hired on Monday
SELECT *
FROM AdventureWorks2012.HumanResources.Employee
WHERE DATENAME(WEEKDAY, HireDate) = 'Monday'
```	
		

- Conversion Functions  
	- CAST - It is universal, that means it is available in most of the databases such as Oracle, MySQL
	- CONVERT - Convert uses style parameter, it is a variation of CAST developed by MS
```sql
SELECT CAST('01/01/2017' AS DATE)
SELECT CAST('01/33/2017' AS DATE)
SELECT CONVERT(DATE, '01/01/2017')

SELECT BusinessEntityID, HireDate, JobTitle,
CONVERT(VARCHAR(100), BirthDate, 107), CONVERT(VARCHAR(100), BirthDate, 7)
FROM HumanResources.Employee

-- User try can avoid stop when have error when running
SELECT TRY_CAST('01/33/2017' AS DATE)

CREATE TABLE COnversionFNs 
(BDay VARCHAR(25))
GO

INSERT INTO COnversionFNs values
('01/15/2018'),('02/29/2019'),('05/15/2016'),('09/31/2019'),('2/15/2018')

-- Use WHERE to avoid stop, same usage as TRY. Both are not good
SELECT * FROM COnversionFNs
WHERE ISDATE(BDay) = 1

SELECT * FROM COnversionFNs
WHERE TRY_CAST(BDay AS DATE) IS NOT NULL

SELECT *, CAST(BDay AS DATE)  
FROM COnversionFNs

SELECT BusinessEntityID, HireDate, JobTitle,
TRY_CONVERT(VARCHAR(100), BirthDate, 117)
FROM HumanResources.Employee
```
	
## Parent child
 - DELETE child first then DELETE parent

 - INSERT parent first then DELETE child
 - Change in both parent and child
 	1. DROP FK --> UPDATE parent --> UPDATE child --> RE-Create FK
	2. CASCADE function
	3. (recommanded)Insert new value in parent --> change child to new value --> delete old value in parent
	

macine, parts, machine-part question	

```sql
-- must drop child table fisrt and then drop parent table
DROP TABLE IF EXISTS machineparts
GO
DROP TABLE IF EXISTS machine
GO
DROP TABLE IF EXISTS parts
GO
CREATE TABLE machine
CREATE TABLE parts
(partID SMALLINT NOT NULL,
part VHARCHAR(100) NOT NULL,
partcat VHARCHAR(100),
-- Define the unique with CLUSTERED INDEX`
CONSTRAINT uk_parts_partID UNIQUE CLUSTERED(partID)
)
GO
CREATE TABLE machineparts
(machineID INT CONSTRAINT fk_machineparts_machineID FOREIGN KEY REFERENCES machine(machineID) ,
(partID SMALLINT CONSTRAINT fk_machineparts_partID FOREIGN KEY REFERENCES part(partID) ,
-- for foreign key, the datatype should be match

ALTER TABLE machineparts
ALTER COLUMN machineID INT NOT NULL,

ALTER TABLE machineparts
ALTER COLUMN partID SMALLINT NOT NULL,
-- must change to not null and then change to primary key
ALTER TABLE machineparts
ADD CONSTRAINT pk_machinepart_machID_partID PRIMARY KEY (machineID, partID)

-- must insert parent first and then insert child
INTER INTO machine VALUES
(1, 'laptop', '5/15/2019')

GO
INSERT INTO parts VALUES
(101, 'mother board', '5/15/2019')

GO
INSERT INTO machineparts VALUES
(1, 101)

DELETE FROM machineparts
```

# System Procedure
- Important
Retrieves the info about an object  
	- EXEC SP_HELP '[dbo].[uspGetBillOfMaterials]'
	- --Gives out the definition of the user defined objects and system objects (Not for table and system functions)
	- EXEC SP_HELPTEXT '[dbo].[uspGetBillOfMaterials]'
	- EXEC SP_HELPTEXT 'SP_HELP'
	- EXEC SP_HELPTEXT '[HumanResources].[Employee]'
	- EXEC SP_HELPTEXT '[dbo].[fnExtCharIndex]'
	- --Provides info about a specified database or all the databases (DB name is optional)
	- EXEC SP_HELPDB 'Training_SQL'
	- EXEC SP_HELPDB 'AdventureWorks2012'
	- EXEC SP_WHO2
	- EXEC SP_WHO
	- EXEC SP_EXECUTESQL N'SELECT * FROM Person'
	```sql
	EXEC SP_EXECUTESQL N'SELECT * FROM Person'
	```
	Why? Use as dynamical execute query
	- EXEC SP_LOCK
	- EXEC SP_RENAME 'B27_ColumnName.newid', 'ID', 'COLUMN';
	- EXEC SP_RENAMEDB 'Training_SQL', 'Training_SQL2'
	- --What are the different objects that depend on a given object
	- EXEC SP_DEPENDS '[dbo].[uspGetBillOfMaterials]'

- Good to remember
	- EXEC SP_WHO 'ACTIVE' --login' | session ID | 'ACTIVE'
	- EXEC SP_ADDTYPE PhoneNo, 'varchar(13)', 'NOT NULL'
	- EXEC SP_DROPTYPE 
	- EXEC SP_COLUMNS 'Emp'
	- --Provides info about indexes on a Table or View
	- EXEC SP_HELPINDEX '[HumanResources].[Employee]'
	- --To attch .df, .ldf and .ndf files to a server instance
	- EXEC SP_ATTACH_DB @dbname=  'AdventureWorks2008', @filename1= 'C:\.........'
	- EXEC SP_DETACH_DB
	- --Provides the info about the files associated to DB .mdf, .ldf, .ndf
	- EXEC SP_HELPFILE
	- --Provides the information about language, months, days, start of the week
	- EXEC SP_HELPLANGUAGE English;
	- EXEC SP_HELPSERVER
	- EXEC SP_HELPSORT
	- EXEC SP_BINDRULE
	- EXEC SP_UNBINDRULE
	- EXEC SP_HELPTRIGGER '[Person].[Password]'
	- EXEC SP_SPACEUSED 'Employee'

- Some more research
	- EXEC SP_CONFIGURE

- Gives the info of all the constraints on a given object
	- EXEC SP_HELPCONSTRAINT 'HumanResources.Employee'
	```sql
	EXEC SP_HELPBD
	EXEC SP_WHO2
	```

- SP\_WHO SP\_WHO2  
for SP\_WHO OR  SP\_WHO2, there is 'blk by' columns means for a session, it is block(wait other session to run and finished) by other session number. Can find the session need to kill.






- SP\_LOCK
- SP\_RENAME   
rename column name
```sql
EXEC SP_RENAME 'Table', 'OldColumnName', 'NewColumnName'
```

- SP\_DEPENDS
Find all tables depend (child table) on this table. Find what tables will be impact if change this table in structure change.

 

# Exercise
### LIKE
```sql

CREATE TABLE PERSON(
PERID INT CONSTRAINT PK_PERSON_PerID PRIMARY KEY,
PerName VARCHAR(25),
SSN CHAR(9) CONSTRAINT CK_SSN CHECK ( SSN LIKE '[0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]')
-- Using CHAR because if use number, it chould be start with 0
EMAIL VHARCHAR(50)

ALTER TABLE PERSON
ADD CONSTRAINT CK_PerName CHECK (PerName LIKE '_____%, ___%')
ADD CONAT
GO
ALTER TABLE PERSON
ADD CONSTRAINT CK_Email CHECK (EMAIL LIKE '_____%@___%.___%')

--check invalid email for user name and rest are fine
--unit test, check every thing as design
INSERT INTO Person VALUES
(2, 'Yue, li' ')

```
```sql
SELECT PH.ProdHouse, 
	CASE 
		WHEN SUM(C.CollectionAmt)> SUM(M.Budget) THEN '2'
		ELSE 0,
		WHEN SUM(C.CollectionA
(SUM(C.CollectionAmt)> SUM(M.Budget)) AS 'IsProfitable'
FROM ProdHouse PH
	LEFT JOIN MovieProd MP
	ON PH.ProdHID = MP.ProdHID
	JOIN Movie M
	ON MP.MovieID = M.MovieID
	JOIN Collections C
	ON M.MovieID = C.MovieID
WHERE ReleaseDate BETWEEN '2018-01-01' AND '2018-12-31'
GROUP BY ProdHID, ProdHouse

```
 TO be solved:
	-Schreenshot TDP model P6 rules on google drive
	-Cascade TDP model function
	- Difference between PK and FK
	- WITH clause


SELECT COUNT(DISTICNT GenreCode)
FROM Genre

SELECT PH.ProdHouse
FROM ProdHouse PH
	JOIN MovieProd
	JOIN Movie M

ON 
# Research Questions
1. Can you add a column in between existing columns.
	Default will add at the end
	EX. col1, col2, col3, ...etc. Can add like a col10 between col2 and col3.
	Answer: In MySQL, could
	``` SQL
	DROP TABLE IF EXISTS 'test';

	CREATE TABLE 'test' (
	'a' int NOT NULL,
	'b' varchar NOT NULL
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

	ALTER TABLE test ADD COLUMN c INT AFTER* a
	```
