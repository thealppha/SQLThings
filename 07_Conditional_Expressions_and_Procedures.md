Section overview:
* CASE
* COALESCE
* NULLIF
* CAST
* Views
* Import and Export Functionality

These keywords and functions will allow us to add logic to our commands and workflows in SQL.

## CASE
We can use the CASE statement to only execute SQL code when certain conditions are met. This is very similar to IF/ELSE statements in other programming languages. There are two main ways to use a CASE statement, either a **general CASE** or a **CASE expression**. 

General syntax:
```sql
CASE
	WHEN condition1 THEN result1
  WHEN condition2 THEN result2
  ELSE some_other_result
```

### Application
Table name: ***Test***
| A |
|---|
| 1 |
| 2 |

```sql
SELECT A, 
CASE WHEN A =  1 THEN 'one'
     WHEN A = 2 THEN 'two'
     ELSE 'other' AS label
END
FROM Test;
```
|  A  | label  |
| --- | ------ |
|  1  |  one   |
|  2  |  two   |

The CASE expression syntax first evaluates an expression then compares the result with each value in the WHEN clauses sequentially.

CASE expression syntax:
```sql
CASE expression
	WHEN value1 THEN result1
  WHEN value2 THEN result2
  ELSE some_other_result
END
```

### Application
Table name: ***Test***
| A |
|---|
| 1 |
| 2 |

```sql
SELECT A,
       CASE A
         WHEN 1 THEN 'one'
         WHEN 2 THEN 'two'
         ELSE 'other'
       END as label
FROM test;
```
|  A  | label  |
| --- | ------ |
|  1  |  one   |
|  2  |  two   |

**Note:** Using the CASE statement allows you to perform conditional aggregation, where you can apply different aggregation functions based on specified conditions.

### Application
```sql
SELECT 
  COUNT(*) AS total_rows,
  SUM(CASE WHEN a = 1 THEN 1 ELSE 0 END) AS count_a_is_1,
  SUM(CASE WHEN a = 2 THEN 1 ELSE 0 END) AS count_a_is_2
FROM test;
```
This query calculates the total number of rows in the ***test*** table (total_rows), the count of rows where the value in the ***a*** column is 1 (count_a_is_1), and the count of rows where the value in the ***a*** column is 2 (count_a_is_2).

## COALESCE
The COALESCE function accepts an unlimited number of arguments. It returns the first argument that is not null. If all arguments are null, the COALESCE function will return null.

### Application
```sql
SELECT COALESCE (1, 2)
```
1
```sql
SELECT COALESCE(NULL, 2, 3)
```
2

The COALESCE function becomes useful when querying a table that contains null values and substituting it with another value.

### Application
Table name: ***example*** 
| Item   | Price | Discount |
|--------|-------|----------|
| A      | 100   | 20       |
| B      | 300   | null     |
| C      | 200   | 10       |

```sql
SELECT item,(price - discount) AS final FROM table
```
| Item   | Final |
|--------|-------|
| A      | 80    |
| B      | null  |
| C      | 190   |

Doesn’t work for item B, should be 300.

```sql
SELECT item,(price - COALESCE(discount,0)) AS final FROM table
```
```
COALESCE(20, 0)  -->  20
COALESCE(null, 0) -->  0
COALESCE(10, 0)  -->  10
```
| Item   | Final |
|--------|-------|
| A      | 80    |
| B      | 300   |
| C      | 190   |

Keep the COALESCE function in mind in case you encounter a table with null values that you want to perform operations on!

## CAST
The CAST operator let’s you convert from one data type into another. Keep in mind not every instance of a data type can be CAST to another data type, it must be reasonable to convert the data, for example ‘5’ to an integer will work, ‘five’ to an integer will not.

Syntax for CAST function:
```sql
SELECT CAST('5' AS INTEGER)
```

PostgreSQL CAST operator:
```sql
SELECT '5'::INTEGER
```

To use this in a SELECT query with a column name instead of a single instance:
```sql
SELECT CAST(date AS TIMESTAMP) FROM table
```

### Application:
Table name: ***example***
| identity    |
|-------------|
| 123456      |
| 7890123     |
| 456789012   |
| 987654321   |
| 12345678901 |

```sql
SELECT 
    identity,
    CHAR_LENGTH(CAST(identity AS VARCHAR(20))) AS char_length_of_identity
FROM example;
```
| identity    | char_length_of_identity |
|-------------|-------------------------|
| 123456      | 6                       |
| 7890123     | 7                       |
| 456789012   | 9                       |
| 987654321   | 9                       |
| 12345678901 | 11                      |

CHAR_LENGTH is used to calculate the character length of the resulting varchar value. We can prefer to use LENGTH instead of CHAR_LENGTH.

## NULLIF
The NULLIF function takes in 2 inputs and returns NULL if both are equal, otherwise it returns the first argument passed.

### Application
```sql
NULLIF(10,10)
```
Returns NULL.

```sql
NULLIF(10,12)
```
Returns 10.

This becomes very useful in cases where a NULL value would cause an error or unwanted result.

### Application
Table name: ***depts***
| Name   | Department |
|--------|------------|
| Lauren | A          |
| Vinton | A          |
| Claire | B          |

To calculate the ratio of department A to department B:
```sql
SELECT (
SUM(CASE WHEN Department = 'A' THEN 1 ELSE 0 END) /
SUM(CASE WHEN Department = 'B' THEN 1 ELSE 0 END) ) AS department_ratio
FROM depts
```
| department_ratio |
|------------------|
| 2                |

To delete rows whose department data is B:
```sql
DELETE FROM depts
WHERE Department = 'B'
```
| Name   | Department |
|--------|------------|
| Lauren | A          |
| Vinton | A          |

To calculate the ratio between departments again:
```sql
SELECT (
SUM(CASE WHEN Department = 'A' THEN 1 ELSE 0 END) /
SUM(CASE WHEN Department = 'B' THEN 1 ELSE 0 END) ) AS department_ratio
FROM depts
```
```
ERROR: division by zero
SQL state: 22012
```

We will use NULLIF to solve this error.

```sql
SELECT (
SUM(CASE WHEN Department = 'A' THEN 1 ELSE 0 END) /
NULLIF(SUM(CASE WHEN Department = 'B' THEN 1 ELSE 0 END)), 0) AS department_ratio
FROM depts
```

NULLIF returns NULL if the total for department B is zero.

| department_ratio |
|------------------|
| [null]           |

## Views
Often there are specific combinations of tables and conditions that you find yourself using quite often for a project. Instead of having to perform the same query over and over again as a starting point, you can create a VIEW to quickly see this query with a simple call. A view is a database object that is of a stored query. A view can be accessed as a virtual table in PostgreSQL. Notice that a view does not store data physically, it simply stores the query. You can also update and alter existing views.

### Application
To receive some customer related queries:
```sql
SELECT first_name, last_name, address FROM customer
INNER JOIN address
ON customer.address_id = address.address_id
```

To keep the previous query in the view:
```sql
CREATE VIEW customer_ınfo AS
SELECT first_name, last_name, address FROM customer
INNER JOIN address
ON customer.address_id = address.address_id
```

To use view now, not the query I often use:
```sql
SELECT * FROM customer_info
```

To create a new view or replace an existing one with the same name if it already exists:
```sql
CREATE OR REPLACE VIEW customer_ınfo AS
SELECT first_name, last_name, address, district FROM customer
INNER JOIN address
ON customer.address_id = address.address_id
```

To remove a view from the database if it exists:
```sql
DROP VIEW IF EXISTS customer_info
```

To change the name of the view:
```sql
ALTER VIEW customer_info RENAME TO c_info
```