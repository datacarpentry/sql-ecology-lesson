Q & A on Database Design (review if time)
-----------------------------------------

1. Order doesn't matter
2. Every row-column combination contains a single *atomic* value, i.e., not
   containing parts we might want to work with separately.
3. One field per type of information
4. No redundant information
     * Split into separate tables with one table per class of information
	 * Needs an identifier in common between tables – shared column - to
       reconnect (foreign key).


<a name="datatypes"></a> Data types
-----------------------------------

| Data type  | Description |
| :------------- | :------------- |
| CHARACTER(n)  | Character string. Fixed-length n  |
| VARCHAR(n) or CHARACTER VARYING(n) |	Character string. Variable length. Maximum length n |
| BINARY(n) |	Binary string. Fixed-length n |
| BOOLEAN	| Stores TRUE or FALSE values |
| VARBINARY(n) or BINARY VARYING(n) |	Binary string. Variable length. Maximum length n |
| INTEGER(p) |	Integer numerical (no decimal). |
| SMALLINT | 	Integer numerical (no decimal). |
| INTEGER |	Integer numerical (no decimal). |
| BIGINT |	Integer numerical (no decimal). |
| DECIMAL(p,s) |	Exact numerical, precision p, scale s. |
| NUMERIC(p,s) |	Exact numerical, precision p, scale s. (Same as DECIMAL) |
| FLOAT(p) |	Approximate numerical, mantissa precision p. A floating number in base 10 exponential notation. |
| REAL |	Approximate numerical |
| FLOAT |	Approximate numerical |
| DOUBLE PRECISION |	Approximate numerical |
| DATE |	Stores year, month, and day values |
| TIME |	Stores hour, minute, and second values |
| TIMESTAMP |	Stores year, month, day, hour, minute, and second values |
| INTERVAL |	Composed of a number of integer fields, representing a period of time, depending on the type of interval |
| ARRAY |	A set-length and ordered collection of elements |
| MULTISET | 	A variable-length and unordered collection of elements |
| XML |	Stores XML data |


<a name="datatypediffs"></a> SQL Data Type Quick Reference
----------------------------------------------------------

Different databases offer different choices for the data type definition.

The following table shows some of the common names of data types between the various database platforms:

| Data type |	Access |	SQLServer |	Oracle | MySQL | PostgreSQL |
| :------------- | :------------- | :---------------- | :----------------| :----------------| :---------------|
| boolean	| Yes/No |	Bit |	Byte |	N/A	| Boolean |
| integer	| Number (integer) | Int |	Number | Int / Integer	| Int / Integer |
| float	| Number (single)	|Float / Real |	Number |	Float |	Numeric
| currency | Currency |	Money |	N/A |	N/A	| Money |
| string (fixed) | N/A | Char |	Char | Char |	Char |
| string (variable)	| Text (<256) / Memo (65k+)	| Varchar |	Varchar / Varchar2 |	Varchar |	Varchar |
| binary object	OLE Object Memo	Binary (fixed up to 8K) | Varbinary (<8K) | Image (<2GB)	Long | Raw	Blob | Text	Binary | Varbinary |

