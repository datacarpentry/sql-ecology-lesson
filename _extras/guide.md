---
layout: page
title: "Instructor Notes"
permalink: /guide/
---

## Learning objectives
* Understand concept of relational database
* Be able to perform simple queries in SQL from a single table
* Understand how to filter and sort results from a query
* Use aggregate functions to combine data
* Perform queries across tables

## Data
As for all of the [Data Carpentry ecology lessons](https://github.com/datacarpentry?utf8=%E2%9C%93&query=ecology), this
lesson uses the Portal Project Teaching Database. The data is available at <a href="https://doi.org/10.6084/m9.figshare.1314459">https://doi.org/10.6084/m9.figshare.1314459</a> and the download includes a
SQLite database file (portal_mammals.sqlite) as well as three .csv files
(species.csv, plots.csv, surveys.csv) that can be imported into SQLite.

**Note** that the figshare download is an archive (.zip) file that rudely explodes all of the files into your current directory.

## Motivation and Framing

See this slide deck as a sample intro for the lesson: 
[SQL Intro Deck](https://speakerdeck.com/christinalk/data-carpentry-sql-introduction)

Key points: 
* Want to query data, instead of editing directly
* Need a solution that is scalable and reproducible
* Introduce the typical ways of dealing with rectangular data (subsetting, 
split-apply-combine)

If you've written up a diagram of the data analysis pipeline (raw data -> 
clean data -> import and analyze -> results -> visualization), it can be 
helpful to identify that you're now somewhere between clean data and analysis.  

## Lesson outline

### 00-sql-introduction
* Introduce relational databases, database management systems, DB Browser for
SQLite, and the Portal dataset
* Import .csv files into sqlite
* Structuring data for database import
* Discuss the different SQL data types

**Tips**

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

## Alternative activities

### Queries on the board

As you teach the lesson, it can be helpful to pause and write up the query keywords 
on the board.  This could look like this: 

* After 01-sql-basic queries
~~~
SELECT column
       FUNCTION(column)
       
FROM table

WHERE (conditional statement, applies to row values) 
      (AND/OR) 
      

   
ORDER BY column/FUNCTION(column) (ASC/DESC)
~~~

* After 02-sql-aggregation
~~~
SELECT column
       FUNCTION(column)
       AGGREGATE_FUNCTION(column)
FROM table

WHERE (conditional statement, applies to row values) 
      (AND/OR) 
      (IS (NOT) NULL)
GROUP BY column
   HAVING (conditional statement, applies to group)
ORDER BY column/FUNCTION(column) (ASC/DESC)
~~~

* After 03-sql-joins-aliases
~~~
SELECT column
       FUNCTION(column)
       AGGREGATE_FUNCTION(column)
FROM table
JOIN table ON table.col = table.col
WHERE (conditional statement, applies to row values) 
      (AND/OR) 
      (IS (NOT) NULL)
GROUP BY column
   HAVING (conditional statement, applies to group)
ORDER BY column/FUNCTION(column) (ASC/DESC)
~~~

As a bonus, if you can leave this on the board, it translates nicely into 
the `dplyr` portion of the `R` lesson, i.e.: 

~~~
SQL:                                                 dplyr: 

SELECT column                                        select(col)
       FUNCTION(column)                              mutate(col = fcn(col))
       AGGREGATE_FUNCTION(column)                    summarize(col = fcn(col))
FROM table
JOIN table ON table.col = table.col
WHERE (conditional statement, applies to row values) filter(condition)
      (AND/OR) 
      (IS (NOT) NULL)                                is.na()
GROUP BY column                                      group_by(col)
   HAVING (conditional statement, applies to group)
ORDER BY column/FUNCTION(column) (ASC/DESC)          arrange()
~~~

### "Interactive" database

If you want to try something more active (esp. if you're teaching SQL in the 
afternoon!), this is a an interactive activity to try.  

* Give each student six cards, with the following labels: 
	* name
	* name
	* height
	* DoC
	* height*2.54
	* dept
* Have students fill out their cards: 
	* Name: first name
	* Height: height in INCHES
	* Dept (department): close enough (just pick one if you don’t have a home department)
	* DoC (Dog or Cat): Dog, Cat, Both, Neither, or leave blank
* Each student is now a *record* in an interactive "students" database, where 
each of the cards they hold is a *field* in that database.  
* At various points in the lesson, stop and "query" the student database.  To do this: 
	* On a slide (or in a text editor), show or type in a sample query.  Something like: 
    ~~~
    SELECT name, name FROM students WHERE height > 66
    ~~~
	* If the query applies to a record (student), that student should stand, 
	and display (hold up) the appropriate field (card)
	* See the following slide deck for a list of sample queries.  [Sample queries](https://speakerdeck.com/christinalk/query-live-database)
