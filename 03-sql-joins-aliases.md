---
layout: lesson
root: .
title: Joins and aliases
minutes: 30
---


Joins
-----

To combine data from two tables we use the SQL `JOIN` command, which comes after
the `FROM` command.

We also need to tell the computer which columns provide the link between the two
tables using the word `ON`.  What we want is to join the data with the same
species codes.

    SELECT *
    FROM surveys JOIN species
    ON surveys.species_id = species.species_id

ON is like `WHERE`, it filters things out according to a test condition.  We use
the `table.colname` format to tell the manager what column in which table we are
referring to.

We often won't want all of the fields from both tables, so anywhere we would
have used a field name in a non-join query, we can use `table.colname`.

For example, what if we wanted information on when individuals of each
species were captured, but instead of their species ID we wanted their
actual species names.

    SELECT surveys.year, surveys.month, surveys.day, species.genus, species.species
    FROM surveys JOIN species
    ON surveys.species_id = species.species_id

> ### Challenge:
>
> Write a query that returns the genus, the species, and the weight
> of every individual captured at the site

Joins can be combined with sorting, filtering, and aggregation.  So, if we
wanted average mass of the individuals on each different type of treatment, we
could do something like

    SELECT plots.plot_type, AVG(surveys.weight)
    FROM surveys JOIN plots
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
    FROM surveys AS surv JOIN species AS sp
    ON surv.species_id = sp.species_id

And column names:

    SELECT surv.year AS yr, surv.month AS mo, surv.day AS day, sp.genus AS gen, sp.species AS sp
    FROM surveys AS surv JOIN species AS sp
    ON surv.species_id = sp.species_id

The `AS` isn't technically required, so you could do

    SELECT surv.year yr
    FROM surveys surv

but using `AS` is much clearer so it's good style to include it.

Previous: [SQL Aggregation](02-sql-aggregation.html) 

