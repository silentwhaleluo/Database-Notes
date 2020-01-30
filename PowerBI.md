# Power BI
## Overview
### Definition
Power BI is a collection of software services, apps, and connectors that work together to connect to multiple sources of data, transform, visualize data and interactive insights.

### Parts
Power bi desktop (**.pbix**) , online power BI service(SaaS software as service), and mobile power BI apps for Android and IOS devices

### Power BI work flow
1. Connecting to data sources
2. Build a reports in Power BI Desktop
3. Publish reports from **Power BI Desktop** to **Power BI Service** 
4. Share reports to consumers and business users

### Sources
1. File  
Excel, CSV, XML, Text, JSON, Folder
2. Database  
SQL Server database, Access Database, SQL Server analysis, Services Database, Oracle Database, IBM DB2 Database, MySQL Database, PostgreSQL Database, Sybase Database, Teradata Database
3. Azure  
Microsoft Azure SQL database, Marketplace, HDInsight, Blob Storage, Table Storage
4. (Online Services) SaaS  
appFigures, QuickBooks Online, zendesk, Github, Twilio, SweetIQ
5. Other  
Web, SharePoint List, ODate Feed, Hadoop File(HDFS), Active Directory, Microsoft Exchange, Dynamics CRM Online, Facebook, Google Analytics, SAP Business Object BI Universe, Salesforce Objects, Salesforce reports, ODBC Query, ODBC Tables

### Modes to extract data from sources
1. From SQL Server
	- Import  
	The selected tables and columns are imported into Power BI Desktop. Use **refresh** data to import the full dataset again
	**1 GB dataset limitation** 
	- Direct Query  
	No data is imported or copied into Power BI Desktop. For relational sources, the selected tables and columns appear in the **Fields list**. Fore multi-dimensional sources like SAP Business Warehouse, the dimensions and measures of the selected cube appear in the Fields list. When create or interact with a visualization, Power BI Desktop queries the underlying data source, so always viewing current data
		- Benefits
			1. Build visualizations over **large dataset** which cannot import all the data with pre-aggregation
			2. Underlying data changes can reuqire a refresh of data
			3. Do not have 1 GB dataset limitation
		- Limitations of DirectQuery
			1. All tables must come from a single database, unless use **[composite models](#compositemodels)** 
			2. If the query editor query is overly complex, an error occurs
			3. Time intelligence capabilites are unavailable in DirectQuery
			4. Limitations are places on DAX expressions allowed in measures to ensure that queries sent to the underlying data source have acceptable performance
			5. **one-million-row** limit for **returning** data when using DirectQuery
		- Considerations
			1. **Performance**. All DirectQuery are sent to the source database so the required visual refresh time depends on how long that back-end source takes to respond with the results from the query (recommended < 5s, if it will take a few minutes will receive an error
			2. **load** on the source database; **row level security (RLS)** will have a significant impact
			3. **security** all users can access data source using the credentials after publication to the power BI service. All users see the same data, regardless of any security rules defined the backend source???
			4. Some features are not supported in DirectQuery such as Quick Insights
2. Analysis Services(Multi-Dimensional/Tabular) from Azure Analysis Services/ SSAS(with a gateway)
	- Import
	- Connect Live ???  
	
		- Limitations
			- Cannot be use with any other source together
	Cannot create new column, new measure or new quick measure
			- cannot manage relationship???
			- Do not support row-level security
			- Cannot create new column, new measure, nor new quick measure
			- Report Pane is available in live connection

<a name='compositemodels'></a>
## Composite models (support automatically)
- composite models: Allows a report to have multiple data connections, including DirectQuery connections or import, in any combinations. 
- Many-to-many relationship:
- storage mode

- Benifits
	- by "DirectQuery", composite model allow combines data from one or more DirectQuery sources, and combines from DirectQuery sources and import data

- Limitations
	- Incremental refresh is supported for composite models connecting to SQL, Oracle, and Teradata data sources only
	- The following Live connect multi-dimensional sources can't be used with composite models
		- SAP HANA
		- SAP Business Warehouse
		- SQL Server Analysis Services
		- Power BI datasets
		- Azure Analysis Services

## Comparison of Import, DirectQuery, Live connections
|Category|Import data|DirectQuery|Live Connection|
|---|---|---|---|
|Data model size limitation|1. Tied to license; 2. Power BI Pro; 3. 1 GB dataset; 4 Power BI premium; 5 Capacity base|Limited only by underlying data source hardware|1. SQL server analysis services; 2. Limited only by underlying data source hardware; 3. power BI service; 4. Power BI service same data set size limits as import data|
|Number of data sources|Unlimited|Only one|Only one|
|Data refresh|1. Tied to license; 2. Power BI Pro: up to 8 times a day at 30 min intervals; 3. Power BI premium: up to 48 times a day at 1 min intervals|Report shows the latest data available in source|Report shows the latest data available in the source|
|Performance|Best|Slowest|Best|
|Data transformation|Fully featured|Limited to what can be translated to data source language|None|
|Data modeling|Fully featured|Highly restricted|1. SSAS Tabular and Power BI Service; 2. measures can be created without restrictions|
|Security|Row-level security can be applied based on current user login|1. Cannot use row-level security defined at data source; 2. Row-level security must be done in Power BI Desktop|Can leverage data source security rules based on current user's login|

### Types of relations
1. Active (solid line)  
Only 1 active relationship
2. Inactive (dotted line)  
	- Multiple inactive relationship between tables
	- Usually happens to role playing dimensions


### Cardinality
1. 1:1
2. 1:M
3. M:1
4. M:M

### Cross filter
1. single  
can filter from 1 side to other
2. both
can filter from both sides

## DAX (data analysis expressions)
DAX is a collection of functions, operators, and constants that can be used in a formula, or expression, to calculated and return one or more values. Create new informatin from data already in your model
Calculations, more in measures and calculated columns
Formula language for power pivot, power BI desktop, and Tabular modeling in SSAS
	- Properties
		- Can be nested up to 64 functions
### Syntax
```DAX
ColumnName = Function()
ColumnName = CALCULATE(expression to evaluated, filter)
```

#### Date and Time
#### Time Intelligence
(time intelligence need to mark table as date)
- TotalYTD  
TOTALYTD(<expression>, <dates>[,<filter>][,<year_end_date>]]
evaluate the year-to-date value of the expression in the current context
- TotalQTD  
dates in the quarter to date
#### Information
#### Logical
#### Mathematical
#### Statistical
#### Text
#### Parent/Child
#### Other


### Commonly used DAX functions
- Data and Time functions
- Filter functions
- Information functions
	- related
- Logical functions
- Mathematical and Trigonometric functions
- Statistical functions
- Text functions
	- concatenate
	- lower
	- upper
	- value
- Time intelligence functions
	- Date
	- calendar
	- year
	- month
	- day
	- now

### Hierarchies
Create hierarchies will implement drill down capability on visuals

### New column, New measures, New Quick measure
- New column
Create new calculated column as part of table and values are stored in PowerBI cache
- New measure
Do not stored in tables but generated over run time
- New quick measure
has some build-in formulas

### Calculated tables
1. Copy of existing table or create new calculated tables
- UNION
- CROSSJON
- NATURALINNERJOIN
- NATURALLEFTOUTERJOIN
- INTERSECT

2. faster in performance because it saved in memory
3. make reports run slow because they take memory space

## Different visuals
1. Area charts
2. Bar and column charts
3. combo charts
4. Doughnut charts
5. Funnel charts
	- Stages and items flow sequentially from one stage between stages
	- Examples: sales process starts with leads and ends with purchase fulfillment
6. Gauge  
current status in the context of goal
7. KPIs
progress toward a measurable goal
8. Line chart
9. Maps: basic maps
10. ArcGIS mpas
11. Filled maps
12. Pie charts
13. Scatter and bubble charts
14. Matrix
15. Tables
16. Tree maps  
colored rectangles with size representing value (can be hierarcal)
17. Waterfall charts  
running total as values are added or subtracted
18. Slicers  
- drop down, list or range values, 
- setting property "single select to OFF"

### Formatting visuals
- title
- background color
- conditional formatting
- field formatting
- data colors
- borders
- length, width, x-position, y-position

### Switching theme
can import JSON file to customize data colors on visuals

## Power Query ???
- find and connect data across
- merge and shape
- create custom view 
- perform data cleansing operation
- transform data


## Bookmarks
capture the currently configured view of a report page
- current page
- filters
- slicers
- sort order
- drill location
- visibility (using selection pane)
- the focus or spotlight modes of any visible object

## Filters
- Visual level filters
- page level filters
- report level filters
- drill through filters

## Role Management (RLS)
- Properties
	- Row level security (RLS) can be used to restrict data access for given user
	- configure for **imported** 
	- use for DirectQuery
	- on-premises model

- Steps to create roles
	1. "manage roles" on "Modeling"
	2. "create" manage roles
	3. Write "Table filter DAX expression"
	4. check for DAX syntax and click "Save"
	5. view reports as created role and test the DAX filters

# Optimize data model
1. Improve query performance in SSMS such as execution plan, use store procedure(sniffing), index
1. remove unused tables and columns
2. avoid distinct counts with high cardinality
3. avoid unnecessary precision and high cardinality
4. use integers instead of strings if possible
5. DAX functions which need to test every row in a table
6. when connecting data sources via DirectQuery, indexing columns that are commonly filtered or sliced again???
7. use filters to limit report visuals to diaplay only what is needed
8. limit the number of visuals on page

# Publishing report
"Home" -\> "Publish"

## workspace
workspace are some kind of destination folders which are chosen while publishing reports to powerBI services
	- Dashboards
	- Report
	- Workbooks
	- Dataset

### components
- Dashboards  
Choose visuals on report to pin on the dashboard
- report
- workbooks  
Workbooks are a special type of dataset. When use Get data with "Excel file", and chose "Connect" workbook will apprear in power BI as a Excel Online
- dataset

### Sharing dashboards/reports
- Content packs  
	- Package up dashboards, reports, Excel workbooks, and datasets and publish them to you team as an organizational content pack; include all PowerBI features includng interactive data exploration, new visuals, Q&A, integration with other data sources, data refresh and more; we can create multiple content packs per workspace  
	- Steps: "Settngs" -\> "create content pack" -\> "Specific group, or entire organization" -\> "enter email address" -\> "Description" -\> "select dashboard, respective, report, and dataset"

- Apps
	- With Power BI apps, we can easily deploy a collection of purpose-built dashboards and reports to a large number of business users and empower them to make data-drivent decisions.
	- There is a 1:1 relationship between Apps and workspace
- Share in email
- Embed codes
- publish to web  
We can easily embed interactive Power BI visualizations online, such as in blog posts, websites, through emails or social media, on any device
	
- publish to sharepoint
	- get the URL of report and "Embed in SharePoint Online", copy URL from dialog

### Subscriptions
Options are daily or weekly


### Refresh
Daily or weekly  
DirectQuery every 15 min; options are daily or weekly; daily or weekly
Live connect every 15 min; users is sent an email if the report refreshed, but no more than once per day

| |DirectQuery|Live Connect|Scheduled refresh(Import)|Excel file in OneDrive/SharePoint Online|
|--|-----------|------------|---------------------|------------------|
|Report/dashbord| Every 15 mins|Power BI checks every 15 mins, if dataset changed, the report is refreshed|Daily can be up to 8 times a day/ Weekly|Once every hour|
|Subscription over email|Daily/Weekly|No options; Users is sent an email if the report refreshed but no more than once per day|If the refresh schedule is daily, options is daily or weekly; if refresh schedule is weekly only option is weekly|When update, no more than once per day|

### Data alerts
Can set on tiles pinned from reports visuals for and only for gauges, KPIs and cards

### Dataset Insights
- generate interesting interactive visualizations based on data

### Usage metrics
usage metrics helps to understand the impact of dashboard and report
- view per day
- total view
- mobile or web
- unique viewers per day
- total views rank

## Getways
A gateway is software that facilitates access to data 
- On-premises data gateway (personal mode), allows one user to connect to sources and cannot shared with others (can only be used with Power BI)
- On-premises data gateway: multiple users to connect to multiple on-premises data sources and can be used by power BI, PowerApps, Flow, and Azure logic apps, all with single gateway installation

-entriprise ??
## Data refresh
- package refresh
- Model/data refresh
- tile refreshapps
updates the cache for tile visuals, once data changes, refresh every 15 mins
- visual container refresh


## Configure schedule refresh
daily, weekly

## schedule cache refresh (for DirectQuery)
1. when user interact with visualization, queries are sent from power bi directly to data base updated is then returned and visualizations are updated
2. If no user interaction in a visualization, data is refreshed automatically approximately every hour. We can change that refresh frequency using the scheduled cache refresh option

## data classification
The data on dashboard can be classified on the sensitivity of data
"Dashboard" -\> "SETTINGS" -\> "Data classification" -\> "Admin Portal" -\> "Tenant settings" -\> "Data classification for dashboards and reports"
	- High
	- Medium
	- Low sensitivity

### Parameters


### Methods to create tiles in dashboard ???
	- in dashboard, use "Add tile"
	- in report, pinning a "live tiles



