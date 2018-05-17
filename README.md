# SQL BASICS
These are notes that I made about general SQL commands, which are used across various database management systems.

## Commands
A table is created with `CREATE TABLE your_table_name ( *column values written here* );`

Here is an example of creating a table with different column values:
```
CREATE TABLE your_table_name (  
  id INTEGER PRIMARY KEY,  
  column_name1 TEXT,  
  column_name2 TEXT UNIQUE,  
  column_name3 TEXT NOT NULL,  
  column_name4 TEXT DEFAULT ‘default_value’); 
  ```  

**id INTEGER PRIMARY KEY:** Creates a column called id, and each row in that column has a primary key - a unique number that identifies each row. Each row must be assigned a different key. Inserting a row with an identical key to a row already in the table will result in a constraint violation which will not allow you to insert the new row.

**TEXT UNIQUE:** These columns must have a different value every row. There can be multiple unique columns in a single table.  
**TEXT NOT NULL:** This column must have a value or it will result in a constraint violation.  
**TEXT DEFAULT:** This gives a row a default value if no value was specified in row creation.  
  
`INSERT INTO table_name (column1, column2, etc.) VALUES (value1, value2, etc.);`	
This adds a new row to the table. You specify the columns you wish to add values to, and what the values are.  

`SELECT * FROM table_name;`  
This selects everything from the table. Replace * with the column names or row IDs if you wish to be more specific. Separate them with commas.  

`UPDATE table_name SET column = value WHERE id = number;`  
This Updates the specified table, selects which column to change (Example: age = 22), and selects the row(s) to change the column value in.

`ALTER TABLE table_name ADD COLUMN column_name TEXT;`  
This alters the specified table and adds a column, and the data type is specified last. 

`DELETE FROM table_name WHERE column_name/id = name/number;`  
This deletes all columns or rows that have the column name or id number specified.

`DELETE FROM table_name WHERE column_name IS NULL;`  
This deletes all rows that have a null value in the specified column.
