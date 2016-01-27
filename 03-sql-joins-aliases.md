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

> ### Challenge:
>
> Write a query that finds the average weight of each rodent species (i.e., only include species with Rodent in the taxa field).


Aliases
-------

As queries get more complex names can get long and unwieldy. To help make things
clearer we can use aliases to assign new names to things in the query.

We can alias both table names:

    SELECT surv.year, surv.month, surv.day, sp.genus, sp.species
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

> ### Challenge (optional):
>
> SQL queries help us *ask* specific *questions* which we want to answer about our data. The real skill with SQL is to know how to translate our scientific questions into a sensible SQL query (and subsequently visualize and interpret our results).
>
> Have a look at the following questions; these questions are written in plain English. Can you translate them to *SQL queries* and give a suitable answer?  
> 1. How many plots from each type are there?  
> 2. How many specimens are of each gender are there for each year?  
> 3. How many specimens of each species were captured in each type of plot?  
> 4. What is the average weight of each taxa?  
> 5. What is the percentage of each species in each taxa?  
> 6. What are the minimum, maximum and average weight for each species of Rodent?  
> 7. What is the hindfoot length for male and female rodent of each species? Is there a Male / Female difference?  
> 8. What is the average weight of each rodent species over the course of the years? Is there any noticeable trend for any of the species?  
> 
> Proposed solutions:
> 
> 1. Solution: ```SELECT plot_type, count(*) AS howmany  FROM plots  GROUP BY plot_type  ORDER BY howmany DESC```
> 2. Solution: ```SELECT year, sex, count(*) AS num_animal  FROM surveys  WHERE sex IS NOT null  GROUP BY sex, year```
> 3. Solution: ```SELECT species_id, plot_type, count(*) FROM surveys JOIN plots USING (plot_id) WHERE species_id IS NOT null GROUP BY species_id, plot_type```
> 4. Solution: ```SELECT taxa, AVG(weight) FROM species JOIN surveys USING(species_id) GROUP BY taxa```
> 5. Solution: ```SELECT taxa, 100*count(*)/(SELECT cast(count(*) as float) FROM surveys) FROM surveys JOIN species USING (species_id ) GROUP BY taxa```
> 6. Solution: ```SELECT species_id, MIN(weight) as min_weight, MAX(weight) as max_weight, AVG(weight) as mean_weight FROM surveys JOIN species USING (species_id ) WHERE taxa = 'Rodent' GROUP BY species_id```
> 7. Solution: ```SELECT species_id, sex, AVG(hindfoot_length) as mean_foot_length  FROM surveys JOIN species USING (species_id ) WHERE taxa = 'Rodent' AND sex IS NOT NULL GROUP BY species_id, sex```
> 8. Solution: ```SELECT species_id, year, AVG(weight) as mean_weight FROM surveys JOIN species USING (species_id ) WHERE taxa = 'Rodent' GROUP BY species_id, year```


Previous: [SQL Aggregation](02-sql-aggregation.html)
