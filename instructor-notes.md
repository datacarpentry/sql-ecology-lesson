---
title: Instructor Notes
---

## Learning objectives

- Understand concept of relational database
- Be able to perform simple queries in SQL from a single table
- Understand how to filter and sort results from a query
- Use aggregate functions to combine data
- Perform queries across tables

## Data

As for all of the [Data Carpentry ecology lessons](https://github.com/datacarpentry?utf8=%E2%9C%93&query=ecology), this
lesson uses the Portal Project Teaching Database. The data is available at <a href="https://doi.org/10.6084/m9.figshare.1314459">[https://doi.org/10.6084/m9.figshare.1314459](https://doi.org/10.6084/m9.figshare.1314459)</a> and the download includes a
SQLite database file (portal\_mammals.sqlite) as well as three .csv files
(species.csv, plots.csv, surveys.csv) that can be imported into SQLite.

**Note** that the figshare download is an archive (.zip) file that rudely explodes all of the files into your current directory.

## Motivation and Framing

See this slide deck as a sample intro for the lesson:
[SQL Intro Deck](https://speakerdeck.com/christinalk/data-carpentry-sql-introduction)

Key points:

- Want to query data, instead of editing directly
- Need a solution that is scalable and reproducible
- Introduce the typical ways of dealing with rectangular data (subsetting,
  split-apply-combine)

If you've written up a diagram of the data analysis pipeline (raw data ->
clean data -> import and analyze -> results -> visualization), it can be
helpful to identify that you're now somewhere between clean data and analysis.

## Common Difficult Concepts

- Making the leap from a research question to a query (seen in some of the challenges)
- Data import
  - Not necessarily a concept, but we always have at least a handful of people that
    struggle with the table import step -- especially in the virtual context with window changing
  - Data type options in SQLite (Integer, text, blob, real, numeric) when importing
    from CSV. (Maybe have a table of SQLite date types in the student material).
- Good to make sure that a comparison is drawn between joins in different
  languages, e.g. SQL vs tidyverse
- HAVING, and why it's different to WHERE.... - especially when teaching online
- given the dataset is *relatively* complex, some students (and instructors) find it difficult to remember what data is where, especially when they're from a totally different domain
- How NULLs behave in different circumstances (when did a NULL change your result and how do you know?)

## Lesson outline

### 00-sql-introduction

- Introduce relational databases, database management systems, DB Browser for
  SQLite, and the Portal dataset
- Import .csv files into sqlite
- Structuring data for database import
- Discuss the different SQL data types

**Tips**

- **Importing data**: Note how cleanly the csv files import into SQL. If you have also taught the spreadsheet lesson, it would be a good idea to compare the format of the csv files with the messy spreadsheet and ask "Remember that messy spreadsheet? What would have happened if we tried to load that in to SQL?"

### 00-supplement-database-design.md

(optional)
The first lesson includes a brief introduction to data design and choosing database systems. This material expands on the database design in the first section.

### 01-sql-basic-queries

- Write basic queries using SELECT and FROM
- Filter results using DISTINCT, WHERE
- Change how results are displayed using ORDER BY and by doing calculations on results

### 02-sql-aggregation

- Combine results using COUNT and GROUP BY
- Filtering based on aggregation with HAVING
- Saving queries using VIEW

### 03-sql-joins-aliases

- combining data from two tables using JOIN, ON, USING
- depending on time, could introduce different types of JOINs
- using aliases with AS to simplify queries

## Alternative activities

### Queries on the board

As you teach the lesson, it can be helpful to pause and write up the query keywords
on the board.  This could look like this:

- After 01-sql-basic queries

```
SELECT column
       FUNCTION(column)

FROM table

WHERE (conditional statement, applies to row values)
      (AND/OR)

ORDER BY column/FUNCTION(column) (ASC/DESC)
```

- After 02-sql-aggregation

```
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
```

- After 03-sql-joins-aliases

```
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
```

As a bonus, if you can leave this on the board, it translates nicely into
the `dplyr` portion of the `R` lesson, i.e.:

```
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
```

### "Interactive" database

If you want to try something more active (esp. if you're teaching SQL in the
afternoon!), this is a an interactive activity to try.

- Give each student six cards. Four of the cards should have the following labels:
  - name
  - height
  - dept
  - DoC
  - The remaining two cards should be blank (for now!)
- Have students fill out their cards:
  - name: first name
  - height: height in INCHES
  - dept: department (if none, just pick one)
  - DoC: choose from `Dog`, `Cat`, `Both`, `Neither`, or leave blank
- Each student is now a *record* in an interactive "students" database, where
  each of the cards they hold is a *field* in that database.
- At various points in the lesson, stop and "query" the student database.  To do this:
  - On a slide (or in a text editor), show or type in a sample query.  Something like:
  ```
  SELECT name, dept FROM students WHERE height > 66;
  ```
  - If the query applies to a record (student), that student should stand,
    and display (hold up) the appropriate field (card)
  - For some queries, the student may have to fill in a blank card with calculated data. For example:
  ```
  SELECT name, name, height*2.54 AS height_cm FROM students;
  ```
  - See the following slide deck for a list of sample queries.  [Sample queries](https://speakerdeck.com/christinalk/query-live-database)

## Exercise Solutions

##### WARNING

This file may not be always up to date with regards to the exact exercises
instructions and the naming of the columns and tables in the database. Check
before you run the workshop!

**EXERCISE**

Write a query that returns the year, month, day, species ID and weight (in mg).

**SOLUTION**

```
SELECT day, month, year, species_id, weight * 1000
FROM surveys;
```

**EXERCISE**

Write a query that returns the day, month, year, species ID, and weight (in kg)
for individuals caught on Plot 1 that weigh more than 75 g.

**SOLUTION**

```
SELECT day, month, year, species_id, weight / 1000
FROM surveys
WHERE plot_id = 1
AND weight > 75;
```

**EXERCISE**

Write a query that returns the day, month, year, species ID, and weight (in kg)
for individuals caught on Plot 1 that weigh more than 75 g.

**SOLUTION**

```
SELECT
	surveys.day,
	surveys.month,
	surveys.year,
	species.species_id,
	surveys.weight / 1000
FROM surveys
JOIN species ON surveys.species_id = species.species_id
WHERE surveys.weight > 75
AND surveys.plot_id = 1;
```

**EXERCISE**

Write a query that returns day, month, year, species ID for individuals caught
in January, May and July.

**SOLUTION**

```
SELECT day, month, year, species_id
FROM surveys
WHERE month IN (1, 5, 7);
```

**EXERCISE**

Write a query that returns year, species ID, and weight in kg from the surveys
table, sorted with the largest weights at the top.

**SOLUTION**

```
SELECT year, species_id, weight / 1000
FROM surveys ORDER BY weight DESC;
```

**EXERCISE**

Let's try to combine what we've learned so far in a single query. Using the
surveys table write a query to display the three date fields, species ID, and
weight in kilograms (rounded to two decimal places), for rodents captured in
1999, ordered alphabetically by the species ID.

**SOLUTION**

```
SELECT year, month, day, species_id, ROUND(weight / 1000, 2)
FROM surveys
WHERE year = 1999
ORDER BY species_id;
```

**EXERCISE**

Write queries that return:

1. How many individuals were counted in each year.

2. Average weight of each species in each year.

**SOLUTION**

```
SELECT year, COUNT(*)
FROM surveys
GROUP BY year;

SELECT year, species_id, ROUND(AVG(weight), 2)
FROM surveys
GROUP BY year, species_id;
```

**EXERCISE**

Write a query that returns the number of each species caught in each year
sorted from most often caught species to the least occurring ones within each
year starting from the most recent records.

**SOLUTION**

```
SELECT year, species_id, COUNT(*)
FROM surveys
GROUP BY year, species_id
ORDER BY year DESC, COUNT(*) DESC;
```

**EXERCISE**

Write a query that returns the genus, the species, and the weight of every
individual captured at the site.

**SOLUTION**

```
SELECT species.genus, species.species_id, surveys.weight
FROM surveys
JOIN species ON surveys.species_id = species.species_id;
```

**EXERCISE**

Write a query that returns the number of genus of the animals caught in each
plot in descending order.

**SOLUTION**

```
SELECT surveys.plot_id, species.genus, COUNT(*)
FROM surveys
JOIN species ON surveys.species_id = species.species_id
GROUP BY species.genus, surveys.plot_id
ORDER BY surveys.plot_id, COUNT(*) DESC
```


