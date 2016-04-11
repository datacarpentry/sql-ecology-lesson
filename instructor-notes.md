## Learning objectives
* Understand concept of relational database
* Be able to perform simple queries in SQL from a single table
* Understand how to filter and sort results from a query
* Use aggregate functions to combine data
* Perform queries across tables

## Data
As for all of the [Data Carpentry ecology lessons](https://github.com/datacarpentry?utf8=%E2%9C%93&query=ecology), this
lesson uses the Portal Project Teaching Database. The data is available at http://dx.doi.org/10.6084/m9.figshare.1314459 and the download includes a
SQLite database file (portal_mammals.sqlite) as well as three .csv files
(species.csv, plots.csv, surveys.csv) that can be imported into SQLite.

**Note** that the figshare download is an archive (.zip) file that rudely explodes all of the files into your current directory.

## Instructor's setup notes

By default SQLite Manager opens in a separate window and it is not possible to
zoom in to enlarge the font
so that it is more readable, especially for students in the back rows.

The way to fix this is to:

1. Open the SQLite Manager
2. Click on the ![Options](img/options_button.png)*Options* button .
3. Chose *Start SQLite Manager: in a new tab*.

You can then use **Ctrl - +** to zoom just like any other web page.

## Lesson outline

### 00-sql-introduction
* Introduce relational databases, database management systems, the SQLite
Firefox plugin, and the Portal dataset
* Import .csv files into sqlite
* Structuring data for database import
* Discuss the different SQL data types

**Tips**

* **Changing data type**: There is a step on import where you have to select the data type for the field. Some people select the wrong type or nothing at all and wonder what they can do to change it. If you right click or Control click on the column name in the Structure tab, then there's an Edit Column option and you can type in a new data type. When you go to save it, it does warn you that this is dangerous behavior, so people are being warned, but it might be nice to show this option and also to say why it's not generally a good idea to change data types.
* **Importing data**: Note how cleanly the csv files import into SQL. If you have also taught the spreadsheet lesson, it would be a good idea to compare the format of the csv files with the messy spreadsheet and ask "Remember that messy spreadsheet? What would have happened if we tried to load that in to SQL?"

### 00-supplement-database-design.md
(optional)
The first lesson includes a brief introduction to data design and choosing database systems. This material expands on the database design in the first section.

### 01-sql-basic-queries
* Write basic queries using SELECT and FROM
* Filter results using DISTINCT, WHERE
* Change how results are displayed using ORDER BY and by doing calculations on results

### 02-sql-aggregation
* Combine results using COUNT and GROUP BY
* Filtering based on aggregation with HAVING
* Saving queries using VIEW

### 03-sql-joins-aliases
* combining data from two tables using JOIN, ON, USING
* depending on time, could introduce different types of JOINs
* using aliases with AS to simplify queries
