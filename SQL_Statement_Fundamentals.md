# SQL Statement Fundamentals

The syntax shown in this section is applicable to any major SQL engine (e.g. MS SQL Server, MySQL, Oracle, etc…)

### **1-SELECT**
SELECT is the most common statement used, and it allows us to retrieve information from a table.

For specific column(s),<br>
``` sql
SELECT column_name FROM table_name
```
For all columns,<br>
``` sql
SELECT * FROM table_name
```

**Note:** In general it is not good practice to use an asterisk (*) in the SELECT statement if you don’t really need all columns.

### **Application**
To learn the first and last names of actors in the ***actor*** table,
``` sql
SELECT * FROM actor;
```
It will bring all columns as actor_id, first_name, last_name, last_update.

For the specific columns,
``` sql
SELECT first_name, last_name FROM actor;
```

**Note:** SQL keywords can be written in lowercase or uppercase. Additionally, a semicolon may or may not be used. By default, capitalizing keywords and using semicolons helps for readability.

### 2-SELECT DISTINCT 
Sometimes a table contains a column that has duplicate values, and you may find yourself in a situation where you only want to list the unique/distinct values. The DISTINCT keyword can be used to return only the distinct values in a column.

``` sql
SELECT DISTINCT column FROM table
```
You can also use parenthesis for clarity:

``` sql
SELECT DISTINCT(column) FROM table
```

### **Application**
To find out how many different release years there are in the ***film*** table,
``` sql
SELECT DISTINCT release_year FROM film;
```
or
``` sql
SELECT DISTINCT(release_year) FROM film;
```

### 3-COUNT
The COUNT function returns the number of input rows that match a specific condition of a query. We can apply COUNT on a specific column or just pass COUNT(*) , we will soon see this should return the same result.

``` sql
SELECT COUNT(name) FROM table;
```
Each column has the same number of rows.
``` sql
SELECT COUNT(name) FROM table;
SELECT COUNT(choice) FROM table;
SELECT COUNT(*) FROM table;
```
All return the same thing.

COUNT is much more useful when combined with other commands, such as DISTINCT. Imagine we wanted to know: How many unique films are there in the table?
``` sql
SELECT COUNT(DISTINCT name) FROM table;
```

### **Application**
To find out how many rows there are in the ***payment*** table
``` sql
SELECT COUNT(*) FROM payment;
```
To find out how many unique amounts are in the ***payment*** table
``` sql
SELECT COUNT(DISTINCT amount) FROM payment;
```

### 2-SELECT WHERE 
The WHERE statement allows us to specify conditions on columns for the rows to be returned.

Basic syntax example,
``` sql
SELECT column1, column2 FROM table WHERE conditions;
```












