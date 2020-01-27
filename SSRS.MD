- SSRS

- RDL (report definition language)
based on XML

- RDLC
client RDL


- Data Sources

	- Shared
	- Embedded

- Reports
	- Query
	- Report
	- Properties
	- Filters

- Drill down
Toggle conditionally hidden report items to control how details data they want to see (for small data. For large dataset consider "Drillthrough"
- Document Map
	index column on preview of report, tree structure with row groups
	- Properties -\> "DocumentMapLabel"
- Bookmark

- Action
	- None
	- Bookmark
	- URL
	- Go to report

- Parameterized
	- Query parameter
	- Report parameter  
	Display on the preview for user to select

	- Properties
		- Visible
		- Hidden  
		hide the report parameter in published report. But still be set on a report URL

		- Internal
		hide and can only be viewed in the report definition (cannot be change after deployment)
		- Available values
		- Default values
		- Advanced

- Drill through 

- Sub report   
A report embedded in another report. It helps to create dashboard

- List Report
"Freeform report"

- Mad report
Geographical information

- Cube report
- Report builder
To build ad-hoc report

- Report builder
	- Linked reports  
	Shortcuts of original reports. W
	- Caching  
	For optimizing a report and is temporary in nature store in report server tampdb
	- Snapshort  
	Permanent in nature sotred in Reportserverdb
	- Execution log ??? (where, how to do)
	Can see the frequent of usge to decide caching snapshot
- Subscriptions
	- Standard  
	Only one format to multiple users
	- Data Driven  
	One report in multiple formats to multiple users

- Security
	- Data level  
	Customize the dataset query to implement security
	- Report level  
	Use hidden properties on data regions and by wrtting expressions
	- Report manager level  
		- security roles
			- Browser  
			View folders, reports, and subscribe the reports
			- Content manager(admin)
			Mange content in the report server. Includes folders, reports, and resources
			- Publisher role  
			publish reports and linked reports to the report server
			- Report builder  
			View report definition

			- Pulisher
			- reports folder

- Report optimization
	- Optimize the query written in a dataset
	- Try to avoid too many expressions on the report which will make the report run slow
	- Avoid too many parameters on the report. Avoid default value
	- Check execution logs create a caching or snapshot of a report to improve performance

- Deployment method
	- SSTD (visual stutdio)
	- Upload a file on report manager
	- Create a report using report builder and save it
	- Using command prompt (CMD) (rs.exe) ???

- configuration manager
	- Encryption key

- lifecircle 
	- Authoring
	- manager
	- delivery

- Native

- snapshot and catching (requriement)

- null repoart


- IIF (if and only if)
