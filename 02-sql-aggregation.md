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

    SELECT species_id, COUNT(species_id)
    FROM surveys
    GROUP BY species_id
    HAVING COUNT(species_id) > 10;

The `HAVING` keyword works exactly like the `WHERE` keyword, but uses
aggregate functions instead of database fields.

If you use `AS` in your query to rename a column, `HAVING` can use this
information to make the query more readable. For example, in the above
query, we can call the `COUNT(species_id)` by another name, like
`occurrences`. This can be written this way:

    SELECT species_id, COUNT(species_id) AS occurrences
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
before the query itself. For example, if we want to save the query giving
the number of individuals in a view, we can write

    CREATE VIEW species_count AS
    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id;

Now, we will be able to access these results with a much shorter notation:

    SELECT *
    FROM species_count;

Assuming we do not need this view anymore, we can remove it from the database
almost as we would a table:

    DROP VIEW species_count;

You can also add a view using *Create View* in the *View* menu and see the
results in the *Views* tab just like a table

> ### Challenge
>
> Write a query that returns the number of each species
caught in each year sorted from most often caught species to the least
occurring ones within each year starting from the most recent records. Save
this query as a `VIEW`.

Previous: [SQL Basic Queries](01-sql-basic-queries.html) Next: [Joins and aliases.](03-sql-joins-aliases.html)
