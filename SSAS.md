# SSAS


- Steps of MOLAP
	1. Use SSIS package to schedule the SQL server Job Agent that run daily and perform incremental load to DW. 
	2. Process cube (two method)
		- Manually (process cube in Analysis server, right click cube and click "Process", click ok
		- Use SSIS to run "Analysis service process task"


- Measure and calculated column
	- calculated column  
	Calculated row by row and save data in table
	- Measure  
	Save expression and calculated when use


-Column Functions
	- RELATED
	- CALCULATE

- Table Functions
	- UNION
	- CROSSJOIN
	- NATURALINNERJOIN
	- CALCULATETABLE
	- LEFTOUTERJOIN

# Partition
- Deploy
- From SSAS --> table right click --> partitions




