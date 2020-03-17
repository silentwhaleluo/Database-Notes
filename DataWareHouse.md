[TOC]
# Data Warehouse
- Characteristics of a DWH
	1. Subject Oriented
	2. Time variant
	3. Static
	4. Integrated

## Fact-Dimension design
- Benifits of Fact-Dimension
1. In most of the cases, data insertion mostly in fact. Seperate Fact and dimension can focused on few fact tables
2. Dimension usually grow slow, so have less index fragmentation problem. We can maintain index accordingly
3. Fact grow exponentially, so we designing indexes and partitioning and partition maintenance for only fact is more easily
4. Data in facts is rarely updated when compared with dimensions
5. Easy to do ETL

- Components
	- Surrogate key  
	Replacement for primary keys, because we want to keep all histories (not update) so the PK in OLTP possible to have duplicates and thus cannot work ad the PK of in OLAP systems
	Surrogate Key is a auto-incremented number (IDENTITY PROPERTY) which acts as a PK.
	Auto incremental keys for only use in the data wharehouse for finding data
		- Why we need a Surrogate Key, when there is a BK which is PK in source?
			1. Because DWH stores historical data, there could be a chance of duplication in BK.
			2. If same PK values are used by different locations (stores) and we want to maintain that data in DWH there will be a chance of duplication in BK.
			3. In many organizations PK values are re-cycled after some time, due to this DWH data might have duplicate business keys pointing to different people or products or places.
			4. Data is analyzed by joining dims with facts, as the data retrieval is in huge amounts the joining columns should be INT values to make it faster. In many cases BK's in OLTP are alphanumeric values, to avoid issues with performance SK is used.
			5. As the SK is a primary key by default it will have a Unique CI, because the values are auto incremented there is very small chance of internal fragmentation.
	- Business key  
	Works like a foreign key related to the OLTP system (but not foreign key because we cannot set foreign key across database)  
	Business Key: It is a column or set columns that is used to reference the data in OLTP. Typically BK is a PK in OLTP. In many cases this will have duplicates in DWH.  


# OLTP and OLAP

- Why we need OLTP
	To handle realtime transactions with lot of DML
	OLTP Purpose of it to maintain data related to day to day business process.

- Why we need OLAP (source of true)
	OLAP Purpose of it is to maintain data for historical needs including compliance, analysis, business predictions.
	- **History**
		- Compliance (stick into rules)
	- Analytics
	- Reports 
	- Make decision

- Features
	1. Subject oriented
	2. Non volotailem/static
	3. Time variant
	4. Integrated
		- Data should have some standard (clean)

# Dimentions  
Dimension is a column in a dimension table, which describes properties of a place, person or item (product). Used to analyze data after joining with facts.
## Slowly Changing Dimension (SCD)
SCD stands for Slowly Changing Dimension. It is a type of dimension in DWH or DM. Most of the dimensions in DWh should fall into one of the 6 types of SCDs. They are Type 0, 1, 2, 3, 4, 6
- Type 0   
These dimensions never change. Examples are DOB, Blood Group, SSN etc. These change very rarely in case of some error data in the system. Ex: Someone's SSN is entered wrong into some system, it has to be updated to correct value.
- Type 1   
This type of dimension holds or stores only latest data. Common examples are Phone Number or Email, though depending on the industry and organization this can be changed. Common examples will be phone numbers, email IDs.
- Type 2   
These dimension type holds all the history. Common examples are Patient diagnosis history, employee salary or pay history, account address changes for banking customers etc. We usually add 2 date columns to implement type 2, EffectiveDate and EndDate for the current rows or values EndDate would be in the future (12/31/9999) or NULL. Some organizations also add another row with BIT data type to identify record status 1 for Current and 0 for expired/closed. Ex: Address, Health info such as weight, blood pressure, PH values of blood, ECG values, Job level, salary etc. (this could vary depending on the design and industry).	
- Type 3   
It holds/stores limited history. Common examples are last 3 addresses of employees. We usually use a separate column for each historical value we want to store. For example if we want to hold last 3 Salaries of an employee, we have 1 column for current salary, 1 for previous salary and another for previous to previous salary.
- Type 4   
With this type, we maintain current values in 1 table and history in another table. When a type 2 dimension grows very fast (RAPIDLY GROWING dimension type) it would cause other columns in the table to grow even though there are no changes in those columns, so to solve this problem current values of a Type 2 are stored in 1 table and historical data is stored in another table. Ex: Prices of products if they are changing frequently. 
- Type 6   
It is combination of 1, 2, 3. Usually there is no specific reason why people use it. In this case some history is maintained in columns like type 3 and some history is maintained in the form of rows like type 2.

## Conformed Dimension 
This is the type of dimension which connects to multiple fact tables and provides same info for both the facts. Ex: Employee info, Product info., customer dimension
	
## Degenerate Dimension 
Some dimensions does not have any related dimensions to make its own dimension table and it does not describe any specific fact and it will grow as big as fact, these dimensions are degenerate dimensions and they are stored in fact tables. Common Examples are receipt numbers, line numbers, invoice numbers, order numbers, claim numbers, service numbers, transaction numbers etc. 
		
## Role Playing 
Some dimensions are connected to a fact multiple times same key from dimension table plays different roles. Ex: Ship Date, Order Date, Delivery Date etc from a Date Dimension is a role playing dimension. Another common example would Employee dimension playing multiple roles in Sales like when a car sales deal is closed by a sales associate and his manager or house purchase deal is closed by employee and his boss etc.. E.g. DimDate


## Late Arriving Dimension/Early Arriving Fact 
This is not a type of a dimension it is the availability of fact and dimension data. If the fact information is arrived to DWH earlier than Dimension we call this as early arriving dimension.		

- How to handle this scenario
	1. Hold the fact record until Dimension record is available
	2. Use 1 "Unknown" or default Dimension record to match all late arriving facts
	3. Inferring the Dimension record
	4. Late Arriving Dimension and SCD Type 2 changes
	
## Inferred Dimension/Inferred Dimension Members 
As Dimension tables are parent tables they have to be loaded first before loading Fact tables. In some special scenarios fact data is received first before dimension data, this scenario is called Early Arriving Fact or Late Arriving Dimension. To handle loading process for this scenario, an inferred member is created in the dim table with a proper business key from source and a SK is generated for the row and rest of the column values are entered NULL values. This dimension is called Inferred dimension and row with the above values is called Inferred Member. Ex: Gift card scenario.
	
## Rapidly Changing Dimension 
This is more of a concept or label than a dimension itself, if a type 1 is changing very frequently it is considered as Rapidly Changing.

## Rapidly Growing dimension 
This depicts the nature of a Type 2 dimension, if the values are frequently changing for a Type 2 to maintain the history of the data new rows are added because of it the dimension grows fast and huge. Usually this is also called as monster dimension. To solve issues with this organizations change this to a Type 4 dimension.
	
## Junk Dimensions 
Some dimensions which are some flags or other sort of identifiers which are not used in any analysis are put together into a dimension table these dimensions. In some cases they could be in DWH because of regulatory or compliance needs. E.g. Shipping Number
	
## Wide Dimension, Narrow Dimension???	

# Facts
## Fact Types
### Additive 
These are the types of facts that can be summarized or summed up across all the dimensions that are connected to the fact table. Ex: Fact with Sales Amount, Quantity connected to Date Dimension, Location Dimension, Product Dimension. Quantity and Sales Amount can be summed across all the three dimensions.
	
### Semi Additive 
There are the types of facts that can be summarized across some dimensions but not all. Ex: Fact table with Daily Account balances connected to Date Dimension and Customer Dimension. Daily balances can be summed up across all customers for a given day but for a given customer or all customer we can not sum up across all days to get meaningful data.
	
### Non Additive 
These measures cannot be summed up any dimensions, usually percentages like APRs, APYs in banks or max and min limits for heart rates or BP are some examples fall in this category. Eg. ratio, percentage
	
### Fact Less Fact 
It is a conjunction table between 2 or more dimension tables. Usually this table helps in auditing needs, though there are no facts in the table still we can analyze data. Ex: Date Dimension, Crime Type Dimension, Person Dimension, Location Dimension are connected to a fact less fact table (Fact Crime), though there are no other facts in the fact still it can be used to analyze number of crimes happened in a given location on a given date by a given person.
	
### Cumulative/Transactional 
These are fact tables where the data can summed up or the details are transaction granularity level.
### Snapshot 
If the fact table rolled up to a day or week and holds that data as summarized info it is called Snapshot. Ex: Daily Balances fact table, Monthly Statement fact table etc.  













Fact/Measure is a numerical info/data that is defined by a dimension.
	Numerical data for analysis
Set of related dims (attributes) 

- Benefits of Fact-Dimension design in DWH
	1. In most of the cases data insertion happens more in facts than dimensions this way the insertion operation can be focused on few tables than more.
	2. As dimensions change rarely compared to facts index maintenance is less on dimensions, so we can schedule this process according to the table type
	3. Data in Facts grow exponentially (very high) compared to dims, so when designing indexes and partitioning and partition maintenance focus will be easy or only on facts.
	4. Data in facts is rarely updated (UPDATE statement) when compared with dimensions.
	5. It also helps (easier) in data loading (ETL) process.

## Dimension
Description (dummy variables) data to analysis

### Types of advanced dimensions
(Type 1, 2, 3 is mostly based on business requirment)
- Type 
Should not change but may be change in some cases (error input).EX. SSN, DOB, Gender, Blood Group, Eye color
- Type 1
Data the dose change, but only the most recent value is shown. Update data and do not keep old record EX. Phone, Email refrence
- Type 2
Data changes, and now all changes or modifications are recorded and shown EX. Address (base on requirment), previous employers
	- Record the start date and end date, for the most recent record, put the end date NULL or a very large date
	- Use flag
- Type 3
Data changes, keep recorded but only certain number of records (only one record with certain number of columns) . EX. 3 most recent patient weight,
- Type 4
Use two tables, one table hold history records, the other hold only one most recent values. History have start date and end date hold all history records (like type 1 + type 3)
- Type 9
Combination of features from type 1, 2, and 3
- Junk dimension
Data from OLTP but is useless
- Conformed dimension
Different facts use same dimension as same means. EX. F(Sales) -- D(product name) -- F(inventory), product name is a conformed dimension

- Degenerate
Useful but do not have any connection to  other

- Role playing dimension
F(Sales) -- D(EmpID) -- F(Manager)

- Rapidly Changing
- Rapidly Growing

- Late arriving dimension
EX. for gift card, the user information may come late (register later) or never get those information

- Inferred dimension/ Inferred dimension member
Way we handle late arriving dimension, we have some non Null columns and leave others information as NULL

- Junk dimension
EX. useless info


- Wide Dimension, Narrow dimension

## Fact

- Types
	- Keys + numbers facts

	- Factless facts
		- Fact with only keys connect a fact and a dimension
		Fact with only keys act as bridge table to present M:N relationship between **Fact** and **Dimension** 

		- Audit
		Facts will only keys connected different dimensions
		Only key may also meaningful, EX. Attendence, appointment

- Additive
EX. For F(sales, keys, Qty, UnitPrice, Linetoal, OrderTotal) - D(Date) - D(Cust) - D(Prodcut), LineTotal of F(sales) is additive (when join to all dimension, the sum(LineTotal) is meaningful), other when do sum, may not meaningful, eg( SUM(qty) for cust, for date, for product)
- semi-additive
EX. samilar with Additive, sum(Qty) group by product is meaningful, but for others not. qty is semi-additive fact

- cumulative
LineTotal can be cumulative

- Snapshot
OrderTotal for F(sales) All snapshot cannot be sum up ( additive or semi-additive)



Candidate Name:
Batch:
1.	What does OLTP stand for? Why is it used?
OLTP is short for online transaction processing. We use OLTP for 1. real-time transaction 2. Multiple DML

2.	What does OLAP stand for? Why is it used?  
OLAP is short for online analytical processing. We use OLAP for 1. maintain history 2. reporting 3. analysis

3.	What are important characteristics of DWH? Provide an example for each characteristics?

4.	What are the differences between OLAP and OLTP?
|OLAP|OLTP|
|----|----|
|more DML|less DML more retriveal
5.	Does running or using an OLAP require different software or coding than that of an OLTP?
6.	Why have an OLAP? What is the main focus of a Data Warehouse?
7.	What is Data Mining? How does it relate to that of a Data Warehouse?
8.	Does all information from a Database get imported into a Data Warehouse? Why?
9.	What is ETL? Why is it important for a Data Warehouse?
10.	Do all companies have to keep all their records in a Data Warehouse? Give an example of a company that might delete some data, and a company that might keep all its data.
11.	What is an Archive Data Warehouse? Why might it be used? Is it mandatory? 
12.	Is a Data Warehouse Normalized? Or De-Normalized? Why do you think it has this design?
13.	Using syntax, complete the following questions with the AdventureworksDW

	Get the Full names of all persons who live in Australia
	Get the Full names of all persons who live outside the United States and are married
List the product name, category name, and full name of all customers who have spent at least $4,000 in a single order
List the full names of all customers, their city, their country, and how much they've spent total in AdventureworksDW



## Dimensional Modeling

- Steps
	1. Choose Business
	Porporse, data sources (clean, normalization, standardize)
	2. Choose Granularity (for fact)
	Most commonly Per transaction (a record into our table)
	3. Choose dimensions
	4. Choose facts


difference between OLTP and OLAP
1. use for transaction or 
2. normalize
3. history
4. index

difference between dimensions and facts
1. SK, dimensions / fact FK, fact columns

data mart subset of data warehouse

Advantages of DW
1. hold history
2. index fast
3. data standardize, format, misspelling/ OLTP may have some dirty data

components of DWH
1. data sources
2. staging area (where ETL real work area)
	- pre-staging (copy of sources)
	- staging
3. data presentation area or DWH/data mart
4. data access tools (SSAS, SSRS)

ETL is done only in Data staging area


1. stanterdize
2. misspelling
3. Null
4. denormalization table

data validation vs data verificatio



ETL documents
1. excel file

How do design a data warehouse/data mart
1. Requeirments, what this data warehouse for
2. data sources
3. Facts and dimensions
4. guralirity
5. schema
6. tools to design data warehouse (ErWin)

# Methedology

|Inmon|	Kimbal|
|-----|-------|
|1. Uses Top Down approach|1. Uses Bottom Up approach|
|2. Uses one source of truth (centralized DWH)| 2. Uses/recommends multiple Data Marts in network as DWH|
|3. Recommends Normalized DWH and denormalized (Star schema) Data Marts| 3. Recommends denormalized (Star Schema) Data Marts|
|4. Slower ROI (Return on Investment) or more expensive.| 4. Faster ROI or cheaper process compared with Inmon methodology.|
|5. Slower in delivering the final product| 5. Faster in delivering the final product.|
|6. Recommends normalized/snow flake schema| 6. Recommends star schema|
|7. Querying for complete data is easy as it proposes centralized DWH	| 7. Querying complete data is complex and in some cases inefficient as data is spread across multiple data marts|

# Data Warehouse vs Data Mart

|Data Warehouse	|Data Mart|
|---------------|---------|
|1. Centralized repository for all organizational data.| 1. Departmental data.|
|2. One source of truth.| 2. Data is not complete in one repository makes it incomplete data.|
|3. Usually DWH size is bigger compared to a DM for the same organization| 3. Usually size is smaller compared to a DWH.|

# Scheme
|star	|snowflake|
|----	|---------|
|1. all dimensions are directly connected to fact table| 1. Not all dimensions are directly connected to Fact table, some dimensions are indirectly (through other dims) connected to Fact Tables|
|2. it is better in performance compared to snowflake in most of the cases.| 2. Little slower in performance.|
|3. based on oltp structure not all oltps can be changed into star schemas| 3. In most of the cases OLTPs can be converted to Snowflake (even if it is complex structure)|
|4. data redundancy is more.| 4. Data redundancy is less compared to Star|
|5. no parent dimensions involved in design| 5. Parent and child dimensions are involved in design.|
|6. ETL loading process is fairly easy.	| 6. ETL loading process is complex.|


