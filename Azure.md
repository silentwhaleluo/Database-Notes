# DIfferent between Azure SQL Database vs. Azure SQL Data Warehouse

|Item|Azure SQL Database|Azure SQL Data Warehouse|
|----|----- --- --------|----- --- ---- ---------|
|Max amount of data per database|4TB|1PB|


test CSI on Azure SQL database

Azure Cloush provide service level agreement (SLA)
It says that our data will be available 24/7 99.99% of time

Disadvantage:
1. Security: 1. data lost 2. declouse 
2. Network is a problem 
3. Some application functionality in SSMS many not support in future

Access of Azure DB DW
add ip in fiel

Version:
Azure SQL Database
||Basic|Standar|Premium|
||-----|-------|-------|
|Maximun storage size|2 GB|1 TB|4 TB|
|Maximun DTUs|5|3000|4000|
|Maximun storage size per database|2 GB|1 TB|4 TB|
|Maximun storage size per pool|156 GB|4 TB|4 TB|
|Maximun eDTUs per database|5|3000|4000|
|Maximun eDTUs per pool|1600|3000|4000|
|Maximun number of database per pool|500|500|100|

pool database???


Basic
Standard
Premium
Perform depends on compute size are expressed in terms of DTU (database transaction units), eDTUs (elastic Database Transaction Units) for elastic pools


Error handling:
- Connection issues
	1. connection errors
	2. transient error (transit fault)  
		- Azure system will shifts (reconfiguration) hardware resources to balance workloads (less than 60 seconds). During the reconfiguration time span, there may leas to connectivity issues to SQL Database. Application should be build to expect these transient errors to handle them. Implement retry logic??? not give an error. If use **ADO.NET**, your program is told about the transient error by the throw of **SqlException** (SqlExeption.Number = 11001 Message:"No such host is known"
		- handle 
			- when transient error occurs during a connection try: delay of several seconds and retry the connection
			- when transient error occurs during a SQL query command: Do not immediately retry the command. Instead, after a delay, freshly establish the connection and then retry the command (do not directly retry a SQL statement that failed with a transient error. Establish a fresh connection and then retry the SELECT; when retry UPDATE, the retry logic must ensure that either the entir database transaction finished or that the entire transaction is rolled back
			- interval increase (first wait for 5 seconds, up to maximun of 60 seconds)
			- maximun number of the retries
		- for "ADO.NET Connection Manager", set "ConnectRetryCount" defult is 1; set "ConnectionRetryInterval" defult is 10, "Connect Timeout" defult is 10

2. Other Connection issue 
	- Use ADO.NET connection string
	- add IP address to SQL Server Firewall
	- Open "Port 1433" in windows firewall


- (ETL)SSIS and Azure
	1. create SSIS Catalog in Azure
		1. connect azure with SSMS, select SSISDB in connection properties, create SSISDB
		2. Use Deployment Wizard to deploy project
	2.1 Schedule in Azure server (need use data factory service)
	2.2 schedule a package with SQL Server Agent on premises 
		1. Add Azure SQL Database to you on-premises SQL Server as a linked server (name of linked server, use SQL server native client, add your Azure SQL Database server endpoint, use SSISDB as the initial catalog, set up linked server credentials, set up linked server options with "EXEC sp\_serveroption 'mylinkedserver', 'rpc out', true; 
		2. create a SQL Server Agent job, create new job and schedule

	
- Azure Datawarehouse
	- GEN1
	- GEN2


	

Only support ADO.NET
