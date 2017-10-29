# SQL - notes, commands, and code samples
-----------------------
# Summary

This document contains my personal notes and code samples related to general SQL syntax and related SQL engines (currently just Postgres and the `psql` command-line tool).

__Please note, I am aggregating these notes from various personal sources (some old, some new), and still editing, organizing, and formatting them for presentation in this Markdown document.__

# Contents
-

## DATABASES & TABLES

### DATA TYPES
__PostgreSQL supports the following Data Types__

#### 1. Boolean
  * Data inserted into a boolean column will be converted into the boolean value (e.g. 1, yes, y, t, true are converted to true, and 0, no, n, false, f coverted to false)
  * ‘boolean’ or ‘bool’ are keywords to declare a column of the boolean type

#### 2. Character
  * 3 types of characters
      * single character: char
      * fixed length character string: char(n)
      * variable length character string: varchar(n)

#### 3. Number
      * Two types of numbers
          1. Integers
              * smallint: 2-byte singed integer of range (-32768, 32767)
              * int: 4-byte has range of (-214783648, 214783647)
              * serial: same as integer but PostgreSQL populates value into column automatically
                  * this is similar to the AUTO-INCREMENT attribute in other DB mgmt systems
          2. Floating Point Numbers
              * float(n): precision, at least, n, up to 8 bytes
              * real or float8: double-precision (8-byte) floating point number
              * numeric or numeric(p,s): real number with p digits and s number after decimal point
                  * numeric(p,) is the exact number
  4. Temporal (i.e. data and time-related data types)
      * date - stores date data
      * time - stores time data
      * timestamp - stores date and time
      * interval - stores the difference in timestamps
      * timestamptz - includes timezone data
  5. Special Types
  6. Array

## PRIMARY vs. FOREIGN KEYS

### Primary Key

```sql
/*
- each table can have only one primary key
- good practice to add a primary key to every table
*/

CREATE TABLE table_name (
  "col_name"   data_type PRIMARY KEY,
  "col_name"   data_type,...
)

-- Or, for multi-column primary keys...

CREATE TABLE table_name (
  "col_name1"  data_type NOT NULL,
  "col_name2"  data_Type NOT NULL,
  "col_name3"  data_type,
  PRIMARY KEY ("col_name1", "col_name2")
);

-- Or, with associated constrain variables...

CREATE TABLE table_name (
  "col_name1"  data_type NOT NULL,
  "col_name2"  data_Type,
  CONSTRAINT constraint_name  -- e.g. table_name_pk
    PRIMARY KEY ("col_name1")
);
```

### Foreign Key

```sql
/*  
- Defined in a table that refers to the primary key
  of the other table
- A table can have multiple foreign keys
- Table that contains foreign key is called the
  reference table or child table
- Table that foreign key references is called the
  referenced or parent table.
- A ForeignKey constraint maintains referential
  integrity between child and parent tables
*/

CREATE TABLE table_name (
  "col_name1"  data_type NOT NULL,
  "col_name2"  data_Type NOT NULL,
  CONSTRAINT constraint_name1  -- e.g. table_name_pk
    PRIMARY KEY ("col_name1")
  CONSTRAINT constraint_name2 -- e.g. table_name_fk0
    FOREIGN KEY ("col_name2")
    REFERENCES "other_table_name"("col_name")
);
```

## CREATE TABLE
```sql
CREATE TABLE table_name (
  "col_name" TYPE col_constraint,
  table_constraint
)
  INHERITS existing_table_name;

-- INHERIT is optional
  -- It creates all columns of existing table plus new

-- Shortcut for copying table structure into a new table...
CREATE TABLE table_name (
  LIKE existing_table_name
);

-- COLUMN CONSTRAINTS...
NOT NULL
  -- value of column cannot be NULL
UNIQUE
  -- however, can have many NULL values
  -- NULL value is treated as unique by PostgreSQL
PRIMARY KEY
  -- combination of NOT NULL and UNIQUE constraints
CHECK
  -- enables to check condition when you insert or update data
  -- (e.g. checking prices to be positive)
REFERENCES
  -- constrains the value of column that exists
    -- in a column of another table
```

## INSERT
```sql
/*
Allows you to insert one or more rows into a
table at a time
*/

INSERT INTO table( col1, col2, ... )
VALUES  (val1, val2, ... ),
        (val1, val2, ... ),
        ... ;

-- Each set of values represent a separate
--    row matching stated columns

-- INSERT FROM ANOTHER TABLE...
INSERT INTO table_name
SELECT col1, col2, ...
FROM other_table_name
WHERE condition;
```

## UPDATE
```sql
/*
Used to update existing data in a table
*/

UPDATE table_name
SET col1 = val1,
    col2 = val2,
    ...
WHERE condition;

-- Can update based on value in another column by specifying...
SET col1 = col2

-- Can also view affected rows by ending query with..

RETURNING col1, col2, ... ;
```
## DELETE
```sql
/*
Deletes row in a table
*/

DELETE FROM table
WHERE condition;

/*
NOTE: If you omit the WHERE clause it will
delete ALL the rows in the table
*/
```
## ALTER TABLE
```sql
-- Changes existing table structure

ALTER TABLE table_name action_statement;

-- Action Statements include..
ADD COLUMN
DROP COLUMN
RENAME COLUMN
ADD CONSTRAINT
RENAME TO
```
## DROP TABLE
```sql
-- Removes existing table from the database...
DROP TABLE [ IF EXISTS ] table_name;
-- "IF EXISTS" prevents error if table does not exist

-- Add RESTRICT to prevent DROP TABLE if other tables are dependent...
DROP TABLE IF EXITS table_name RESTRICT
-- PostgreSQL does this be default.

-- add CASCADE to drop table with dependencies...
DROP TABLE IF EXITS table_name CASCADE
```
## CHECK
```sql
/*
A kind of constraint that allows you to specify
if a value in a column must meet a specific requirement

  - Uses a boolean expression to evaluate the values
    of a column
  - If values pass the check, PostgreSQL will insert
    and update those values
  - If you attempt to insert a value that violates the
    check, PostgreSQL will give an error outlining what
    CHECK was violated
*/
CREATE TABLE table_name(
  col1 TYPE CONSTRAINT constraint_name CHECK(col1 condition);
/*
Keyword CONSTRAINT names the constraint that you
are checking so that it creates a more readable
error message.

  - The constraint name can be omitted if not desired
*/
```
## NOT, NOT NULL, UNIQUE Constraints

NULL is an unknown or missing value
  - this is different from empty or zero

NOT NULL forces column not to accept the NULL value

UNIQUE Constraint...
  - Everytime you insert a new role, PostgreSQL will check if the value is already in the table.
  - If found that it is already there, it will give an error message and reject changes

## VIEWS
```sql
/*
- A Postgres view is a logical table that represents
  data of one or more underlying tables through a
  select statement
- Simplifies the complexity of a query because you
  can query a VIEW, which is based on a complex query
- Like a table, you can grant permission to users
  through a view that contains specific data the user
  is authorized to see
- A view provides a consistent layer even if the
  columns of the underlying table changes
*/

CREATE VIEW view_name AS
SELECT ...; -- query content goes here

-- Alter an existing view...
ALTER VIEW view_name RENAME view_new_name;
-- Delete a view...
DROP VIEW IF EXISTS view_name;
-- To view the view table...
SELECT * FROM view_name;
```

## SQL QUERY STATEMENTS

### SELECT

```sql
SELECT column1, column2, ... FROM table_name;
-- ‘ * ‘ is used as the shorthand for all
--    columns in the specified table.
```
#### SELECT DISTINCT

```sql
-- A column may contain duplicate values
-- You may want to just grab distinct values with...
SELECT DISTINCT column1, column2, ... FROM table_name;
```
#### SELECT WHERE

```sql
-- To query rows matching ONLY specific conditions...
SELECT col1, col2, ...
FROM table_name
WHERE condition1 AND condition2;

-- Basic operators are...
=, > ,<, >=, <=, <> or !=, AND, OR
```

### COUNT

```sql
-- Returns number of rows matching specific condition(s)...
SELECT COUNT(*) FROM table;

-- Does not consider null values in a column...
SELECT COUNT(column) FROM table;

SELECT COUNT(DISTINCT column) FROM table;
-- Can also use two sets of parentheses for readability...
SELECT COUNT(DISTINCT (column)) FROM table;
```

### LIMIT

```sql
-- Limits the number of rows you get back after a query
-- Useful when wanting all columns BUT NOT all rows
-- Goes at the end of a query

SELECT col_name1, col_name2
FROM table_name
LIMIT #_desired;
```

### ORDER BY

```sql
-- Sorts your result set
    -- Instead of appearing in order rows were created
-- Ascending, descending or by some other criteria
-- Ascending is default if not specified...
SELECT col1, col2
FROM table
ORDER BY col1 ASC,
col2 DESC;

-- Postgres ALSO ALLOWS sorting by columns not even listed
    -- Other SQL engines may not allow this
    -- Good practice it to SELECT column you want to sort
        -- This ensures query works in other SQL DBs
```
### BETWEEN

```sql
-- Matches a value against a range of values:

WHERE col BETWEEN low AND high
-- ...is equiv. to >=low and <=high

WHERE col NOT BETWEEN low AND high
```

### IN

```sql
-- Used with WHERE clause
-- Checks if value matches any value in a list of values...
WHERE col IN ( value1, value2, ... );

-- Can also have sub-query...
WHERE col IN ( SELECT value FROM tbl_name );

-- Can also use NOT operator...
WHERE col NOT IN ( value1, value2, ... );

-- IN is the same as writing...
WHERE col = value1
OR col = value2
OR col = ...;
```
### LIKE

```sql
-- Matches pattern to find something ‘like’ a given value
LIKE    -- is Case Sensitive
ILIKE   -- is Case Insensitive

-- Wild card characters are:
  %   -- Matches any sequence ('pattern') of characters
      -- can be used at beginning/middle/end of a sequence
      '%value'
      '%value%'
      'value%'
  _   -- Matches any single character

-- Example of LIKE with 'pattern' wild card...
WHERE col LIKE ‘value%’;
```

### MIN, MAX, AVG, SUM aggregate functions
```sql
-- Combines everything into a single value
-- Commonly used with GROUP BY statement

-- Example without GROUP BY statement...
SELECT AVG( col ) FROM table;
-- To round the final answer to 2 decimal places...
SELECT ROUND( AVG( col ) , 2 ) FROM table;
```

### GROUP BY
```sql
-- Divides rows returned from the SELECT into groups
-- For each group, can apply an aggregate function, e.g.:
    -- sum of items
    -- count of number of items in groups
SELECT col1, AGG_FUNCT( col2 )
FROM table
GROUP BY col1
ORDER BY AGG_FUNCT(col2);

-- Without aggregate function, GROUP BY acts like DISTINCT
-- Postgres GROUP BY is more flexible than other engines
    -- Others may require you SELECT the GROUP BY column
    -- Or they may require an aggregate function
```

* HAVING
    * Often used with GROUP BY clause to filter group rows that do not satisfy a specified condition
    * It’s like a WHERE statement, but you’re using it with the GROUP BY clause
    * HAVING sets the condition for group rows created by GROUP BY after the GROUP BY clause applies
    * WHERE sets the condition for individual rows before the GROUP BY clause applies.
        * SELECT col1, AGG_FUNC( col2 )
FROM table
GROUP BY col1
HAVING AGG_FUNC(col2) condition;
* AS
    * Allows you to rename columns or table selections with an alias
        * SELECT col_name AS col_alias
FROM table;
    * It is also useful with GROUP BY
        * SELECT col1, SUM( col2 ) AS total_spent
FROM table
GROUP BY col1;
    * Can be used to shorten table names to speed up typing/shorten queries for JOIN statements
        * SELECT A.pka, A.c1, B_name.pkb, B_name.c2 AS c2_alias
FROM A
INNER JOIN B AS B_alias ON A.pka = B_alias.fka;
    * Additionally, AS statement can even be removed (leaving just a space) and the query will still work as expected
        * SELECT A.pka, A.c1, B_name.pkb, B_name.c2 c2_alias
FROM A
INNER JOIN B B_alias ON A.pka = B_alias.fka;
* JOINS
        * Allows you to relate data in one table to the data in other tables
        * Example with columns from tables A and B wherein there is a primary key (pk...) for each table and a foreign key (fk...) that links to the other table’s primary key
            * SELECT A.pka, A.c1, B.pkb, B.c2
FROM A
INNER JOIN B ON A.pka = B.fka;
        * In the above example we select A as our ‘main’ table and then we join B onto A, matching A.pka to B.fka
        * You do not need to specify the table name for a given column, if that column name only exists in one table
        * Can use WHERE and ORDER BY just like you would in a typical query at the end of the SQL query
    * INNER JOIN
        * This clause returns rows in A table that have corresponding rows in B table
        * Just writing JOIN defaults to INNER JOIN
        * This is the most common JOIN type to use
    * FULL OUTER JOIN
        * produces the set of all records in Table A and Table B, with matching records from both sides where available
            * If there is no match, the missing side will contain null.
        * To produce the set of records unique to Table A and Table B, you can use a WHERE clause
            * SELECT A.pka, A.c1, B.pkb, B.c2
FROM A
FULL OUTER JOIN B ON A.pka = B.fka
WHERE B.c2 IS null
OR A.c1 IS NULL;
    * LEFT OUTER JOIN
        * produces a complete set of records from Table A with the matching records (where available) in Table B.
            * If there is no match the right side will contain a null
        * using a WHERE clause excludes the records you don’t want from the right side
        * Can also just write LEFT JOIN and SQL will know you mean left outer join
            * Example
                * SELECT A.pka, A.c1, B.pkb, B.c2
FROM A
LEFT OUTER JOIN B ON A.pka = B.fka
WHERE B.c2 IS null;
    * RIGHT OUTER JOIN
        * similar to Left outer join but opposite
    * FULL OUTER JOIN
        * Produces the set of records unique to Table A an Table B
* UNION
    * This operator combines result sets of two or more SELECT statements into a single result set.
        * SELECT col1, col2
FROM table1
UNION
SELECT col1, col2
FROM table2
    * Two Rules for UNION
        * Both queries must return the same number of columns
        * The corresponding columns in the queries must have compatible data types
    * UNION operator removes all duplicate rows unless the UNION ALL is used
    * UNION may place rows in the first query before, after, or between the rows in the result set of the second query
        * To sort rows in the combined set, you use the ORDER BY clause
* TIMESTAMPS
    * Date Time functions may vary based on SQL engine
    * PostgreSQL Date/Time Types Documentation:
https://www.postgresql.org/docs/9.1/static/datatype-datetime.html
    * Example:
        * SELECT extract(day from col1)
FROM table;
* MATHEMATICAL FUNCTIONS
    * Documentation can be found at:
https://www.postgresql.org/docs/9.5/static/functions-math.html
* STRING FUNCTIONS
    * Documentation can be found at:
https://www.postgresql.org/docs/current/static/functions-string.html
    * Example with simple concatenation:
        * SELECT first_name || ' ' || last_name AS full_name
FROM customer;
* SUB-QUERIES
    * A query within a query
    * Example:
        * SELECT title, rental_rate
FROM film
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film);
    * More complicated example
        * SELECT film_id, title
FROM film
WHERE film_id IN
(SELECT inventory.film_id
FROM rental
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
WHERE
return_date BETWEEN '2005-05-29' AND '2005-05-30');
* SELF JOIN
    * When you want to combine rows with other rows in the same table
    * You must use a table alias to help SQL distinguish the left table from the right table of the same table
    * Self joins are often popular SQL interview question
        * google example for “manager employee self join"
    * Example:
        * SELECT e1.employee_name
FROM employee AS e1, employee AS e2
WHERE
e1.employee_location = e2.employee_location
AND e2.employee_name = ‘Joe’;
    * Alternate approach, same result:
        * SELECT e1.employee_name
FROM employee AS e1
JOIN  employee AS e2
ON e1.employee_location = e2.employee_location
AND e2.employee_name = ‘Joe’;
    * With OUTER JOIN
        * SELECT e1.employee_name
FROM employee AS e1
LEFT OUTER JOIN  employee AS e2
ON e1.employee_location = e2.employee_location
AND e2.employee_name = ‘Joe’;

PostgreSQL with PYTHON

*
