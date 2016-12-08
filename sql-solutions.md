---
layout: lesson
root: .
title: Solutions to SQL exercises
---

##### WARNING

This file may not be always up to date with regards to the exact exercises
instructions and the naming of the columns and tables in the database. Check
before you run the workshop!


**EXERCISE**

Write a query that returns the year, month, day, species ID and weight (in mg).

**SOLUTION**

	SELECT day, month, year, species_id, weight * 1000
	FROM surveys;


**EXERCISE**

Write a query that returns the day, month, year, species ID, and weight (in kg)
for individuals caught on Plot 1 that weigh more than 75 g.

**SOLUTION**

	SELECT day, month, year, species_id, weight / 1000
	FROM surveys
	WHERE plot_id = 1
	AND weight > 75;


**EXERCISE**

Write a query that returns the day, month, year, species ID, and weight (in kg)
for individuals caught on Plot 1 that weigh more than 75 g.

**SOLUTION**

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


**EXERCISE**

Write a query that returns day, month, year, species ID for individuals caught
in January, May and July.

 **SOLUTION**

	SELECT day, month, year, species_id
	FROM surveys
	WHERE month IN (1, 5, 7);


**EXERCISE**

Write a query that returns year, species ID, and weight in kg from the surveys
table, sorted with the largest weights at the top.

**SOLUTION**

	SELECT year, species_id, weight / 1000
	FROM surveys ORDER BY weight DESC;


**EXERCISE**

Let’s try to combine what we’ve learned so far in a single query. Using the
surveys table write a query to display the three date fields, species ID, and
weight in kilograms (rounded to two decimal places), for rodents captured in
1999, ordered alphabetically by the species ID.

**SOLUTION**

	SELECT year, month, day, species_id, ROUND(weight / 1000, 2)
	FROM surveys
	WHERE year = 1999
	ORDER BY species_id;


**EXERCISE**

Write queries that return:

1. How many individuals were counted in each year.

2. Average weight of each species in each year.

**SOLUTION**

	SELECT year, COUNT(*)
	FROM surveys
	GROUP BY year;

	SELECT year, species_id, ROUND(AVG(weight), 2)
	FROM surveys
	GROUP BY year, species_id;


**EXERCISE**

Write a query that returns the number of each species caught in each year
sorted from most often caught species to the least occurring ones within each
year starting from the most recent records.

**SOLUTION**

	SELECT year, species_id, COUNT(*)
	FROM surveys
	GROUP BY year, species_id
	ORDER BY year DESC, COUNT(*) DESC;


**EXERCISE**

Write a query that returns the genus, the species, and the weight of every
individual captured at the site.

**SOLUTION**

	SELECT species.genus, species.species_id, surveys.weight
	FROM surveys
	JOIN species ON surveys.species_id = species.species_id;


**EXERCISE**

Write a query that returns the number of genus of the animals caught in each
plot in descending order.

**SOLUTION**

	SELECT surveys.plot_id, species.genus, COUNT(*)
	FROM surveys
	JOIN species ON surveys.species_id = species.species_id
	GROUP BY species.genus, surveys.plot_id
	ORDER BY surveys.plot_id, COUNT(*) DESC

