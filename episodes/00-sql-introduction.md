---
title: "Databases using SQL"
teaching: 60
exercises: 5
questions:
- "What is a relational database and why should I use it?"
- "What is SQL?"
objectives:
- "Describe why relational databases are useful."
- "Create and populate a database from a text file."
- "Define SQLite data types."
- "Select, group, add to, and analyze subsets of data."
- "Combine data across multiple tables."
keypoints:
- "SQL allows us to select and group subsets of data, do math and other calculations, and combine data."
- "A relational database is made up of tables which are related to each other by shared keys."
- "Different database management systems (DBMS) use slightly different vocabulary, but they are all based on the same ideas."
---

## Setup

_Note: this should have been done by participants before the start of the workshop._

We use [SQLite Manager](https://addons.mozilla.org/en-us/firefox/addon/sqlite-manager/)
and the 
[Portal Project dataset](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459)
throughout this lesson. See [Setup](/sql-ecology-lesson/setup/) for
instructions on how to download the data, and also how to install and open
SQLite Manager.

# Motivation

To start, let's orient ourselves in our project workflow.  Previously, 
we used Excel and OpenRefine to go from messy, human created data 
to cleaned, computer-readable data.  Now we're going to move to the next piece 
of the data workflow, using the computer to read in our data, and then 
use it for analysis and visualization.  

## What is SQL?

SQL stands for Structured Query Language. SQL allows us to interact with relational databases through queries. 
These queries can allow you to perform a number of actions such as: insert, update and delete information in a database.


## Dataset Description

The data we will be using is a time-series for a small mammal community in
southern Arizona. This is part of a project studying the effects of rodents and
ants on the plant community that has been running for almost 40 years.  The
rodents are sampled on a series of 24 plots, with different experimental
manipulations controlling which rodents are allowed to access which plots.

This is a real dataset that has been used in over 100 publications. We've
simplified it just a little bit for the workshop, but you can download the
[full dataset](http://esapubs.org/archive/ecol/E090/118/) and work with it using
exactly the same tools we'll learn about today.

## Questions

First, let's download and look at some of the cleaned spreadsheets 
from the 
[Portal Project dataset](https://figshare.com/articles/Portal_Project_Teaching_Database/1314459).  
We'll need the following three files: 

* `surveys.csv`
* `species.csv`
* `plots.csv`

> ## Challenge
>
> Open each of these csv files and explore them. 
> What information is contained in each file?  Specifically, if I had 
> the following research questions: 
> 
> * How has the hindfoot length and weight of *Dipodomys* species changed over time?
> * What is the average weight of each species, per year?  
> * What information can I learn about *Dipodomys* species in the 2000s, over time?
> 
> What would I need to answer these questions?  Which files have the data I need? What 
> operations would I need to perform if I were doing these analyses by hand?  
{: .challenge}

## Goals

In order to answer the questions described above, we'll need to do the 
following basic data operations: 

* select subsets of the data (rows and columns)
* group subsets of data
* do math and other calculations
* combine data across spreadsheets

In addition, we don't want to do this manually!  Instead of searching 
for the right pieces of data ourselves, or clicking between spreadsheets, 
or manually sorting columns, we want to make the computer do the work.  

In particular, we want to use a tool where it's easy to repeat our analysis 
in case our data changes. We also want to do all this searching without 
actually modifying our source data.  

Putting our data into a relational database and using SQL will help us achieve these goals.  

> ## Definition: *Relational Database*
>
> A relational database stores data in *relations* made up of *records* with *fields*.
> The relations are usually represented as *tables*;
> each record is usually shown as a row, and the fields as columns.
> In most cases, each record will have a unique identifier, called a *key*,
> which is stored as one of its fields.
> Records may also contain keys that refer to records in other tables,
> which enables us to combine information from two or more sources.
{: .callout}

# Databases

## Why use relational databases

Using a relational database serves several purposes.

* It keeps your data separate from your analysis.
    * This means there's no risk of accidentally changing data when you analyze it.
    * If we get new data we can just rerun the query.
* It's fast, even for large amounts of data.
* It improves quality control of data entry (type constraints and use of forms in MS Access, Filemaker, Oracle Application Express etc.)
* The concepts of relational database querying are core to understanding how to do similar things using programming languages such as R or Python.

## Database Management Systems

There are a number of different database management systems for working with
relational data. We're going to use SQLite today, but basically everything we
teach you will apply to the other database systems as well (e.g. MySQL,
PostgreSQL, MS Access, MS SQL Server, Oracle Database and Filemaker Pro). The 
only things that will differ are the details of exactly how to import and 
export data and the [details of data types](#datatypediffs).

## Relational databases

Let's look at a pre-existing database, the `portal_mammals.sqlite`
file from the Portal Project dataset that we downloaded during
[Setup](/sql-ecology-lesson/setup/). Clicking on the "open file" icon, then
find that file and clicking on it will open the database.

You can see the tables in the database by looking at the left hand side of the
screen under Tables, where each table corresponds to one of the `csv` files 
we were exploring earlier.  To see the contents of any table, click on it, and
then click the “Browse and Search” tab in the right panel.  This will 
give us a view that we're used to - just a copy of the table.  Hopefully this 
helps to show that a database is, in some sense, just a collection of tables, 
where there's some value in the tables that allows them to be connected to each 
other (the "related" part of "relational database").  

The leftmost tab, "Structure", provides some metadata about each table.  It 
describes the columns, often called *fields*. (The rows of a database table 
are called *records*.)  If you scroll down in the Structure view, you'll 
see a list of fields, their labels, and their data *type*.  Each field contains 
one variety or type of data, often numbers or text.  You can see in the 
`surveys` table that most fields contain numbers (integers) while the `species` 
table is nearly all text.  

The "Execute SQL" tab is blank now - this is where we'll be typing our queries 
to retrieve information from the database tables.  

To summarize: 

* Relational databases store data in tables with fields (columns) and records
  (rows)
* Data in tables has types, and all values in a field have
  the same type ([list of data types](#datatypes))
* Queries let us look up data or make calculations based on columns

## Database Design

* Every row-column combination contains a single *atomic* value, i.e., not
   containing parts we might want to work with separately.
* One field per type of information
* No redundant information
    * Split into separate tables with one table per class of information
    * Needs an identifier in common between tables – shared column - to
       reconnect (known as a *foreign key*).

## Import

Before we get started with writing our own queries, we'll create our own 
database.  We'll be creating this database from the three `csv` files 
we downloaded earlier.  Close the currently open database and then 
follow these instructions: 

1. Start a New Database 
    - **Database -> New Database**
    - Give a name **Ok -> Open**. Creates the database in the opened folder
2. Start the import **Database -> Import**
3. Select the `surveys.csv` file to import
4. Give the table a name that matches the file name (`surveys`), or use the default
5. If the first row has column headings, check the appropriate box
6. Make sure the delimiter and quotation options are appropriate for the CSV files.  Ensure 'Ignore trailing Separator/Delimiter' is left *unchecked*.
7. Press **OK**
8. When asked if you want to modify the table, click **OK**
9. Set the data types for each field using the suggestions in the table below (this includes fields from `plots` and `species` tables also):

| Field             | Data Type      | Motivation                                                                       | Table(s)          |
|-------------------|:---------------|----------------------------------------------------------------------------------|-------------------|
| day               | INTEGER        | Having data as numeric allows for meaningful arithmetic and comparisons          | surveys           |
| genus             | TEXT           | Field contains text data                                                 	| species           |
| hindfoot_length   | REAL           | Field contains measured numeric data                                             | surveys           |
| month             | INTEGER        | Having data as numeric allows for meaningful arithmetic and comparisons          | surveys           |
| plot_id           | INTEGER        | Field contains numeric data	    						| plots, surveys    |
| plot_type         | TEXT           | Field contains text data                                                 	| plots             |
| record_id         | INTEGER        | Field contains numeric data 							| surveys           |
| sex               | TEXT           | Field contains text data                                                 	| surveys           |
| species_id        | TEXT           | Field contains text data								| species, surveys  |
| species           | TEXT           | Field contains text data                                                 	| species           |
| taxa              | TEXT           | Field contains text data                                                 	| species           |
| weight            | REAL           | Field contains measured numerical data                                           | surveys           |
| year              | INTEGER        | Allows for meaningful arithmetic and comparisons                                 | surveys           |


Finally, click **OK** one more time to confirm the operation.


> ## Challenge
>
> - Import the `plots` and `species` tables
{: .challenge}

You can also use this same approach to append new data to an existing table.

## Adding data to existing tables

1. "Browse and Search" tab -> Add
1. Enter data into a csv file and append


## <a name="datatypes"></a> Data types

| Data type                          | Description                                                                                              |
|------------------------------------|:---------------------------------------------------------------------------------------------------------|
| CHARACTER(n)                       | Character string. Fixed-length n                                                                         |
| VARCHAR(n) or CHARACTER VARYING(n) | Character string. Variable length. Maximum length n                                                      |
| BINARY(n)                          | Binary string. Fixed-length n                                                                            |
| BOOLEAN                            | Stores TRUE or FALSE values                                                                              |
| VARBINARY(n) or BINARY VARYING(n)  | Binary string. Variable length. Maximum length n                                                         |
| INTEGER(p)                         | Integer numerical (no decimal).                                                                          |
| SMALLINT                           | Integer numerical (no decimal).                                                                          |
| INTEGER                            | Integer numerical (no decimal).                                                                          |
| BIGINT                             | Integer numerical (no decimal).                                                                          |
| DECIMAL(p,s)                       | Exact numerical, precision p, scale s.                                                                   |
| NUMERIC(p,s)                       | Exact numerical, precision p, scale s. (Same as DECIMAL)                                                 |
| FLOAT(p)                           | Approximate numerical, mantissa precision p. A floating number in base 10 exponential notation.          |
| REAL                               | Approximate numerical                                                                                    |
| FLOAT                              | Approximate numerical                                                                                    |
| DOUBLE PRECISION                   | Approximate numerical                                                                                    |
| DATE                               | Stores year, month, and day values                                                                       |
| TIME                               | Stores hour, minute, and second values                                                                   |
| TIMESTAMP                          | Stores year, month, day, hour, minute, and second values                                                 |
| INTERVAL                           | Composed of a number of integer fields, representing a period of time, depending on the type of interval |
| ARRAY                              | A set-length and ordered collection of elements                                                          |
| MULTISET                           | A variable-length and unordered collection of elements                                                   |
| XML                                | Stores XML data                                                                                          |


## <a name="datatypediffs"></a> SQL Data Type Quick Reference

Different databases offer different choices for the data type definition.

The following table shows some of the common names of data types between the various database platforms:

| Data type                                               | Access                    | SQLServer            | Oracle             | MySQL          | PostgreSQL    |
|:--------------------------------------------------------|:--------------------------|:---------------------|:-------------------|:---------------|:--------------|
| boolean                                                 | Yes/No                    | Bit                  | Byte               | N/A            | Boolean       |
| integer                                                 | Number (integer)          | Int                  | Number             | Int / Integer  | Int / Integer |
| float                                                   | Number (single)           | Float / Real         | Number             | Float          | Numeric       |
| currency                                                | Currency                  | Money                | N/A                | N/A            | Money         |
| string (fixed)                                          | N/A                       | Char                 | Char               | Char           | Char          |
| string (variable)                                       | Text (<256) / Memo (65k+) | Varchar              | Varchar2 | Varchar        | Varchar       |
| binary object	OLE Object Memo	Binary (fixed up to 8K)   | Varbinary (<8K)           | Image (<2GB)	Long | Raw	Blob          | Text	Binary | Varbinary     |
