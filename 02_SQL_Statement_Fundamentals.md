# SQL Statement Fundamentals

The syntax shown in this section is applicable to any major SQL engine (e.g. MS SQL Server, MySQL, Oracle, etc…)

## **1-SELECT**
SELECT is the most common statement used, and it allows us to retrieve information from a table.

Basic syntax example for specific column(s):
``` sql
SELECT column_name FROM table_name
```

For all columns:
``` sql
SELECT * FROM table_name
```

**Note:** In general it is not good practice to use an asterisk (*) in the SELECT statement if you don’t really need all columns.

### **Application**
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To learn the ***first_name*** and ***last_name***:
``` sql
SELECT first_name, last_name FROM person;
```
| first_name | last_name |
|------------|-----------|
| John       | Smith     |
| Alice      | Johnson   |
| Bob        | Brown     |
| Alice      | Adams     |
| John       | Doe       |

To bring all columns:
``` sql
SELECT * FROM person;
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

**Note:** SQL keywords can be written in lowercase or uppercase. Additionally, a semicolon may or may not be used. By default, capitalizing keywords and using semicolons helps for readability.

## 2-SELECT DISTINCT 
Sometimes a table contains a column that has duplicate values. The DISTINCT keyword can be used to return only the distinct values in a column.

Basic syntax example:
``` sql
SELECT DISTINCT column_name FROM table_name
```

You can also use parenthesis for clarity:
``` sql
SELECT DISTINCT(column_name) FROM table_name
```

### **Application**
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find out how many different first_name there are:
``` sql
SELECT DISTINCT first_name FROM person;
```
OR
``` sql
SELECT DISTINCT(first_name) FROM person;
```
| first_name |
|------------|
| John       |
| Alice      |
| Bob        |


## 3-COUNT
The COUNT function returns the number of input rows that match a specific condition of a query. It can be used on a specific column or just pass COUNT(*).

Basic syntax example:
``` sql
SELECT COUNT(column_name) FROM table_name;
```

Because of the each column has the same number of rows, next two commands return the same things.
``` sql
SELECT COUNT(column_name) FROM table_name;
SELECT COUNT(*) FROM table_name;
```

COUNT is much more useful when combined with other commands, such as DISTINCT.
``` sql
SELECT COUNT(DISTINCT column_name) FROM table_name;
```

### **Application**
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find out how many rows there are in the ***person*** table:
``` sql
SELECT COUNT(*) FROM person;
```
| COUNT(*) |
|----------|
| 5        |


To find out how many unique last_name's are in the ***person*** table:
``` sql
SELECT COUNT(DISTINCT last_name) FROM person;
```
| COUNT(DISTINCT last_name) |
|---------------------------|
| 4                         |

## 4-SELECT WHERE 
The WHERE statement allows to specify conditions on columns for the rows to be returned.

Basic syntax example:
``` sql
SELECT * FROM table_name WHERE conditions;
```
The WHERE clause appears immediately after the FROM clause of the SELECT statement. The conditions are used to filter the rows returned from the SELECT statement.

### **Application**
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find out whose ***first name*** is Alice in the table:
``` sql
SELECT * FROM customer WHERE first_name='Alice';
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   |  30 | 2020-03-20   |
| 104 | Alice      | Adams     |  22 | 2019-05-30   |

To find out which person's ***first_name*** is equal to John and whose ***age*** is greater than 26:
``` sql
SELECT * FROM person WHERE first_name='John' AND age>26;
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 105 | John       | Doe       |  27 | 2023-06-04   |

## 5-ORDER BY 
This command is used to sort rows based on a column value, in either ascending or descending order.

Basic syntax:
``` sql
SELECT * FROM table_name ORDER BY column_name ASC/DESC;
```
Use ASC to sort in ascending order. Use DESC to sort in descending order. As a default, ORDER BY uses ASC.

### **Application-1** 
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To order the table by ***age***:
``` sql
SELECT * FROM person ORDER BY age DESC;
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 105 | John       | Doe       | 27  | 2023-06-04   |
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |

**Note:** Also, ORDER BY multiple columns. This makes sense when one column has duplicate entries.
``` sql
SELECT * FROM table_name ORDER BY column_1_name ASC/DESC, column_2_name ASC/DESC;
```

### **Application-2** 
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

Order by ***first_name*** as DESC, and then order duplicate ***first_name***'s among themselves as ASC by ***age***:
``` sql
SELECT * FROM person ORDER BY first_name DESC, age ASC;
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 105 | John       | Doe       | 27  | 2023-06-04   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |


## 6-LIMIT
The LIMIT command is used to restrict the number of rows returned in a query, providing a concise way to view a subset of data for a quick overview of the table. LIMIT goes at the very end of a query request and is the last command to be executed.

Basic syntax:
``` sql
SELECT * FROM table_name LIMIT value;
```

### **Application** 
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To order by ***age*** as DESC and see only the first 3 outputs:
``` sql
SELECT * FROM person ORDER BY age DESC LIMIT 3;
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

## 7-BETWEEN
The BETWEEN operator can be used to match a value against a range of values BETWEEN low AND high. You can also combine BETWEEN with the NOT logical operator value NOT BETWEEN low AND high. 

**Note:** The BETWEEN operator can also be used with dates. Note that you need to format dates in the ISO 8601 standard format, which is YYYY-MM-DD date BETWEEN ‘2007-01-01’ AND ‘2007-02-01’.

Basic syntax:
``` sql
SELECT * FROM table_name WHERE column_name BETWEEN low_value AND high_value;
```

### **Application** 
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find out who is between the ***age***'s of 27 and 30:
``` sql
SELECT * FROM person WHERE age BETWEEN 27 AND 30;
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 105 | John       | Doe       | 27  | 2023-06-04   |


To find out who is between the ***joining_date***'s of 2018-01-01 and 2022-01-01:
``` sql
SELECT * FROM person WHERE joining_date BETWEEN '2018-01-01' AND '2022-01-01';
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |

## 8-IN
The IN operator in SQL makes it easier to filter rows by checking if a column's value is among a specified list of values.

Basic syntax:
``` sql
SELECT * FROM table_name WHERE column_name IN (‘value_1’, ’value_2’)
```
Also, IN can be combined with the NOT logical operator:
``` sql
SELECT * FROM table_name WHERE column_name NOT IN (‘value_1’, ’value_2’)
```

### **Application** 
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find people with ***last_name***'s Johnson and Doe:
``` sql
SELECT * FROM person WHERE last_name IN ('Johnson', 'Doe');
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

## 9-LIKE and ILIKE
We’ve already been able to perform direct comparisons against strings, such as: 
* WHERE first_name= ‘John’
But what if we want to match against a general pattern in a string:
* All emails ending in ‘@gmail.com’

The LIKE operator allows us to perform pattern matching against string data with the use of wildcard characters:
* Percent(%): Matches any sequence of characters
* Underscore(_): Matches any single character

**Note:** Notice that LIKE is case-sensitive, we can use ILIKE which is case-insensitive.

### **Application** 
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find people with ***first_name***'s start with J:
``` sql
SELECT * FROM person WHERE first_name LIKE 'J%';
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To find people whose ***last_name*** has 'o' as the 2nd letter:
``` sql
SELECT * FROM person WHERE last_name LIKE '_o%';
```
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 105 | John       | Doe       | 27  | 2023-06-04   |
