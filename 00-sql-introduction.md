---
layout: lesson
root: .
title: Databases using SQL
minutes: 60
---

Setup
-----

_Note: this should have been done by participants before the start of the workshop._

1. Install Firefox
2. Install the SQLite Manager add on: **Menu (the three horizontal lines near the
top right corner of Firefox) -> Add-ons -> Search -> SQLite
Manager -> Install -> Restart now**
4. Add SQLite Manager to the menu: **Menu -> Customize, then drag the SQLite
   Manager icon to one of the empty menu squares on the right, Exit Customize**
5. Open SQLite Manager: **Menu -> SQLite Manager**


Relational databases
--------------------

* Relational databases store data in tables with fields (columns) and records
  (rows)
* Data in tables has types, and all values in a field have
  the same type
* Queries let us look up data or make calculations based on columns


Why use relational databases
----------------------------

* Data separate from analysis.
  * No risk of accidentally changing data when analyzing it
  * If we change the data we can just rerun the query
* Fast for large amounts of data
* Improve quality control of data entry (type constraints and use of forms in
  Access, Filemaker, etc.)
* The concepts of relational database querying are core to understanding how to do similar things using programming languages such as R or Python.


Database Management Systems
---------------------------

There are a number of different database management systems for working with
relational data. We're going to use SQLite today, but basically everything we
teach you will apply to the other database systems as well (e.g., MySQL,
PostgreSQL, MS Access, Filemaker Pro). The only things that will differ are the
details of exactly how to import and export data and the details of data types.


Database Design
---------------

1. Every row-column combination contains a single *atomic* value, i.e., not
   containing parts we might want to work with separately.
2. One field per type of information
3. No redundant information
     * Split into separate tables with one table per class of information
	   * Needs an identifier in common between tables – shared column - to
       reconnect (foreign key).


Introduction to SQLite Manager
------------------------------

Let's all open the database we downloaded in SQLite Manager by clicking on the
open file icon.

You can see the tables in the database by looking at the left hand side of the
screen under Tables.

To see the contents of a table, click on that table and then click on the Browse
and search tab in the right hand section of the screen.

If we want to write a query, we click on the Execute SQL tab.


Dataset Description
-------------------

The data we will be using is a time-series for a small mammal community in
southern Arizona. This is part of a project studying the effects of rodents and
ants on the plant community that has been running for almost 40 years.  The
rodents are sampled on a series of 24 plots, with different experimental
manipulations controlling which rodents are allowed to access which plots.

This is a real dataset that has been used in over 100 publications. We've
simplified it just a little bit for the workshop, but you can download the
[full dataset](http://esapubs.org/archive/ecol/E090/118/) and work with it using
exactly the same tools we'll learn about today.


Import
------

1. Download the three CSV files from the [Portal Database](http://figshare.com/articles/Portal_Project_Teaching_Database/1314459)
1. Start a New Database **Database -> New Database**
2. Start the import **Database -> Import**
3. Select the file to import
4. Give the table a name that matches the file name (surveys, species, plots), or use the default
5. If the first row has column headings, check the appropriate box
6. Make sure the delimiter and quotation options are correct
7. Press **OK**
8. When asked if you want to modify the table, click **OK**
9. Set the data types for each field: choose TEXT for fields with text
   (`species_id`, `genus`, `sex`, etc.) and INT for fields with numbers (`day`,
   `month`, `year`, `weight`, etc.)

> ### Challenge
>
> Import the plots and species tables

You can also use this same approach to append new data to an existing table.




Adding data to existing tables
------------------------------

* Browse & Search -> Add
* Enter data into a csv file and append


Other database management systems
---------------------------------

* Access or Filemaker Pro
    * GUI
    * Forms w/QAQC
	* But not cross-platform
* MySQL/PostgreSQL
    * Multiple simultaneous users
	* More difficult to setup and maintain

Previous: [Index](index.html) Next: [Basic queries.](01-sql-basic-queries.html)
