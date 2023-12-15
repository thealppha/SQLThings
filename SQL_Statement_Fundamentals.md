# SQL Statement Fundamentals

The syntax shown in this section is applicable to any major SQL engine (e.g. MS SQL Server, MySQL, Oracle, etc…)

## **1-SELECT**
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

## 2-SELECT DISTINCT 
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

## 3-COUNT
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
To find out how many rows there are in the ***payment*** table,
``` sql
SELECT COUNT(*) FROM payment;
```
To find out how many unique amounts are in the ***payment*** table,
``` sql
SELECT COUNT(DISTINCT amount) FROM payment;
```

## 4-SELECT WHERE 
The WHERE statement allows us to specify conditions on columns for the rows to be returned.

Basic syntax example,
``` sql
SELECT column1, column2 FROM table WHERE conditions;
```
The WHERE clause appears immediately after the FROM clause of the SELECT statement. The conditions are used to filter the rows returned from the SELECT statement.

PostgreSQL provides a variety of standard operators to construct the conditions:
* Comparison Operators: Compare a column value to something.
  * Is the price greater than $3.00?
  * Is the pet’s name equal to “Sam”?

* Logical Operators: Allow us to combine multiple comparison operators
  * AND
  * OR
  * NOT

### **Application**
To find out whose first name is Jared in the Customer table,
``` sql
SELECT * FROM customer WHERE first_name='Jared';
```
To find out which movies have a rental rate greater than 4 and a replacement cost greater than 19.99 in the movie table,
``` sql
SELECT title FROM film WHERE rental_rate>4 AND replacement_cost>19.99;
```
To find out the number of movies with a rating of R or PG-13 in the movie table,
``` sql
SELECT COUNT(*) FROM film WHERE rating='R' OR rating='PG-13';
```

## 5-ORDER BY 
You may have noticed PostgreSQL sometimes returns the same request query results in a different order. You can use ORDER BY to sort rows based on a column value, in either ascending or descending order.

Basic syntax for ORDER BY,
``` sql
SELECT column_1,column_2 FROM table ORDER BY column_1 ASC/DESC;
```
Use ASC to sort in ascending order. Use DESC to sort in descending order. If you leave it blank, ORDER BY uses ASC by default.

### **Application-1** 
Define table
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 101       | John      | Smith     | 25        |
| 102       | Alice     | Johnson   | 30        |
| 103       | Bob       | Brown     | 28        |
| 104       | Alice     | Adams     | 22        |
| 105       | John      | Doe       | 27        |

``` sql
SELECT * FROM table ORDER BY column_2 DESC;
```
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 102       | Alice     | Johnson   | 30        |
| 104       | Alice     | Adams     | 22        |
| 103       | Bob       | Brown     | 28        |
| 105       | John      | Doe       | 27        |
| 101       | John      | Smith     | 25        |

You can also ORDER BY multiple columns. This makes sense when one column has duplicate entries.
``` sql
SELECT column_1,column_2 FROM table ORDER BY column_1 ASC/DESC, column_2 ASC/DESC;
```
In this command, we order by column_1 and then order by column_2 on the specified preference(ASC / DESC).

### **Application-2** 
Define table
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 101       | John      | Smith     | 25        |
| 102       | Alice     | Johnson   | 30        |
| 103       | Bob       | Brown     | 28        |
| 104       | Alice     | Adams     | 22        |
| 105       | John      | Doe       | 27        |

``` sql
ORDER BY column_2 ASC
```
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 102       | Alice     | Johnson   | 30        |
| 104       | Alice     | Adams     | 22        |
| 103       | Bob       | Brown     | 28        |
| 105       | John      | Doe       | 27        |
| 101       | John      | Smith     | 25        |

``` sql
ORDER BY column_4 ASC
```
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 104       | Alice     | Adams     | 22        |
| 102       | Alice     | Johnson   | 30        |
| 103       | Bob       | Brown     | 28        |
| 105       | John      | Doe       | 27        |
| 101       | John      | Smith     | 25        |

## 6-LIMIT
The LIMIT command allows us to limit the number of rows returned for a query. Useful for not wanting to return every single row in a table, but only view the top few rows to get an idea of the table layout.

LIMIT goes at the very end of a query request and is the last command to be executed.

### **Application** 
Define table
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 101       | John      | Smith     | 25        |
| 102       | Alice     | Johnson   | 30        |
| 103       | Bob       | Brown     | 28        |
| 104       | Alice     | Adams     | 22        |
| 105       | John      | Doe       | 27        |

``` sql
SELECT * FROM example_table ORDER BY column_2 ASC, column_4 ASC LIMIT 3;
```
| column_1 | column_2 | column_3 | column_4 |
|-----------|-----------|-----------|-----------|
| 104       | Alice     | Adams     | 22        |
| 102       | Alice     | Johnson   | 30        |
| 103       | Bob       | Brown     | 28        |

## 7-BETWEEN
The BETWEEN operator can be used to match a value against a range of values BETWEEN low AND high. The BETWEEN operator is the same as value >= low AND value <= high.

You can also combine BETWEEN with the NOT logical operator value NOT BETWEEN low AND high. The NOT BETWEEN operator is the same as value < low OR value > high.

The BETWEEN operator can also be used with dates. Note that you need to format dates in the ISO 8601 standard format, which is YYYY-MM-DD date BETWEEN ‘2007-01-01’ AND ‘2007-02-01’.

### **Application** 
Define table
| column_1 | column_2 | column_3 | column_4   |
|-----------|-----------|-----------|------------|
| 101       | John      | 25        | 2005-07-12 |
| 102       | Alice     | 30        | 2003-04-05 |
| 103       | Bob       | 28        | 2009-11-23 |
| 104       | Alice     | 22        | 2002-09-15 |
| 105       | John      | 27        | 2006-08-04 |

``` sql
SELECT * FROM table WHERE column_3 BETWEEN 26 AND 30;
```
| column_1 | column_2 | column_3 | column_4   |
|-----------|-----------|-----------|------------|
| 101       | John      | 28        | 2005-07-12 |
| 102       | Alice     | 29        | 2003-04-05 |
| 103       | Bob       | 27        | 2009-11-23 |
| 104       | Alice     | 26        | 2002-09-15 |

``` sql
SELECT * FROM table WHERE column_4 BETWEEN '2007-01-01' AND '2010-12-31';
```
| column_1 | column_2 | column_3 | column_4   |
|-----------|-----------|-----------|------------|
| 101       | John      | 28        | 2007-02-15 |
| 102       | Alice     | 30        | 2009-08-20 |
| 105       | John      | 27        | 2008-11-30 |

## 8-IN
In certain cases you want to check for multiple possible value options, for example, if a user’s name shows up IN a list of known names. We can use the IN operator to create a condition that checks to see if a value in included in a list of multiple options.

Example query,
``` sql
SELECT column_1 FROM table WHERE column_1 IN (‘value_1’,’value_2’)
```
You can also combine IN with the NOT logical operator.
``` sql
SELECT column_1 FROM table WHERE column_1 NOT IN (‘value_1’,’value_2’)
```

### **Application** 
Define table
| column_1 | column_2 | column_3 | column_4   |
|-----------|-----------|-----------|------------|
| 101       | John      | 25        | 2005-07-12 |
| 102       | Alice     | 30        | 2003-04-05 |
| 103       | Bob       | 28        | 2009-11-23 |
| 104       | Alice     | 22        | 2002-09-15 |
| 105       | John      | 27        | 2006-08-04 |

``` sql
SELECT * FROM table WHERE column_2 IN ('Alice', 'Bob');
```
| column_1 | column_2 | column_3 | column_4   |
|-----------|-----------|-----------|------------|
| 102       | Alice     | 30        | 2003-04-05 |
| 103       | Bob       | 28        | 2009-11-23 |
| 104       | Alice     | 22        | 2002-09-15 |

``` sql
SELECT * FROM table WHERE column_2 NOT IN ('John', 'Bob');
```
| column_1 | column_2 | column_3 | column_4   |
|-----------|-----------|-----------|------------|
| 102       | Alice     | 30        | 2003-04-05 |
| 104       | Alice     | 22        | 2002-09-15 |

## 9-LIKE and ILIKE
We’ve already been able to perform direct comparisons against strings, such as: 
* WHERE first_name= ‘John’
  
But what if we want to match against a general pattern in a string:
* All emails ending in ‘@gmail.com’. 
* All names that begin with an ‘A’

The LIKE operator allows us to perform pattern matching against string data with the use of wildcard characters:
* Percent(%): Matches any sequence of characters
* Underscore(_): Matches any single character

All names that begin with an ‘A’:
* WHERE name LIKE ‘A%’
  
All names that end with an ‘a’:
* WHERE name LIKE ‘%a’

Using the underscore allows us to replace just a single character. Get all Mission Impossible films:
* WHERE title LIKE ‘Mission Impossible _’

We can also combine pattern matching operators to create more complex patterns:
* WHERE name LIKE ‘_her%’
  * Cheryl
  * Theresa
  * Sherri

Notice that LIKE is case-sensitive, we can use ILIKE which is case-insensitive.

Full regex capabilities: https://www.postgresql.org/docs/12/functions-matching.html
