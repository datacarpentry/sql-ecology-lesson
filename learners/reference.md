---
title: 'FIXME'
---

## Glossary

## Reference

See [this cheat sheet](files/sql-cheat-sheet.md) for an list of the commands
covered in this lesson.

### Keywords Description Summary

<table>
    <thead>
        <tr>
            <th>Keyword</th>
            <th>Definition</th>
            <th>Example</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=3> SELECT </td>
            <td rowspan = 3> Select data from database or table </td>
            <td> SELECT *</td>
            <td> Selects the entire dataset</td>
        </tr>
        <tr>
            <td>SELECT column1</td>
            <td> Selects a particular column</td>
        </tr>
        <tr>
            <td>SELECT 1 + 2</td>
            <td>Performs a calculation</td>
        </tr>
        <tr>
            <td>FROM</td>
            <td>Indicates the table from which the data is selected or deleted</td>
            <td>SELECT year <br> FROM surveys</td>
            <td>Query will display the desired column from the table</td>
        </tr>
        <tr>
            <td>WHERE</td>
            <td>Filter the result set so that only the values satisfying the condition are included</td>
            <td>SELECT * <br> FROM surveys <br> WHERE year == 1990</td>
            <td>Query will display all values from the table for which the year is 1990</td>
        </tr>
        <tr>
            <td>LIMIT</td>
            <td>Retrieves the number of records from the table up to the limit value</td>
            <td>SELECT * <br> FROM surveys <br> LIMIT 5 </td>
            <td>Query will will return only the first 5 rows from the table</td>
        </tr>
        <tr>
            <td>DISTINCT</td>
            <td>Select distinct values</td>
            <td>SELECT DISTINCT year <br> FROM surveys </td>
            <td>Query will display the distinct years present on the table </td>
        </tr>
        <tr>
            <td>AS</td>
            <td>Used as an alias to rename the column or table</td>
            <td>SELECT 1 + 2 AS calc</td>
            <td>Column will be renamed to "calc"</td>
        </tr>
        <tr>
            <td>GROUP BY</td>
            <td>Groups the result set</td>
            <td>SELECT MAX(weight) <br> FROM surveys <br> GROUP BY year</td>
            <td>Query will display the max weight per year</td>
        </tr>
        <tr>
            <td>HAVING</td>
            <td>Used to filter grouped rows when using aggregate functions</td>
            <td>SELECT MAX(weight) <br> FROM surveys <br> GROUP BY year HAVING MAX(weight) > 100</td>
            <td>Filter the results by the years that have a maximum weight greater than 100g</td>
        </tr>
        <tr>
            <td>JOIN</td>
            <td> Joins tables</td>
            <td>SELECT * <br> FROM surveys <br> JOIN species <br> ON surveys.species_id = species.species_id</td>
            <td>Query will display all the columns from both tables where the condition is met</td>
        </tr>
    </tbody>
</table>


