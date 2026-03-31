---
title: SQL Cheat Sheet
---

Basic Queries
-------------

Select one or more columns of data from a table:

    SELECT column_name_1, column_name_2 FROM table_name;

Select all of the columns in a table:

    SELECT * FROM table_name;

Get only unique lines in a query:

    SELECT DISTINCT column_name FROM table_name;

Perform calculations in a query:

    SELECT column_name_1, ROUND(column_name_2 / 1000.0) FROM table_name;


Filtering
---------

Select only the data meeting certain criteria:

    SELECT * FROM table_name WHERE column_name = 'Hello World';

Combine conditions:

    SELECT * FROM table_name WHERE (column_name_1 >= 1000) AND (column_name_2 = 'A' OR column_name_2 = 'B');


Sorting
-------

Sort results using `ASC` for ascending order or `DESC` for descending order:

    SELECT * FROM table_name ORDER BY column_name_1 ASC, column_name_2 DESC;

Data Types across Database Platforms
------------------------------------

Different database systems use different names for data types. Please refer to the documentation of your specific database software.

| Data type                                             | Description                                                                                              |
| ----------------------------------------------------- | :------------------------------------------------------------------------------------------------------- |
| CHARACTER(n) or CHAR(n)                               | Character string. Fixed-length n                                                                         |
| VARCHAR(n) or CHARACTER VARYING(n)                    | Character string. Variable length. Maximum length n                                                      |
| BINARY(n)                                             | Binary string. Fixed-length n                                                                            |
| BOOLEAN                                               | Stores TRUE or FALSE values                                                                              |
| VARBINARY(n) or BINARY VARYING(n)                     | Binary string. Variable length. Maximum length n                                                         |
| INTEGER                                               | Integer numerical (no decimal)                                                                           |
| SMALLINT                                              | Integer numerical (no decimal)                                                                           |
| INTEGER                                               | Integer numerical (no decimal)                                                                           |
| BIGINT                                                | Integer numerical (no decimal)                                                                           |
| NUMERIC(p,s) or DECIMAL(p,s) or NUMBER(p,s)           | Exact numerical, precision p (significant digits), scale s (digits in the fraction part).                |
| FLOAT(p)                                              | Approximate numerical (a floating point number) with p mantissa precision bits                           |
| REAL                                                  | Approximate numerical, same as FLOAT(p) but p will depend on the database system                         |
| FLOAT                                                 | Approximate numerical, using the default value of p for the database system                              |
| DOUBLE PRECISION                                      | Approximate numerical                                                                                    |
| DATE                                                  | Stores year, month, and day values                                                                       |
| TIME                                                  | Stores hour, minute, and second values                                                                   |
| DATETIME or TIMESTAMP                                 | Stores year, month, day, hour, minute, and second values                                                 |
| INTERVAL                                              | Composed of a number of integer fields, representing a period of time, depending on the type of interval |
| ARRAY                                                 | A set-length and ordered collection of elements                                                          |
| MULTISET                                              | A variable-length and unordered collection of elements                                                   |
| XML                                                   | Stores XML data                                                                                          |

The following table shows some of the common names of data types used with various database platforms:

| Data type           | SQLite  | Access (not SQL)               | SQLServer                     | Oracle           | MySQL         | PostgreSQL    |
| :-------------------| :-------| :----------------------------- | :---------------------------- | :--------------- | :------------ | :-------------|
| boolean             | INTEGER | Yes/No                         | BIT                           | INTEGER          | INTEGER       | BOOLEAN       |
| integer             | INTEGER | Number (Integer)               | INT                           | NUMBER           | INT / INTEGER | INTEGER       |
| float               | REAL    | Number (Single / Double)       | FLOAT / REAL                  | NUMBER           | FLOAT         | NUMERIC /REAL |
| currency            | REAL    | Currency                       | MONEY                         | N/A              | N/A           | MONEY         |
| string (fixed)      | TEXT    | N/A                            | CHAR                          | CHAR             | CHAR          | CHAR          |
| string (variable)   | TEXT    | Short (\<255 char) / Long Text | VARCHAR                       | VARCHAR2         | VARCHAR       | VARCHAR       |
| binary data         | BLOB    | Attachment                     | BINARY / IMAGE (\<2GB)        | RAW / BLOB       | BLOB / BINARY | BYTEA         |

Binary data is data with no specfic type (BLOB stands for _Binary Large OBject_) and is typically stored in the database exactly as given by the user.

Further information can be found at these references:
- [SQLite](https://sqlite.org/datatype3.html)
- [SQLServer](https://learn.microsoft.com/en-us/sql/t-sql/data-types/data-types-transact-sql)
- [Oracle](https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlqr/Data-Types.html)
- [MySQL](https://dev.mysql.com/doc/refman/9.6/en/data-types.html)
- [PostgreSQL](https://www.postgresql.org/docs/current/datatype.html)


Missing Data
------------

Use `NULL` to represent missing data.

`NULL` is neither true nor false.
Operations involving `NULL` produce `NULL`, e.g., `1+NULL`, `2>NULL`, and `3=NULL` are all `NULL`.

Test whether a value is null:

    SELECT * FROM table_name WHERE column_name IS NULL;

Test whether a value is not null:

    SELECT * FROM table_name WHERE column_name IS NOT NULL;


Grouping and Aggregation
------------------------

Combine data into groups and calculate combined values in groups:

    SELECT column_name_1, SUM(column_name_2), COUNT(*) FROM table_name GROUP BY column_name_1;


Joins
-----

Join data from two tables:

    SELECT * FROM table_name_1 JOIN table_name_2 ON table_name_1.column_name = table_name_2.column_name;


Combining Commands
------------------

SQL commands must be combined in the following order:
`SELECT`, `FROM`, `JOIN`, `ON`, `WHERE`, `GROUP BY`, `ORDER BY`.


Creating Tables
---------------

Create tables by specifying column names and types.
Include primary and foreign key relationships and other constraints.

    CREATE TABLE survey(
            taken   INTEGER NOT NULL,
            person  TEXT,
            quant   REAL NOT NULL,
            PRIMARY KEY(taken, quant),
            FOREIGN KEY(person) REFERENCES person(ident)
    );

Transactions
------------

Put multiple queries in a transaction to ensure they are ACID
(atomic, consistent, isolated, and durable):

    BEGIN TRANSACTION;
      DELETE FROM table_name_1 WHERE condition;
      INSERT INTO table_name_2 values(...);
    END TRANSACTION;

Programming
-----------

Execute queries in a general-purpose programming language by:

* loading the appropriate library
* creating a connection
* creating a cursor
* repeatedly:
    * execute a query
    * fetch some or all results
* disposing of the cursor
* closing the connection

Python example:

    import sqlite3
    connection = sqlite3.connect("database_name")
    cursor = connection.cursor()
    cursor.execute("...query...")
    for r in cursor.fetchall():
        ...process result r...
    cursor.close()
    connection.close()

