---
layout: lesson
root: .
title: Aggregation
minutes: 30
---

## COUNT and GROUP BY

Aggregation allows us to combine results by grouping records based on value and
calculating combined values in groups.

Let’s go to the surveys table and find out how many individuals there are.
Using the wildcard simply counts the number of records (rows)

    SELECT COUNT(*)
    FROM surveys;

We can also find out how much all of those individuals weigh.

    SELECT COUNT(*), SUM(weight)
    FROM surveys;

We can output this value in kilograms, rounded to 3 decimal
places:

    SELECT ROUND(SUM(weight)/1000.0, 3)
    FROM surveys;

There are many other aggregate functions included in SQL including
`MAX`, `MIN`, and `AVG`.

> ### Challenge
>
> Write a query that returns: total weight, average weight, and the min and max weights for all animals caught over the duration of the survey. Can you modify it so that it outputs these values only for weights between 5 and 10?


Now, let's see how many individuals were counted in each species. We do this
using a `GROUP BY` clause

    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id;

`GROUP BY` tells SQL what field or fields we want to use to aggregate the data.
If we want to group by multiple fields, we give `GROUP BY` a comma separated list.

> ### Challenge
>
> Write queries that return:
>
> 1. How many individuals were counted in each year.
a) in total;
b) per each species.
> 2. Average weight of each species in each year.
Can you modify the above queries combining them into one?

## The `HAVING` keyword

In the previous lesson, we have seen the keywords `WHERE`, allowing to
filter the results according to some criteria. SQL offers a mechanism to
filter the results based on aggregate functions, through the `HAVING` keyword.

For example, we can adapt the last request we wrote to only return information
about species with a count higher than 10:

    SELECT species_id, COUNT(surveys.species_id)
    FROM surveys
    GROUP BY species_id
    HAVING COUNT(surveys.species_id) > 10;

The `HAVING` keyword works exactly like the `WHERE` keyword, but uses
aggregate functions instead of database fields.

If you use `AS` in your query to rename a column, `HAVING` can use this
information to make the query more readable. For example, in the above
query, we can call the `COUNT(surveys.species_id)` by another name, like
`occurrences`. This can be written this way:

    SELECT species_id, COUNT(surveys.species_id) AS occurrences
    FROM surveys
    GROUP BY species_id
    HAVING occurrences > 10;

Note that in both queries, `HAVING` comes *after* `GROUP BY`. One way to
think about this is: the data are retrieved (`SELECT`), can be filtered
(`WHERE`), then joined in groups (`GROUP BY`); finally, we only select some
of these groups (`HAVING`).

> ### Challenge
>
> Write a query that returns, from the `species` table, the number of
`genus` in each `taxa`, only for the `taxa` with more than 10 `genus`.

## Ordering aggregated results.

We can order the results of our aggregation by a specific column, including
the aggregated column.  Let’s count the number of individuals of each
species captured, ordered by the count

    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id
    ORDER BY COUNT(species_id);


## Saving queries for future use

It is not uncommon to repeat the same operation more than once, for example
for monitoring or reporting purposes. SQL comes with a very powerful mechanism
to do this: views. Views are a form of query that is saved in the database,
and can be used to look at, filter, and even update information. One way to
think of views is as a table, that can read, aggregate, and filter information
from several places before showing it to you.

Creating a view from a query requires to add `CREATE VIEW viewname AS`
before the query itself. For example, imagine that my project only covers
the data gathered during the summer (May - September) of 2000.  That 
query would look like: 

    SELECT *
    FROM surveys
    WHERE year = 2000 AND (month > 4 AND month < 10)

But we don't want to have to type that every time we want to ask a 
question about that particular subset of data!  Let's create a view: 

    CREATE VIEW summer_2000 AS
    SELECT *
    FROM surveys
    WHERE year = 2000 AND (month > 4 AND month < 10)

You can also add a view using *Create View* in the *View* menu and see the
results in the *Views* tab just like a table

Now, we will be able to access these results with a much shorter notation:

    SELECT *
    FROM summer_2000;

> ### Challenge
>
> Write a query that returns the number of each species
caught in each year sorted from most often caught species to the least
occurring ones within each year starting from the most recent records. Save
this query as a `VIEW`.

## Null values

Using the view we created in the previous section (`summer_2000`), let's 
talk about null values.  Missing values in SQL are identified with the 
special NULL value.  Scroll through our `summer_2000` view.  It should be 
easy to find several records with missing values.  How do you think we 
could filter to find these rows?  

To find all records where the species_id 
is missing, we can use: 

    SELECT *
    FROM summer_2000
    WHERE species_id IS NULL

If we wanted to use all the records where `species_id` is NOT null, we 
would add the `NOT` keyword to our query.  

    SELECT *
    FROM summer_2000
    WHERE species_id IS NOT NULL

There are many hidden "gotchas" with NULL values.  If we restrict our 
query to the "PE" species, this will be easier to see: 

    SELECT *
    FROM summer_2000
    WHERE species_id == 'PE'

There should only be six records.  If you look at the weight column, it's 
easy to see what the average weight would be.  If we use SQL to find the 
average weight, SQL behaves like we would hope, ignoring
the NULL values:

    SELECT AVG(weight)
    FROM summer_2000
    WHERE species_id == 'PE'

But if we try to be extra clever, and find the average ourselves, 
we might get tripped up: 

   SELECT SUM(weight), COUNT(*), SUM(weight)/COUNT(*)
   FROM summer_2000
   WHERE species_id == 'PE'

Here the COUNT command includes all six records (even those with null 
values), but the SUM only includes the 4 records with data in the 
weight field, giving us an incorrect average.  However, 
my strategy *will* work if I modify the count command slightly: 

   SELECT SUM(weight), COUNT(weight), SUM(weight)/COUNT(weight)
   FROM summer_2000
   WHERE species_id == 'PE'

When I count the weight field specifically, it ignores the records 
with data missing in that field.  So 
here is one example where NULLs can be tricky - COUNT(*) and 
COUNT(field) can return different values.  

Another case is when 
we use a "negative" query - let's count all the non-female animals: 

   SELECT COUNT(*) 
   FROM summer_2000
   WHERE sex != 'F'

Now let's count all the non-male animals: 

   SELECT COUNT(*) 
   FROM summer_2000
   WHERE sex != 'M'

But if we compare those two numbers with the total: 

   SELECT COUNT(*) 
   FROM summer_2000

We'll see that they don't add up to the total!  That's because SQL 
doesn't automatically include NULL values in a negative conditional 
statement.  So if we are quering "not x", then SQL divides our data 
into three categories: 'x', 'not NULL, not x' and NULL and 
returns the 'not NULL, not x' group. Sometimes this may be what we want - 
but sometimes we may want the missing values included as well!  In that 
case, we'd need to change our query to: 

  SELECT COUNT(*)
  FROM summer_2000
  WHERE sex != 'M' OR sex IS NULL

Previous: [SQL Basic Queries](01-sql-basic-queries.html) Next: [Joins and aliases.](03-sql-joins-aliases.html)
