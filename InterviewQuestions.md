1.	Tell me about your experience in your last project. What was the day to day responsibilities?
2.	What is the advantage of using stored procedure?
	1. Save execution plan and thus improve the performance
	2. Reusable
	3. Security (store procedure can create with ENCRYPTION
	4. Reduce network traffics
	5. Modularize
	6. Nesting recursion
	7. Accepts parameters

3.	What subscription ssrs have?
4.	How to subscribe the report to email?
5.	If you have two tables in a report and while rendering you want to render those tables in different sheets, how can you achieve that?
6.	If your report data is not matching when the client check in their side, how would you fix that?
7.	How will you troubleshoot slow running reports?
8.	How to optimize SQL query?
	- For developer
	1. Look into the execution plan and see if there are any operators which are bad for performance(EX: SORT, SPOOL, RID LOOKUP, KEY LOOKUP)
	2. avoid using * in the SELECT statement
	3. Use 2 part or 3 part naming convention
	4. Avoid using co-related sub queries and try using JOINs if related to SELF JOIN
	5. Try using WINDOW functions situations related to running totals, LAG and LEAD situations
	6. SET NOCOUNT ON
	7. Try to filter the data as much as possible
	8. Try to do batch operations when dealing with DELETEs or UPDATEs
	9. Avoid using WILD CARDS
	10. Use proper CONTROL FLOW
	11. Use proper logical operators when searching
	12. Use THROW over RAISERROR
	13. Avoid deeper NESTING in Sub Queries, Functions and SP
	14. Use UNION ALL instead of UNION if results don't differ
	15. Use JOINs instead of Sub Queries (in most of the cases)
	16. create procedures to make use of precompiled set of SQL code and execution plan caching
	18. Try to modularize a complex procedure with multiple simple procedures. In some cases this will solve Parameter Sniffing
	19. Avoid using CURSORs and WHILE loops as much as possible. Try replacing CTE if possible
	20. While using SCALAR UDFs use RETURN NULL ON NULL INPUT
	21. Avoid ORDER BY as much as possible
	22. Make use of INLINE UDFs over MULTI-LINE UDF
	23. Base on the size of data use TABLE VARIABLES over TEMP TABLES
	24. Avoid using SCALAR functions in WHERE condition
	25. Avoid TRIGGERS on tables where you have log of DML operations
	26. Avoid using LINKED SERVERS try using SSIS package
	27. Use WITH NOLOCK whenever it is possible
	28. Use HINTS as last resort
	29. Minimize the implicit conversion
	30. Monitor database using Query Store to understand queries ran against database
	- For admin
	1. Create indexes as needed on columns used for filtering (covering, filter index)
	2. Use INT data types as much as possible for CI
	3. Create NCI on columns on which FOREIGN KEYS are defined
	4. create partitions on large tables based on a column that is used in searching
	5. Use profiler??? to find slow running queries or bottle necks for performance
	6. Look into fragmentation of indexes and REBUILD or REORGANIZE as necessary
	7. Create INDEX VIEWS to use multiple tables which are frequently searched
	8. When data retrieval is large usually happening tables that doesn't have any DML go for Column Store Index
	9. Use SQL Profiler and DTA to identify the need of new indexes, indexed views, partitions on the tables
	10. Create a trace to find the statistics and update or create them as necessary

	1. Use TRUNCATE when possible
	2. Choose correct data types for the columns
	3. Use BULK operations if it fits the requriment
	4. Refresh or recompile SPs over the time


9.	Have you worked with CTE? And in what scenario?
	Find the Nth Top client. In the CTE use ROW_NUMBER() first to give an rank, and then select N-th top
10.	How to check and delete duplicate values?
	1. Group by
	2. ROW\_NUMBER()
11.	When you start at a new place, what is your approach to quickly understand their data?
	1. Find Database Diagrams to find the relationship between each other
	1. Data profiling to find statistic of columns
	???

12.	What is the biggest challenge?
13.	There are order and customer table. Order table has CID,OID, Salesamount. Customer table has CID, Cname.Please write a SP to bring the data to SSRS ??? and set up start date and end date parameters.
	```sql
	CREATE PROC usp_custdata(
		@StartDate DATE,
		@EndDate DATE)
	AS
	BEGIN
		SELECT O.CID, O.OID, O.Salesamount, C.Cname
		FROM customer C JOIN order O on C.CID=O.CID
		WHERE  ???

	END
	GO

	```

	
14.	If you must run 100 reports overnight and you get some errors, where do you check for the status of the report? 
15.	What are the different roles in reporting services? Who do you set it for?
16.	The difference between cross join and full join?
	1. cross join is do cartesian product, all combination
	2. Full join will include the non-match records as well
17.	Complex reports you created and why is it complex?
	drill through reports for RegionWiseOrder report. Because Brinker has more than 16 hundreds restaurants and for each restaurants and each region, we have to create many child reports for each restaurant and each region to perfom detailed information on each hierarchy.
18.	How do you gather the report requirements?

19.	What kind of information do you find in report server database?
20.	Could you work alone?
21.	What is the difference between primary key and unique key?

|PRIMARY KEY|UNIQUE KEY|
|-----------|----------|
|Only 1|multiple, 999|
|UNIQUE + NOT NULL| Can have 1 null|
|By default, CI|By default, NCI|
|mantain Entity Integrity|N/A|
22.Can CTE have index? How about temp table?
Yes, Yes

