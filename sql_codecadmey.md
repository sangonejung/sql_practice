<!--Author: Sang One Jung -->
# Chapter 1: Introduction to SQL
## Introduction to Database
**Database**: A database is a set of data stored in a computer. This data is usually structured into _tables_ like that of an excel. Tables can grow large and have a multitude of columns and records. <br/><br/>
**Relational Database**:  A structure that allows users to identify and access data in relation to another piece of data in the database. Often, data in a relational database is organized into tables.<br/><br/>
**Database Schema**: A database schema is considered the “blueprint” of a database which describes how the data may relate to other tables or other data models <br/><br/>
**Database Instance**: The schema does not actually contain data. A sample of data from a database at a single moment in time is known as a database instance.<br/><br/>
**Query**: SQL allows the user to write _queries_ which define the subset of data for which the user is looking. Unlike excel, SQL will handle how to get the data. <br/><br/>
**Sample SQL Code**
```sql
SELECT *
FROM browse
LIMIT 10;
```
Select all (*) columns from the database (table) called _browse_ Limit to first 10 rows <br/>
```sql
SELECT user_id, browse_date
FROM browse
LIMIT 5;
```
Only query two columns: _user_id_ and _browse_date_ from _browse_ table up to first 5 rows
## SQL Use Cases Example
Consider the following three scenarios
* **Creating Usage Funnels**<br/>
Consider visitors of Codecademy's website. They will follow a simple workflow <br/>
1. Browse items available for sale
2. Click an icon to begin the checkout process
3. Enter payment information to complete their purchase <br/><br/>
Not all users who browse on the website will find something that they like enough to checkout, and not all users who begin the checkout process will finish entering their payment information to make a purchase.<br/><br/>
This type of multi-step process where some users leave at each step is called a __funnel__.

* **Analyzing User Churn**<br/>
A __churn rate__ is the percent of subscribers to a monthly service who have canceled. For example, in January, let’s say Codecademy has 1,000 customers. In February, 200 learners sign up, and 250 cancel.
$$\frac{\text{cancellations}}{\text{total subscribers}} = \frac{250}{1000 + 250} = 20.8\%$$

* Determining Web Traffic Attribution <br/>
 __UTM Parameters__ are special tags that site owners add to their pages to track what website a user was on before they reach the website <br/>
  1. If a user found Codecademy’s website through Google search, the table page_visits might have utm_source set to ‘google’.
  2. If a different user clicked a Facebook ad to get to Codecademy’s website, then their row in page_visits might have utm_source as ‘facebook’.
   
# Chapter 2: SQL Manipulation
## Database Index
A database index is a data structure that improves the speed of data retrieval operations on a database table at the cost of additional writes and storage space to maintain the index data structure. Simply put, it is a pointer to data in a table
## SQL Data Type
SQL Data Type Documentation https://www.w3schools.com/sql/sql_datatypes.asp <br/><br/>
Common Data Types in SQL
| Data Type    | Description |
| -------- | ------- |
| INTEGER  | A positive/negative whole number    |
| TEXT | A text string     |
| DATE    | Date formatted as YYYY-MM-DD    |
| REAL | A decimal value|

## SQL Statement
<!--
SQL Manipulation Cheat Sheet
https://www.codecademy.com/learn/paths/analyze-data-with-sql/tracks/analyze-data-sql-get-started-with-sql/modules/analyze-data-sql-learn-manipulation-c4b/cheatsheet
-->
A __statement__ is text that the database recognizes as a valid command, which ends in a semicolon. Consider the following statement
```sql
CREATE TABLE table_name (
   column_1 data_type, 
   column_2 data_type, 
   column_3 data_type
);
```
**Clause**: CREATE TABLE is a clause. Clauses perform specific tasks in SQL. By convention, clauses are written in capital letters. Clauses can also be referred to as commands. <br/><br/>
**table**: table_name refers to the name of the table that the command is applied to. <br/><br/>
**parameter**: (column_1 data_type, column_2 data_type, column_3 data_type) is a parameter. A parameter is a list of columns, data types, or values that are passed to a clause as an argument. Here, the parameter is a list of column names and the associated data type.
## CREATE
The __CREATE__ statement allows the user to create a new table in the database. Use the _CREATE_ statement to create a new table from scratch. The statement below creates a new table named celebs.
```sql
CREATE TABLE celebs (
   id INTEGER, 
   name TEXT, 
   age INTEGER
);
```
## Column Constraints 
Constraints that add information about how a column can be used are invoked after specifying the data type for a column. It is used to tell the database to reject inserted data that does not adhere to a certain restriction. <br/><br/>
Consider the following SQL code
```sql
CREATE TABLE celebs (
  id INTEGER PRIMARY KEY,
  name TEXT UNIQUE,
  date_of_birth TEXT NOT NULL,
  date_of_death TEXT DEFAULT 'Not Applicable'
);
```
__PRIMARY KEY__ The primary key is used as a unique identifier for each record in the table. <br/>
__UNIQUE__ The unique key is also a unique identifier for records when the primary key is not present in the table.<br/><br/>
__PRIMARY KEY__ vs __UNIQUE__<br/>
Primary Key cannot store NULL values in the but Unique key can store NULL values. Furthermore, Primary Key cannot be updated but an Unique Key can be updated<br/><br/>
__NOT NULL__ columns must have a value. Attempts to insert a row without a value for a NOT NULL column will result in a constraint violation and the new row will not be inserted. <br/>

__DEFAULT__ columns take an additional argument that will be the assumed value for an inserted row if the new row does not specify a value for that column.
```sql
CREATE TABLE celebs (
  col_name data_type constraints
);
```
## INSERT
The __INSERT__ statement inserts a new row into a table. Use the _INSERT_ statement to add new records. The statement below enters a record for Justin Bieber into the celebs table.
```sql
INSERT INTO celebs (id, name, age) 
VALUES (1, 'Justin Bieber', 22);
```
__INSERT INTO__ is a clause that adds the specified row or rows.
__VALUES__ is a clause that indicates the data being inserted.
```sql
-- Insert into columns in order:
INSERT INTO table_name
VALUES (value1, value2);

-- Insert into columns by name:
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);
```
## ALTER
The __ALTER TABLE__ statement adds a new column to a table. Use this command to add columns to a table. 
```sql
ALTER TABLE table_name 
ADD COLUMN new_col_name data_type;
```
__ALTER TABLE__ is a clause that allows to make specified changes.
__ADD COLUMN__ is a clause that adds a new column to a table <br/><br/>
The statement below adds a new column twitter_handle to the celebs table.
```sql
ALTER TABLE celebs 
ADD COLUMN twitter_handle TEXT;
```
When adding a new column, the value in each row is NULL
| id | name | age | twitter_handle|
| -- | -- | --| --|
| 1| Justin | 22 | NULL| 
| 2| Beyonce | 33 | NULL| 
| 3| Jeremy | 26 | NULL| 
| 4| Taylor | 36 | NULL| 

NULL is a special value in SQL that represents missing or unknown data. Here, the rows that existed before the column was added have NULL (∅)
## UPDATE
The __UPDATE__ statement edits a row in a table. Use the UPDATE statement to change existing records.
```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE some_column = some_value;
```
__UPDATE__ is a clause that edits a row in the table. <br/>
__SET__ is a clause that indicates the column to edit <br/>
__WHERE__ is a clause that indicates which row(s) to update with the new column value<br/><br/>
The statement below updates the record with an id value of 4 to have the twitter_handle @taylorswift13.
```sql
UPDATE celebs 
SET twitter_handle = '@taylorswift13' 
WHERE id = 4; 
```
| id | name | age | twitter_handle|
| -- | -- | --| --|
| 1| Justin | 22 | NULL| 
| 2| Beyonce | 33 | NULL| 
| 3| Jeremy | 26 | NULL| 
| 4| Taylor | 36 | @taylorswift13| 
## DELETE
The __DELETE FROM__ statement deletes one or more rows from a table. Use the statement to delete existing records.
```sql
DELETE FROM table_name
WHERE some_column = some_value;
```
__DELETE FROM__ is a clause that lets you delete rows from a table.
__IS NULL__ is a condition in SQL that returns true when the value is NULL and false otherwise. <br/><br/>
The statement below deletes all records in the celebs table with no twitter_handle
```sql
DELETE FROM celebs 
WHERE twitter_handle IS NULL;
```
| id | name | age | twitter_handle|
| -- | -- | --| --|
| 4| Taylor | 36 | @taylorswift13| 
# Chapter 3: SQL Queries
## AS
The columns or tables can be aliased using the __AS__ clause. This allows columns or tables to be specifically renamed in the returned result set.
```sql
SELECT col_name AS 'new_col_name', col_name2 AS 'new_col_name2'
FROM table_name
```
## DISTINCT
Unique values of a column can be selected using a __DISTINCT__ query. It filters duplicate entries.
```sql
SELECT DISTINCT col_name
FROM table_name
```
## LIKE (Wildcard)
The __LIKE__ operator can be used inside of a WHERE clause to match a specified pattern. The '%' wildcard can be used in a LIKE operator pattern to match any single unspecified character. 
* A% matches with names that begin with letter 'A'
* %A matches with names that end with 'A'
* %A% matches with names that contain 'A'

```sql
SELECT * 
FROM table_name 
WHERE col_name LIKE '%ABC%';
```
## BETWEEN
The __BETWEEN__ operator can be used to filter by a range of values. The range of values can be text, numbers, or date data.

* Number

```sql
SELECT *
FROM table_name
WHERE col_name BETWEEN num_1 AND num_2;
```
* Text
```sql
SELECT *
FROM table_name
WHERE col_name BETWEEN text_1 AND text_2;
```

## ORDER BY
The __ORDER BY__ clause can be used to sort the result set by a particular column either alphabetically or numerically. It can be ordered in two ways:
* __DESC__ is a keyword used to sort the results in descending order.
* __ASC__ is a keyword used to sort the results in ascending order (default).
```sql
SELECT *
FROM table_name
ORDER BY col_name DESC;
```
It is also possible to order by multiple times
```sql
SELECT *
FROM table_name
ORDER BY col_name1 ASC, col_name2 DESC;
```
## CASE
A __CASE__ statement allows us to create different outputs (usually in the SELECT statement).
```sql
SELECT col_name,
 CASE
  WHEN condition_1 THEN 'result_1'
  WHEN condition_2 THEN 'result_2'
  ELSE 'result_3'
 END AS 'new_col_name'
FROM table_name;
```
Condition needs either '>' or '<', or '='

# Chapter 4: SQL Aggregations
## Definition
SQL Queries don’t just access raw data, they can also perform calculations on the raw data to answer specific data questions.
Calculations performed on multiple rows of a table are called __aggregates__
## COUNT
The __COUNT()__ aggregate function returns the total number of rows that match the specified criteria

* __COUNT(*)__  counts every row
* __COUNT(col_name)__ does not count NULL values in that column.

Counts the total number of rows in table_name
```sql
SELECT COUNT(*)
FROM table_name;
```
Counts the number of rows of col_name that are not NULL 
```sql
SELECT COUNT(col_name)
FROM table_name;
```

## SUM
The __SUM()__ aggregate function takes the name of a column as an argument and returns the sum of all the value in that column. Ignores the NULL value and will be counted as 0

Sums the values of the col_name
```sql
SELECT SUM(col_name)
FROM table_name;
```
Sums the values of the col_name with the given condition
```sql
SELECT SUM(col_name)
FROM table_name
WHERE condition_1
```
## MAX/MIN
The __MAX()__ and __MIN()__ functions return the highest and lowest values in a column, respectively.

Maximum value of the col_name
```sql
SELECT MAX(col_name)
FROM table_name
```
Minimum value of the col_name 
```sql
SELECT MIN(col_name)
FROM table_name
```
## AVG
The __AVG()__ aggregate function returns the average value in a column.

Avg value of the col_name
```sql
SELECT AVG(col_name)
FROM table_name
```
## ROUND
The __ROUND()__ function will round a number value to a specified number of places. It takes two arguments: a number, and a number of decimal places. It can be combined with other aggregate functions

Round value of the col_name to the num_1 place
```sql
SELECT ROUND(col_name, num_1)
FROM table_name
```
Nested Aggregates such as the one below also works as well
```sql
SELECT ROUND(AVG(col_name), num_1)
FROM table_name
```
## GROUP BY
The __GROUP BY__ clause will group records in a result set by identical values in one or more columns. It is often used in combination with aggregate functions to query information of similar records. 

The __GROUP BY__ clause can come after __FROM__ or __WHERE__ but MUST come before any __ORDER BY__ or __LIMIT__ clause.

Assume aggregate() function can be any one of the aggregates we have learned so far. This will cluster the table by col_name
```sql
SELECT col_name, aggreagate(col_name) 
FROM table_name
GROUP BY col_name;
```
Again, assume aggregate() function can be any one of the aggregates we have learned so far. We can cluster within another clustered table
```sql
SELECT col_name, col_name2, aggregate(col_name3) 
FROM table_name
GROUP BY col_name, col_name2;
```
## HAVING
The __HAVING__ clause is used to further filter the result set groups provided by the GROUP BY clause. It is often used with aggregate functions to filter the result set groups based on an aggregate property. It is different from __WHERE__ clause because __HAVING__ clause is filtering the groups not the rows.

The __HAVING__ clause MUST always come after a __GROUP BY__ clause but MUST come before any __ORDER BY__ or __LIMIT__ clause.

Filters out that have condition1 from grouping
```sql
SELECT col_name, 
   aggregate(col_name2) 
FROM table_name
GROUP BY col_name
HAVING condition1;
```

# Chapter 5.1: SQL Multiple Tables
## INNER & FULL OUTER JOIN
The __INNER JOIN__ (or __JOIN__) clause allows $A \cap B$ by joining _common_ values specified using an __ON__ clause.

```sql
SELECT * 
FROM table_name1 
INNER JOIN table_name2  
  ON table_name1.column = table_name2.column;
```

The __FULL OUTER JOIN__ clause allows $A \cup B$ by joining _all_ values specified using an __ON__ clause. 

```sql
SELECT * 
FROM table_name1 
FULL OUTER JOIN table_name2  
  ON table_name1.column = table_name2.column;
```

## LEFT & RIGHT JOIN
 In a __LEFT JOIN__ and __RIGHT JOIN__, every row in the left table or the right table respectively is returned in the final table. Suppose the join condition is not met, then __NULL__ values are used to fill in the columns. 

 In the view of sets, we are joining by excluding either $A^c$ or $B^c$

__table_name1__ is the left table 
```sql
SELECT * 
FROM table_name1 
LEFT JOIN table_name2  
  ON table_name1.column = table_name2.column;
```
__table_name2__ is the right table 
```sql
SELECT * 
FROM table_name2 
RIGHT JOIN table_name1  
  ON table_name1.column = table_name2.column;
```

## PRIMARY KEY & FOREIGN KEY
__PRIMARY KEY__ uniquely identifies each row of that table for each table. 

Requirements for a __PRIMARY KEY__
* None of the values can be __NULL__
* Each value must be unique. No duplicates allowed
* A table cannot have more than one primary key column

__FOREIGN KEY__ is a primary key for one table that appears in a different table

## CROSS JOIN
The __CROSS JOIN__ clause is used to combine each row from one table with each row from another in the result set. This is helpful for creating all possible combinations for the records (rows) in two tables.

The following will give us a table such that there are total of $A \choose B$ rows. In other words, for each $A$ elements, we have $B$ elements. 
```sql
SELECT * 
FROM table_name1 
CROSS JOIN table_name2  
```

__CROSS JOIN__ with a __WHERE__ clause will produce the same result as the __INNER JOIN__ clause
```sql
SELECT * 
FROM table_name1 
CROSS JOIN table_name2  
WHERE condition1
```
## UNION
The __UNION__ clause is used to combine results that appear from multiple SELECT statements and filter duplicates.

Requirements for __UNION__
* Tables must have the same number of columns.
* The columns must have the same data types in the same order as the first table.
```sql
SELECT *
FROM table_name1
UNION
SELECT *
FROM table_name2
```

Difference between __UNION__ and __FULL OUTER JOIN__ is that __UNION__ simply add rows to the table whereas the __JOIN__ clauses merges columns

## WITH
The __WITH__ clause stores the result of a query in a temporary table using an alias

```sql
WITH temp_table1 AS (
   SELECT *
   FROM table_name1
)
SELECT *
FROM temp_table1
--More Query Statements Go Here
```

## SQL Subquery
Here

<!-- # Chapter 6: Analyzing Real Data with SQL
## Usage Funnel
## User Churn
## Marketing Attribution -->

# Chapter 6.1: SQL Window Function
## Introduction
## PARTITION BY
## FIRST_VALUE & LAST_VALUE
## LAG
## LEAD
## ROW_NUMBER
## RANK
## NTILE
# Chapter 6.2: SQL Math and Date Functions
