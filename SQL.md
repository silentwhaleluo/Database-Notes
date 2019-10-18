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
	- DROP
- DML
	- DELETE
	- INSERT
	```sql
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
	```

	
	- UPDATE
	- TRUNCATE
		
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
	- GROUP BY
	- HAVING
	- ORDER BY
	- AS   
	Following kind of code might cuase issues
		- only can be used in SELECT, FROM

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
	``` MySQL
	DROP TABLE IF EXISTS 'test';

	CREATE TABLE 'test' (
	'a' int NOT NULL,
	'b' varchar NOT NULL
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

	ALTER TABLE test ADD COLUMN c INT AFTER* a
	```
	
	
