---
title: Reference
---

## Glossary

*The definitions below are modified from the [Carpentries
Glosario](https://glosario.carpentries.org/) (CC-BY-4.0)*

[[**aggreation**]{#aggreation}](https://glosario.carpentries.org/en/#aggreation)
:   To combine many values into one, e.g., by summing a set of numbers.

[[**field**]{#field}](https://glosario.carpentries.org/en/#field)
:   A component of a [record](#record) containing a single value. Every
    record in a database [table](#table) has the same fields.

[[**filter**]{#filter}](https://glosario.carpentries.org/en/#filter)
:   To choose a set of [records](#record) (i.e., rows of a table) based
    on the values they contain.

[[**full join**]{#full-join}](https://glosario.carpentries.org/en/#full_join)
:   A [join](#join) that returns all rows and all columns from two
    tables A and B. Where the [keys](#key) of A and B match, values are
    combined; where they do not, missing values from either table are
    filled with [null](#null), NA, or some other [missing
    value](#missing-value) signifier.

[[**group**]{#group}](https://glosario.carpentries.org/en/#group)
:   To divide data into subsets according to a set of criteria while
    leaving records in a single structure.

[[**inner join**]{#inner-join}](https://glosario.carpentries.org/en/#inner_join)
:   A [join](#join) that returns the combination of rows from two
    tables, A and B, whose [keys](#key) exist in both tables.

[[**join**]{#join}](https://glosario.carpentries.org/en/#join)
:   One of several operations that combine values from two
    [tables](#table).

[[**key**]{#key}](https://glosario.carpentries.org/en/#key)
:   A [field](#field) or combination of fields whose value(s)
    uniquely identify a [record](#record) within a [table](#table) or
    dataset. Keys are often used to select specific records and in
    [joins](#join).

[[**left join**]{#left-join}](https://glosario.carpentries.org/en/#left_join)
:   A [join](#join) that combines data from two tables, A and B, where
    [keys](#key) in table A match keys in table B, [fields](#field) are
    concatenated. Where a key in table A does *not* match a key in
    table B, columns from table B are filled with [null](#null), NA, or
    some other [missing value](#missing-value). Keys from table B that
    do not match keys from table A are excluded for the result.

[[**missing value**]{#missing-value}](https://glosario.carpentries.org/en/#missing_value)
:   A special value such as [null](#null) or NA used to indicate the
    absence of data. Missing values can signal that data was not
    collected or that the data did not exist in the first place (e.g.,
    the middle name of someone who does not have one).

[[**null**]{#null}](https://glosario.carpentries.org/en/#null)
:   A special value used to represent a missing object.

[[**record**]{#record}](https://glosario.carpentries.org/en/#record)
:   A group of related values that are stored together. A record may be
    represented as a tuple or as a row in a [table](#table); in the
    latter case, every record in the table has the same
    [fields](#field).

[[**relational_database**]{#relational-database}](https://glosario.carpentries.org/en/#relational_database)
:   A database that organizes information into [tables](#table), each of
    which has a fixed set of named [fields](#field) (shown as columns)
    and a variable number of [records](#record) (shown as rows).

[[**right join**]{#right-join}](https://glosario.carpentries.org/en/#right_join)
:   A [join](#join) that combines data from two tables, A and B. Where
    [keys](#key) in table A match keys in table B, [fields](#field) are
    concatenated. Where a key in table B does \*not\* match a key in
    table A, columns from table A are filled with [null](#null), NA, or
    some other [missing value](#missing-value) signifier. Keys from
    table A that do not exist in table B are dropped.

[[**select**]{#select}](https://glosario.carpentries.org/en/#select)
:   To choose entire columns or rows from a table by name or location.

[[**SQL**]{#SQL}](https://glosario.carpentries.org/en/#sql)
:   The language used for writing queries for a [relational
    database](#relational-database). The term is an acronym for
    Structured Query Language.

[[**table**]{#table}](https://glosario.carpentries.org/en/#table)
:   A set of [records](#record) in a [relational
    database](#relational-database) or observations in a data frame.
    Tables are usually displayed as rows (each of which represents one
    record or observation) and columns (each of which represents a
    [field](#field) or variable.)

## Commands

See [this cheat sheet](files/sql-cheat-sheet.md) for an list of the commands
covered in this lesson.

### Keywords

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
            <td>SELECT *</td>
            <td>Selects the entire dataset</td>
        </tr>
        <tr>
            <td>SELECT column1</td>
            <td>Selects a particular column</td>
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
            <td>Joins tables</td>
            <td>SELECT * <br> FROM surveys <br> JOIN species <br> ON surveys.species_id = species.species_id</td>
            <td>Query will display all the columns from both tables where the condition is met</td>
        </tr>
    </tbody>
</table>


