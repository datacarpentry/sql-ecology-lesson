---
layout: lesson
root: .
title: Aggregation
minutes: 30
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


##### Ordering aggregated results.

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




Previous: [SQL Basic Queries](01-sql-basic-queries.html) Next: [Joins and aliases.](03-sql-joins-aliases.html)
