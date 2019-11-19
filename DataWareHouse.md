# OLTP and OLAP

## OLTP
- Why we need OLTP
	To handle realtime transactions with lot of DML
- Features 
	- Surrogate key
	Replacement for primary keys, because we want to keep all histories (not update) so the PK in OLTP possible to have duplicates and thus cannot work ad the PK of in OLAP systems
		- Auto incremental keys for only use in the data wharehouse for finding data
	- Business key
		Works like a foreign key related to the OLTP system (but not foreign key because we cannot set foreign key across database)
- Why we need OLAP (source of true)
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

# Dimentions and facts
- Dimentions  
- Facts
	Numerical data for analysis
Set of related dims (attributes) 

## Dimension
Description (dummy variables) data to analysis

- Types of advanced dimensions
(Type 1, 2, 3 is mostly based on business requirment
	- Type 0
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

