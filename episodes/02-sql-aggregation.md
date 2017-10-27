---
title: "SQL Aggregation and aliases"
teaching: 50
exercises: 10
questions:
- "How can I summarize my data by aggregating, filtering, or ordering query results?"
- "How can I make sure column names from my queries make sense and aren't too long?"
objectives:
- "Apply aggregation to group records in SQL."
- "Filter and order results of a query based on aggregate functions."
- "Employ aliases to assign new names to items in a query."
- "Save a query to make a new table."
- "Apply filters to find missing values in SQL."
keypoints:
- "Use the `GROUP BY` keyword to aggregate data."
- "Functions like `MIN`, `MAX`, `AVERAGE`, `SUM`, `COUNT`, etc. operate on aggregated data."
- "Aliases can help shorten long queries. To write clear and readible queries, use the `AS` keyword when creating aliases."
- "Use the `HAVING` keyword to filter on aggregate properties."
- "Use a `VIEW` to access the result of a query as though it was a new table."
---

## COUNT and GROUP BY

Aggregation allows us to combine results by grouping records based on value, also it is useful for
calculating combined values in groups.

Let’s go to the surveys table and find out how many individuals there are.
Using the wildcard * simply counts the number of records (rows):

    SELECT COUNT(*)
    FROM surveys;

We can also find out how much all of those individuals weigh:

    SELECT COUNT(*), SUM(weight)
    FROM surveys;

We can output this value in kilograms (dividing the value to 1000.0), then rounding to 3 decimal places:
(Notice the divisor has numbers after the decimal point, which forces the answer to have a decimal fraction)

    SELECT ROUND(SUM(weight)/1000.00, 3)
    FROM surveys;

There are many other aggregate functions included in SQL, for example:
`MAX`, `MIN`, and `AVG`.

> ## Challenge
>
> Write a query that returns: total weight, average weight, and the min and max weights
> for all animals caught over the duration of the survey.
> Can you modify it so that it outputs these values only for weights between 5 and 10?
{: .challenge}

Now, let's see how many individuals were counted in each species. We do this
using a `GROUP BY` clause

    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id;

`GROUP BY` tells SQL what field or fields we want to use to aggregate the data.
If we want to group by multiple fields, we give `GROUP BY` a comma separated list.

> ## Challenge
>
> Write queries that return:
>
> 1. How many individuals were counted in each year
>    *   in total
>    *   per each species
> 2. Average weight of each species in each year.
>
> Can you modify the above queries combining them into one?
{: .challenge}

## Ordering Aggregated Results

We can order the results of our aggregation by a specific column, including
the aggregated column.  Let’s count the number of individuals of each
species captured, ordered by the count:

    SELECT species_id, COUNT(*)
    FROM surveys
    GROUP BY species_id
    ORDER BY COUNT(species_id);

## Aliases

As queries get more complex names can get long and unwieldy. To help make things
clearer we can use aliases to assign new names to things in the query.

We can use aliases in column names or table names using `AS`:

    SELECT MAX(year) AS last_surveyed_year
    FROM surveys;

The `AS` isn't technically required, so you could do

    SELECT MAX(year) yr
    FROM surveys surv;

but using `AS` is much clearer so it is good style to include it.

## The `HAVING` keyword

In the previous episode, we have seen the keyword `WHERE`, allowing to
filter the results according to some criteria. SQL offers a mechanism to
filter the results based on **aggregate functions**, through the `HAVING` keyword.

For example, we can request to only return information
about species with a count higher than 10:

    SELECT species_id, COUNT(species_id)
    FROM surveys
    GROUP BY species_id
    HAVING COUNT(species_id) > 10;

The `HAVING` keyword works exactly like the `WHERE` keyword, but uses
aggregate functions instead of database fields to filter.

If you use `AS` in your query to rename a column, `HAVING` you can use this
information to make the query more readable. For example, in the above
query, we can call the `COUNT(species_id)` by another name, like
`occurrences`. This can be written this way:

    SELECT species_id, COUNT(species_id) AS occurrences
    FROM surveys
    GROUP BY species_id
    HAVING occurrences > 10;

Note that in both queries, `HAVING` comes *after* `GROUP BY`. One way to
think about this is: the data are retrieved (`SELECT`), which can be filtered
(`WHERE`), then joined in groups (`GROUP BY`); finally, we can filter again based on some
of these groups (`HAVING`).

> ## Challenge
>
> Write a query that returns, from the `species` table, the number of
> `genus` in each `taxa`, only for the `taxa` with more than 10 `genus`.
{: .challenge}

## Saving Queries for Future Use

It is not uncommon to repeat the same operation more than once, for example
for monitoring or reporting purposes. SQL comes with a very powerful mechanism
to do this by creating views. Views are a form of query that is saved in the database,
and can be used to look at, filter, and even update information. One way to
think of views is as a table, that can read, aggregate, and filter information
from several places before showing it to you.

Creating a view from a query requires to add `CREATE VIEW viewname AS`
before the query itself. For example, imagine that my project only covers
the data gathered during the summer (May - September) of 2000.  That
query would look like:

    SELECT *
    FROM surveys
    WHERE year = 2000 AND (month > 4 AND month < 10);

But we don't want to have to type that every time we want to ask a
question about that particular subset of data. Hence, we can benefit from a view:

    CREATE VIEW summer_2000 AS
    SELECT *
    FROM surveys
    WHERE year = 2000 AND (month > 4 AND month < 10);

You can also add a view using *Create View* in the *View* menu and see the
results in the *Views* tab, the same way as creating a table from the menu.

Using a view we will be able to access these results with a much shorter notation:

    SELECT *
    FROM summer_2000
    WHERE species_id == 'PE';

## What About NULL?

From the last example, there should only be six records.  If you look at the `weight` column, it's
easy to see what the average weight would be. If we use SQL to find the
average weight, SQL behaves like we would hope, ignoring the NULL values:

    SELECT AVG(weight)
    FROM summer_2000
    WHERE species_id == 'PE';

But if we try to be extra clever, and find the average ourselves,
we might get tripped up:

    SELECT SUM(weight), COUNT(*), SUM(weight)/COUNT(*)
    FROM summer_2000
    WHERE species_id == 'PE';

Here the `COUNT` command includes all six records (even those with NULL
values), but the `SUM` only includes the 4 records with data in the
`weight` field, giving us an incorrect average. However,
our strategy *will* work if we modify the `COUNT` command slightly:

    SELECT SUM(weight), COUNT(weight), SUM(weight)/COUNT(weight)
    FROM summer_2000
    WHERE species_id == 'PE';

When we count the weight field specifically, SQL ignores the records with data
missing in that field.  So here is one example where NULLs can be tricky:
`COUNT(*)` and `COUNT(field)` can return different values.

Another case is when we use a "negative" query.  Let's count all the
non-female animals:

    SELECT COUNT(*)
    FROM summer_2000
    WHERE sex != 'F';

Now let's count all the non-male animals:

    SELECT COUNT(*)
    FROM summer_2000
    WHERE sex != 'M';

But if we compare those two numbers with the total:

    SELECT COUNT(*)
    FROM summer_2000;

We'll see that they don't add up to the total! That's because SQL
doesn't automatically include NULL values in a negative conditional
statement.  So if we are quering "not x", then SQL divides our data
into three categories: 'x', 'not NULL, not x' and NULL; then,
returns the 'not NULL, not x' group. Sometimes this may be what we want -
but sometimes we may want the missing values included as well! In that
case, we'd need to change our query to:

    SELECT COUNT(*)
    FROM summer_2000
    WHERE sex != 'M' OR sex IS NULL;
