---
layout: lesson
root: .
title: Aggregation
minutes: 60
---

COUNT and GROUP BY
----
Aggregation allows us to combine results by grouping records based on value and
calculating combined values in groups.

Let’s go to the surveys table and find out how many individuals there are.
Using the wildcard simply counts the number of records (rows)

    SELECT COUNT(*) FROM surveys

We can also find out how much all of those individuals weigh.

    SELECT COUNT(*), SUM(weight) FROM surveys

We can output this value in kilograms, rounded to 3 decimal
   places:

    SELECT ROUND(SUM(weight)/1000.0, 3) FROM surveys

There are many other aggregate functions included in SQL including
`MAX`, `MIN`, and `AVG`.

> ### Challenge
>
> Write query that returns: total weight, average weight, and the min and max weights for all animals caught over the duration of the survey. Can you modify it so that it outputs that for a range of weights?


Now, let's see how many individuals were counted in each species. We do this
using a GROUP BY clause

    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id

GROUP BY tells SQL what field or fields we want to use to aggregate the data.
If we want to group by multiple fields, we give GROUP BY a comma separated list.

> ### Challenge
>
> Write queries that return:
>
> 1. How many individuals were counted in each year.  
a) in total;  
b) per each species.
> 2. Average weight of each species in each year.  
Can you modify the above queries combining them into one? 


##### Ordering arrgregated results.

We can order the results of our aggregation by a specific column, including the
aggregated column.  Let’s count the number of individuals of each species
captured, ordered by the count

    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id
    ORDER BY COUNT(species_id)
    
> ### Challenge
>
>   Write a query that returns the number of each species caught in each year sorted from most often caught species to the least occurring ones within each year starting from the most recent records.    


Joins
-----

To combine data from two tables we use the SQL `JOIN` command, which comes after
the `FROM` command.

We also need to tell the computer which columns provide the link between the two
tables using the word `ON`.  What we want is to join the data with the same
species codes.

    SELECT *
    FROM surveys
    JOIN species ON surveys.species_id = species.species_id

ON is like `WHERE`, it filters things out according to a test condition.  We use
the `table.colname` format to tell the manager what column in which table we are
referring to.

We often won't want all of the fields from both tables, so anywhere we would
have used a field name in a non-join query, we can use `table.colname`.

For example, what if we wanted information on when individuals of each
species were captured, but instead of their species ID we wanted their
actual species names.

    SELECT surveys.year, surveys.month, surveys.day, species.genus, species.species
    FROM surveys
    JOIN species ON surveys.species_id = species.species_id

> ### Challenge:
>
> Write a query that returns the genus, the species, and the weight
> of every individual captured at the site

Joins can be combined with sorting, filtering, and aggregation.  So, if we
wanted average mass of the individuals on each different type of treatment, we
could do something like

    SELECT plots.plot_type, AVG(surveys.weight)
    FROM surveys
    JOIN plots
    ON surveys.plot_id = plots.plot_id
    GROUP BY plots.plot_type

> ### Challenge:
>
> Write a query that returns the number of genus of the animals caught in each plot in descending order.



Aliases
-------

As queries get more complex names can get long and unwieldy. To help make things
clearer we can use aliases to assign new names to things in the query.

We can alias both table names:

    SELECT surv.year, surv.month, surv.day, surv.genus, sp.species
    FROM surveys AS surv
    JOIN species AS sp ON surv.species_id = sp.species_id

And column names:

    SELECT surv.year AS yr, surv.month AS mo, surv.day AS day, sp.genus AS gen, sp.species AS sp
    FROM surveys AS surv
    JOIN species AS sp ON surv.species_id = sp.species_id

The `AS` isn't technically required, so you could do

    SELECT surv.year yr
    FROM surveys surv

but using `AS` is much clearer so it's good style to include it.



Previous: [SQL Basic Queries](01-sql-basic-queries.html) 
