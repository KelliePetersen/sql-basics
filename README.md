SQL BASICS
=====  
These are notes that I made about general SQL commands, which are used across various database management systems.  
There are also MySQL notes at the bottom of each section.  

## CONTENTS

[Commands](https://github.com/KelliePetersen/sql-basics#commands)  
[Operators](#operators)  
[Queries](#queries)  
[Aggregate Functions](#aggregate-functions)  
[Command Order](#command-order)  
[View Database Contents](#view-database-contents)  
[Multiple Tables](#multiple-tables)  
[Notes](#notes)  
[Other](#other)  

## COMMANDS
### Creating a Table
A table is created with `CREATE TABLE your_table_name ( *column values written here* );`  
Here is an example of creating a simple table with different column values:
```
CREATE TABLE Students (  
  id INTEGER,  
  name TEXT,  
  grade REAL,  
  username TEXT,  
  year_level INTEGER,  
  enrollment DATE
);  
  ```  
This creates a table containing the columns id, name, grade, username, year_level and enrollment. INTEGER, TEXT, REAL and DATE are *keywords*. The *keywords* specify the *data type* that each column will accept. 

### Data Types
**TEXT**: A simple text value, written in quotes.  
**INTEGER**: Any whole numbers.  
**REAL**: Numbers with decimal places.  
**DATE**: The date written in YYYY-MM-DD format, in quotes.  
A full list can be found here: [w3schools Data Types](https://www.w3schools.com/sql/sql_datatypes.asp).

### Keywords
Here is the example from earlier with additional keywords:
```
CREATE TABLE Students (  
  id INTEGER,  
  name TEXT,  
  grade REAL,  
  username TEXT UNIQUE,  
  year_level INTEGER NOT NULL,  
  enrollment DATE DEFAULT '2018-01-20');  
  ```  

**UNIQUE:** This means that the column must have a different value entered into it every row. Trying to create identical values in multiple rows would cause an error. There can be multiple unique columns in one table. Unlike Primary Key, *unique* does not work as an identifer.  
**NOT NULL:** This means that the column must have a value or it will result in a constraint violation.  
**DEFAULT dfvalue:** This means that if no value is entered, its default value will be whatever is written as dfvalue.  
  
> **MySQL**  
> **UNSIGNED:** This is used on INTEGER columns. It means that the column will not accept negative numbers as values. 
> **AUTO_INCREMENT:** This is used on INTEGER columns. It means that if its value is left blank when inserting a row, MySQL will automatically generate a unique identifier value. This value will be 1 greater than the maximum value in the column already.

### Primary Keys
Let's expand one line from the earlier example:
```
CREATE TABLE Students (  
  id INTEGER PRIMARY KEY, 
  ...
);
```
PRIMARY KEY gives each row in the column a *primary key* - a unique number that identifies each row. Each row must be assigned a different key and cannot be left blank. Inserting a row with a key that is identical to a row already in the table will result in a *constraint violation*, which will not allow you to insert the new row.  

Primary Keys are made differently in MySQL:
```
CREATE TABLE Students (
  id INTEGER,
  ...
  PRIMARY KEY(id)
);
```

### Foreign Keys
When the primary key appears in a different table, it is called a foreign key. A foreign key links two tables together.
This MySQL example links the *grade* column to another table:
```
CREATE TABLE Orders (
  ...
  grade REAL,
  ...
  FOREIGN KEY (grade) REFERENCES Grades(total)
); 
```
The *grade* column will get its value from the *total* column in the *Grades* table. 

### Inserting Data
`INSERT INTO table_name (column1, column2, etc.) VALUES (value1, value2, etc.);`  
This adds a new row to the table. You specify the columns you wish to add values to, and what the values are.  
For example:  
`INSERT INTO students (name, grade, year_level) VALUES ('Mike Smith', 75.5, 5);`  

Specifying columns is optional. If you do not write any column names, the values will be applied to each column in order.  
For example:
`INSERT INTO students VALUES (1, 'Mike Smith', 75.5, 'Mikey', 5, '2014-03-05');`  

Multiple rows can be inserted at once, by containing each row inside parentheses and separating them with commas.  

**MySQL**  
If a column is set to AUTO_INCREMENT and you want to take advantage of that, use NULL for its value when inserting. 
Set Priority by adding the following after the word INSERT:  
HIGH_PRIORITY:  
LOW_PRIORITY: The system will insert when data is not being read from the table.  
DELAYED: Inserted data will be buffered. If the server is busy, you can continue using queries without waiting for this operation to complete.  
Add these settings after INSERT and/or priority:  
IGNORE: If the inserted row(s) contain an already-used unique key, they will be silently skipped.  

### Selecting Data
`SELECT * FROM table_name;`  
This selects everything from the table. Replace * with the column names or row IDs if you wish to be more specific. Separate them with commas.  

### Updating Data
`UPDATE table_name SET column = value WHERE id = number;`  
This Updates the specified table, selects which column to change (Example: age = 22), and selects the row(s) to change the column value in.

### Adding Columns
`ALTER TABLE table_name ADD COLUMN column_name TEXT;`  
This alters the specified table and adds a column, and the data type is specified last. 

### Deleting Data
`DELETE FROM table_name WHERE column_name/id = name/number;`  
This deletes all columns or rows that have the column name or id number specified.

`DELETE FROM table_name WHERE column_name IS NULL;`  
This deletes all rows that have a null value in the specified column.

## OPERATORS

[SQL Operators](https://www.w3schools.com/sql/sql_operators.asp)  
[MySQL Operators](https://dev.mysql.com/doc/refman/5.5/en/non-typed-operators.html)  

## QUERIES													

**ALIAS:** `SELECT name AS alias_name FROM table_name;`  
This allows you to rename a column or table using an alias.

**DISTINCT:** `SELECT DISTINCT name FROM table_name;`  
This returns unique values in the output. It filters out all duplicate values in the specified column(s). 

**WHERE:** `SELECT * FROM table_name WHERE condition;`  
This returns the values that satisfy the condition. For example, return years later than (>) 1990.

**NULL:** `SELECT * FROM table_name WHERE name IS/IS NOT NULL;`  
**LIKE:** `SELECT * FROM table_name WHERE name LIKE ‘wildcard_pattern’;`  
This returns all values matching the wildcard pattern. A wildcard pattern is a word containing _ or %. For example, LIKE ‘n__e’ returns the values name and note. ‘A%’ and ‘%a’ return all values beginning with ‘A’ or ending with ‘a’. ‘%a%’ returns all values containing ‘a’. It is case insensitive. */

**IN:** `SELECT * FROM table_name WHERE column_name IN ( ‘column_value’);`  
This returns all the rows that have the specified column value in the specified column. 

**BETWEEN:** `SELECT * FROM table_name WHERE name BETWEEN start AND end;`  
This returns all values that are between the specified range. If the range is specified in numbers, it returns both start and end. If the range is specified in letters, it does not return the end letter. */

**AND:** `SELECT * FROM table_name WHERE name BETWEEN start AND end AND column = value;`
This combines multiple conditions.

**OR:** `SELECT * FROM table_name WHERE name BETWEEN start AND end OR column = value;`
This returns anything that matches any one of the specified conditions.

**NOT:** `SELECT * FROM table_name WHERE (not name = ‘value’) and (not name = ‘value’);`  
	`SELECT * FROM table_name WHERE NOT (name = ‘value’ or name = ‘value’);`  
This returns values that do not match the specified values. May also use “!=” instead of not.

**ORDER BY:** `SELECT * FROM table_name ORDER BY name ASC/DESC;`  
This orders all returned values in ascending (default value) or descending (Z-A, 10-1) order. 

**LIMIT:** `SELECT * FROM table_name LIMIT number;`  
This limits the amount of the selection to the specified number value. 

**CASE:** `SELECT name, CASE WHEN condition THEN string ELSE string END/END AS alias_name FROM table_name;`	 
This creates different outputs based on conditions. It is SQL’s way of handling if-then logic. There can be multiple WHEN conditions. Indent long CASE commands for readability.

## AGGREGATE FUNCTIONS										

**COUNT:** `SELECT COUNT(*) FROM table_name;`  
This returns the total number of rows in the table. 

**SUM:** `SELECT SUM(name) FROM table_name;`  
This returns the sum of all values in the column. 

**MIN/MAX:** `SELECT MIN/MAX(name) FROM table_name;`  
This returns the smallest or largest value. 

**AVG:** `SELECT AVG(name) FROM table_name;`  
This returns the average value in the column. 

**GROUP BY:** `SELECT name FROM movies GROUP BY name;`  
This groups all equal values into a single row or column.  

**HAVING:** `SELECT name FROM movies GROUP BY name HAVING condition;`  
This filters which groups to include and which to exclude. It’s like WHERE, but for groups.

**ROUND:** `SELECT ROUND(name, 0) FROM table_name;`  
This rounds the returned numbers of the specified column to the number of decimal points specified. It may contain other commands as the name value. For example, ROUND(AVG(name), 2). */

## COMMAND ORDER
When using aggregations, WHERE will always run before them, so use HAVING instead.  
```
SELECT * AS alias_name,
    CASE
        WHEN condition THEN string
        ELSE string
    END AS alias_name
FROM table_name
JOIN other_table_name
ON table.column = other_table.column
WHERE condition
GROUP BY name
HAVING condition
ORDER BY name
LIMIT number;
```

## VIEW DATABASE CONTENTS											

| | MySQL | PostgreSQL | SQLite |
|--|--|--|--|
| Table Names | show tables; | \dt | .tables |
| Table Information | describe table_name; | \d table_name | .schema table_name |
| Database Names | show databases; |

## MULTIPLE TABLES									

Information will often be spread out across multiple tables. If all information was contained in a single table, there would be a lot of repeated information. For example, a table that had a column for a person’s name, and had a row for purchased items, would list the person’s name multiple times for each unique item they purchased. This is why it’s important to learn how to handle multiple tables.  

**JOIN:** `SELECT * FROM table_name JOIN other_table_name ON table_name.column_name = other_table_name.column_name;`  

Combining multiple tables is called joining tables. The JOIN command combines the two specified tables. The ON command gives duplicate columns shared between each table the same name, to avoid ambiguity. JOIN can be made more specific by using table_name.column_name (e.g. person.age) for SELECT, instead of just using table_name or \*. Other Commands included, such as WHERE, will have to use table_name.column_name format. If the two tables have different information in the shared column_name, those rows won’t merge.  

**SIMPLE JOIN:** `SELECT * FROM table, other_table WHERE table.column = other_table.column;`  
A simplified version of the above JOIN command.

**LEFT JOIN:** `SELECT * FROM table LEFT JOIN other_table ON table.column = other_table.column`  
This allows us to keep the unmatched rows of two combined tables. The first table’s data is kept.  

**CROSS JOIN:** `SELECT table.column, other_table.column FROM table CROSS JOIN other_table;`  
This combines all rows of one table with all rows of another table. It does not require ON command.  

**UNION:** `SELECT * FROM table1 UNION SELECT * FROM table2;`  
This stacks one table on top of the other. The tables must have the same amount of columns, and must have the same data types in the same order.  

**WITH:** `WITH previous_query AS ( SELECT … ) SELECT * FROM previous_query ... ;`  
This creates a separate query functioning as a temporary table, which we may use in the final query.

## NOTES

* Aggregates and commands may contain and combine themselves and each other.

Numbers may be used to refer to SELECT columns in GROUP BY and ORDER BY commands.
For example, `SELECT name, age GROUP BY 2` groups the returned values by age.

## OTHER													
These commands vary depending on the database system.

**TRANSACTION:** `BEGIN TRANSACTION; <insert query here> END TRANSACTION;`
Place your SQL commands between the transaction tags. Increases performance for large databases. Transactions are good because they are self contained and can be rolled back.

**TRIGGER:** `CREATE TRIGGER event_name AFTER trigger BEGIN event; END;`  
Causes an event to occur whenever the event trigger is triggered. If the event adds something, use the NEW alias. For example, the event block could be table.id = NEW.id WHERE condition.

**ROLLBACK:** `Use ROLLBACK; after SQL commands to revert changes.`  
Tip: You can SQL commands to prevent updates, by having ROLLBACK as a TRIGGER event.

**VIEW:** `CREATE VIEW view_name AS SELECT * FROM <continue query>`  
This allows you to save a query as a new way to view a table. Views may be joined.
