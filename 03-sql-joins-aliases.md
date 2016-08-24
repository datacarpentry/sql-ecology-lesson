---
layout: lesson
root: .
title: Joins and aliases
minutes: 30
---

## Joins

To combine data from two tables we use the SQL `JOIN` command, which comes after
the `FROM` command.

The `JOIN` command on its own will result in a cross product, where each row in
first table is paired with each row in the second table. Usually this is not
what is desired when combining two tables with data that is related in some way.

For that, we need to tell the computer which columns provide the link between the two
tables using the word `ON`.  What we want is to join the data with the same
species codes.

    SELECT *
    FROM surveys
    JOIN species
    ON surveys.species_id = species.species_id;

`ON` is like `WHERE`, it filters things out according to a test condition.  We use
the `table.colname` format to tell the manager what column in which table we are
referring to.

The output of the `JOIN` command will have columns from first table plus the
columns from the second table. For the above command, the output will be a table
that has the following column names:

| record_id | month | day | year | plot_id | species_id | sex | hindfoot_length | weight | species_id | genus | species | taxa |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ... |||||||||||||   
| 96  | 8  | 20  | 1997  | 12  | **DM**  |  M |  36  |  41  | **DM** | Dipodomys  | merriami  | Rodent  |
| ... |||||||||||||| 

Alternatively, we can use the word `USING`, as a short-hand.  In this case we are
telling the manager that we want to combine `surveys` with `species` and that
the common column is `species_id`.

    SELECT *
    FROM surveys
    JOIN species
    USING (species_id);

The output will only have one **species_id** column

| record_id | month | day | year | plot_id | species_id | sex | hindfoot_length | weight  | genus | species | taxa |
|---|---|---|---|---|---|---|---|---|---|---|---|
| ... ||||||||||||
| 96  | 8  | 20  | 1997  | 12  | DM  |  M |  36  |  41  | Dipodomys  | merriami  | Rodent  |
| ... |||||||||||||

We often won't want all of the fields from both tables, so anywhere we would
have used a field name in a non-join query, we can use `table.colname`.

For example, what if we wanted information on when individuals of each
species were captured, but instead of their species ID we wanted their
actual species names.

    SELECT surveys.year, surveys.month, surveys.day, species.genus, species.species
    FROM surveys
    JOIN species
    ON surveys.species_id = species.species_id;

| year | month | day | genus | species |
|---|---|---|---|---|
| ... |||||
| 1977 | 7 | 16 | Neotoma | albigula|
| 1977 | 7 | 16 | Dipodomys | merriami|
|...||||||

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
    GROUP BY plots.plot_type;

> ### Challenge:
>
> Write a query that returns the number of genus of the animals caught in each plot in descending order.

> ### Challenge:
>
> Write a query that finds the average weight of each rodent species (i.e., only include species with Rodent in the taxa field).


## Functions

SQL includes numerous functions for manipulating data. You've already seen some
of these being used for aggregation (`SUM` and `COUNT`) but there are functions
that operate on individual values as well. Probably the most important of these
are `IFNULL` and `NULLIF`. `IFNULL` allows us to specify a value to use in
place of `NULL`.

We can represent unknown sexes with "U" instead of `NULL`:

    SELECT species_id, sex, IFNULL(sex, 'U') AS non_null_sex
    FROM surveys;

The lone "sex" column is only included in the query above to illustrate where
`IFNULL` has changed values; this isn't a usage requirement.

> ### Challenge:
>
> Write a query that returns 30 instead of `NULL` for values in the
> `hindfoot_length` column.

> ### Challenge:
>
> Write a query that calculates the average hind-foot length of each species,
> assuming that unknown lengths are 30 (as above).

`IFNULL` can be particularly useful in `JOIN`. When joining the `species` and
`surveys` tables earlier, some results were excluded because the `species_id`
was `NULL`. We can use `IFNULL` to include them again, re-writing the `NULL` to
a valid joining value:

    SELECT surveys.year, surveys.month, surveys.day, species.genus, species.species
    FROM surveys
    JOIN species
    ON surveys.species_id = IFNULL(species.species_id, 'AB');

> ### Challenge:
>
> Write a query that returns the number of genus of the animals caught in each
> plot, using `IFNULL` to assume that unknown species are all of the genus
> "Rodent".

The inverse of `IFNULL` is `NULLIF`. This returns `NULL` if the first argument
is equal to the second argument. If the two are not equal, the first argument
is returned. This is useful for "nulling out" specific values.

We can "null out" plot 7:

    SELECT species_id, plot_id, NULLIF(plot_id, 7) AS partial_plot_id
    FROM surveys;

Some more functions which are common to SQL databases are listed in the table
below:

| Function                     | Description                                                                                     |
|------------------------------|-------------------------------------------------------------------------------------------------|
| `ABS(n)`                     | Returns the absolute (positive) value of the numeric expression *n*                             |
| `LENGTH(s)`                  | Returns the length of the string expression *s*                                                 |
| `LOWER(s)`                   | Returns the string expression *s* converted to lowercase                                        |
| `NULLIF(x, y)`               | Returns NULL if *x* is equal to *y*, otherwise returns *x*                                      |
| `ROUND(n)` or `ROUND(n, x)`  | Returns the numeric expression *n* rounded to *x* digits after the decimal point (0 by default) |
| `TRIM(s)`                    | Returns the string expression *s* without leading and trailing whitespace characters            |
| `UPPER(s)`                   | Returns the string expression *s* converted to uppercase                                        |

Finally, some useful functions which are particular to SQLite are listed in the
table below:

| Function                            | Description                                                                                                                                                                    |
|-------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `IFNULL(x, y)`                      | Returns *x* if it is non-NULL, otherwise returns *y*                                                                                                                           |
| `RANDOM()`                          | Returns a random integer between -9223372036854775808 and +9223372036854775807.                                                                                                |
| `REPLACE(s, f, r)`                  | Returns the string expression *s* in which every occurrence of *f* has been replaced with *r*                                                                                  |
| `SUBSTR(s, x, y)` or `SUBSTR(s, x)` | Returns the portion of the string expression *s* starting at the character position *x* (leftmost position is 1), *y* characters long (or to the end of *s* if *y* is omitted) |

> ### Challenge:
>
> Write a query that returns genus names, sorted from longest genus name down
> to shortest.

## Aliases

As queries get more complex names can get long and unwieldy. To help make things
clearer we can use aliases to assign new names to things in the query.

We can alias both table names:

    SELECT surv.year, surv.month, surv.day, sp.genus, sp.species
    FROM surveys AS surv
    JOIN species AS sp
    ON surv.species_id = sp.species_id;

And column names:

    SELECT surv.year AS yr, surv.month AS mo, surv.day AS day, sp.genus AS gen, sp.species AS sp
    FROM surveys AS surv
    JOIN species AS sp
    ON surv.species_id = sp.species_id;

The `AS` isn't technically required, so you could do

    SELECT surv.year yr
    FROM surveys surv;

but using `AS` is much clearer so it is good style to include it.

> ### Challenge (optional):
>
> SQL queries help us *ask* specific *questions* which we want to answer about our data. The real skill with SQL is to know how to translate our scientific questions into a sensible SQL query (and subsequently visualize and interpret our results).
>
> Have a look at the following questions; these questions are written in plain English. Can you translate them to *SQL queries* and give a suitable answer?  
> 
> 1. How many plots from each type are there?  
> 
> 2. How many specimens are of each sex are there for each year?  
> 
> 3. How many specimens of each species were captured in each type of plot?  
> 
> 4. What is the average weight of each taxa?  
> 
> 5. What is the percentage of each species in each taxa?  
> 
> 6. What are the minimum, maximum and average weight for each species of Rodent?  
>
> 7. What is the average hindfoot length for male and female rodent of each species? Is there a Male / Female difference?  
> 
> 8. What is the average weight of each rodent species over the course of the years? Is there any noticeable trend for any of the species?  

> Proposed solutions:
>
> 1. Solution: `SELECT plot_type, count(*) AS num_plots  FROM plots  GROUP BY plot_type  ORDER BY num_plots DESC`
>
> 2. Solution: `SELECT year, sex, count(*) AS num_animal  FROM surveys  WHERE sex IS NOT null  GROUP BY sex, year`
>
> 3. Solution: `SELECT species_id, plot_type, count(*) FROM surveys JOIN plots ON surveys.plot_id=plots.plot_id WHERE species_id IS NOT null GROUP BY species_id, plot_type`
>
> 4. Solution: `SELECT taxa, AVG(weight) FROM surveys JOIN species ON species.species_id=surveys.species_id GROUP BY taxa`
>
> 5. Solution: `SELECT taxa, 100.0*count(*)/(SELECT count(*) FROM surveys) FROM surveys JOIN species ON surveys.species_id=species.species_id GROUP BY taxa`
>
> 6. Solution: `SELECT surveys.species_id, MIN(weight) as min_weight, MAX(weight) as max_weight, AVG(weight) as mean_weight FROM surveys JOIN species ON surveys.species_id=species.species_id WHERE taxa = 'Rodent' GROUP BY surveys.species_id`
>
> 7. Solution: `SELECT surveys.species_id, sex, AVG(hindfoot_length) as mean_foot_length  FROM surveys JOIN species ON surveys.species_id=species.species_id WHERE taxa = 'Rodent' AND sex IS NOT NULL GROUP BY surveys.species_id, sex`
>
> 8. Solution: `SELECT surveys.species_id, year, AVG(weight) as mean_weight FROM surveys JOIN species ON surveys.species_id=species.species_id WHERE taxa = 'Rodent' GROUP BY surveys.species_id, year`


Previous: [SQL Aggregation](02-sql-aggregation.html)
