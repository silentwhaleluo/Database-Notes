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

 - SCHEME  
	- It is a DB object focused specifically for security. It can be considered as a subset of DB. It provides convenience of maning different user groups to have access only to certain DB objects.
	- Every object (which is under a DB) should have a schema, if no schema is defined while creating the object default schemea is assigned for the object. The default is dbo (database owner)

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
It holds security related information and it is very important DB from migration and security perspective. It holds server level information such DB users, Server logins, passwords, server level settings. All user defined DBs are authenticated using this DB. It can be considered as heart of SQL Server.
- Model   
It is like a template, which is used by SQL server. When user creates a DB SQL server internally creates a copy of Model DB and renames to the name given by user. 
- MSDB   
Schedules used by or created for SQL Job Agent are managed and stored in MSDB. Developers can store/deploy their SSIS packages in MSDB.   
- TempDB   
It is used by SQL Server to handle/manage temp objects such as Temp Tables, snapshots, cursors etc.  
- Resource DB   
This is a hidden system DB. It holds all meta data and resource management info.

	
		
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
		DROP CONSTRAINT 
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
				
			- DATA TYPES (kind of)
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
				
				INSERT INTO test VALUES (1,1,1,null,0)

				INSERT INTO test (salesID, prodID, clientID, Total)VALUES (2,1,1,0)

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
	- INSERT
	```sql
	INSERT INTO TableWithDefault VALUES (1,'a',DEFAULT)
	/*
	Run to 
	*/
	INSERT INTO candidate VALUES(1,'a');
	INSERT INTO candidate VALUES(2,'b');
	INSERT INTO candidate VALUES(3,'c');
	```
	   
	```sql
	/*
	Run to ' ; ' will end. In this query, only insert (1,'a')
	*/
	INSERT INTO candidate VALUES(1,'a');
	INSERT INTO candidate VALUES(2,'b');
	INSERT INTO candidate VALUES(3,'c');
	```
	   
	```sql
	-- copy data from one to other
	INSERT INTO db
	SELECT * FROM DB2;



	
	- UPDATE
	- TRUNCATE

	```
	- INSERT INTO
		- copies data into a existing table
	```sqlserver
	INTERT INTO newtale
	SELECT *
	FROM oldtable
	```
		
- DQL
Data query language
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

	- HAVING
		- Filter groups
		- Data is fultered in buffer after all required data is pulled
		- Commonly use with a group by clause
		- After group by in logical order
		- Use columns in group by clause or aggregate funtion
		
	- ORDER BY
		- It is the only that guarantees a result set which sorted other wise the data is not guaranteed to be displayed  in sorted order
		- run after SELECT (so can and the only one clause use alias name in SELECT)
		- Use as less as possible for save resource
		- Default in ASC; DESC can be use
		- ORDER BY 1 (sort in the first column) (not recommend because databse can be edit)
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
		MiddleName NOT IN ('A','B') --This will not includ NULL values in MiddleName
		MiddleName IN ('A','B')
		-- the two reults cannot union to full set because cannot compare NULL with other values
		MiddleName NOT IN ('A', 'B') OR MiddleName IS NULL
		-- NULL cannot compare with eqaul or uneqaul
		```
		


	- wild card
		- %
		- ^ or ! (not)
		- [] ( LIKE '[a,f,s]%'
		- _  ( stands for any 1 charactor)

	- Precedence   
	The order is following:  
	1. FROM
	2. WHERE
	3. GROUP BY
	4. HAVING
	5. SELECT 
	6. ORDER BY
	



## Datatype
The right datatype will imporve the performance
- String
- NUMBERIC
NUMBERIC(8,2) ~999999.99
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
	
		

		
db_test
create two files:
1. db_test.MDF
2. db_test.LDF
	- log shipping
	- improve only  perations for 
	- helps in back up
	- disastor recorvery

DML,DDL --> db_test --> .LDF --> .MDF

in delete, each delete will create one .LDF
in truncate, only one or two .LDF (regarding to size)

		

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
(1,101)

DELETE FROM machineparts
```

