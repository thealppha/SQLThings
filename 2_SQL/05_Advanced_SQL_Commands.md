# Advanced SQL Commands

## Timestamps and Extract
* TIME - Contains only time 
* DATE - Contains only date
* TIMESTAMP - Contains date and time 
* TIMESTAMPTZ - Contains date,time, and timezone 

Let’s explore functions and operations related to these specific data types:

* TIMEZONE
* NOW
* TIMEOFDAY
* CURRENT_TIME 
* CURRENT_DATE

**Usages:**
```sql
SHOW TIMEZONE;
```
|   TimeZone   |
|--------------|
|      UTC     |

```sql
SELECT NOW();
```
|        NOW()        |
|---------------------|
| 2023-12-22 06:46:14 |

```sql
SELECT TIMEOFDAY();
```
|          timeofday          |
|-----------------------------|
| Fri Dec 22 06:49:02.879739  |

```sql
SELECT CURRENT_TIME;
```
|      current_time      |
|------------------------|
| 06:55:28.719912+00     |

```sql
SELECT CURRENT_DATE;
```
|   current_date   |
|------------------|
|   2023-12-22     |

Let’s explore extracting information from a time based data type using:

* EXTRACT()
  * Allows you to “extract” or obtain a sub-component of a date value:
    * YEAR
    * MONTH
    * DAY
    * WEEK
    * QUARTER
  * EXTRACT(YEAR FROM date_col)

* AGE()
  * Calculates and returns the current age given a timestamp
  * AGE(date_col)
  * Returns: 13 years 1 mon 5 days 01:34:13.003423

* TO_CHAR()
  * General function to convert data types to text
  * Useful for timestamp formatting 
  * TO_CHAR(date_col, ‘mm-dd-yyyy’)

### Application
Table Name: ***payment***
| id |      payment_date      |
|----|------------------------|
| 1  | 2019-13-22 08:00:00    |
| 2  | 2017-15-22 10:30:00    |
| 3  | 2023-05-22 13:45:00    |
| 4  | 2020-08-22 16:15:00    |
| 5  | 2022-16-22 19:20:00    |

```sql
SELECT EXTRACT(YEAR FROM payment_date) FROM payment;
```
| extract |
|---------|
|  2019   |
|  2017   |
|  2023   |
|  2020   |
|  2022   |

```sql
SELECT EXTRACT(MONTH FROM payment_date) FROM payment;
```
| extract |
|---------|
|    12   |
|    12   |
|    5    |
|    8    |
|    12   |

```sql
SELECT EXTRACT(QUARTER FROM payment_date) FROM payment;
```
| extract |
|---------|
|    4    |
|    4    |
|    2    |
|    3    |
|    4    |

```sql
SELECT AGE(payment_date) FROM payment;
```
|            age            |
|---------------------------|
| 3 years 11 mons 30 days 16:00:00 |
| 5 years 11 mons 30 days 13:30:00 |
| 6 mons 30 days 10:15:00         |
| 3 years 3 mons 30 days 07:45:00 |
| 11 mons 30 days 04:40:00        |

```sql
SELECT TO_CHAR(payment_date, 'MONTH-YYYY') FROM payment;
```
|     to_char      |
|------------------|
| DECEMBER - 2019  |
| DECEMBER - 2017  |
| MAY      - 2023  |
| AUGUST   - 2020  |
| DECEMBER - 2022  |

```sql
SELECT TO_CHAR(payment_date, 'mon/dd/YYYY') FROM payment;
```
|   to_char    |
|--------------|
| dec/22/2019  |
| dec/22/2017  |
| may/22/2023  |
| aug/22/2020  |
| dec/22/2022  |

```sql
SELECT TO_CHAR(payment_date, 'dd/mm/YYYY') FROM payment;
```
|   to_char    |
|--------------|
| 22/12/2019   |
| 22/12/2017   |
| 22/05/2023   |
| 22/08/2020   |
| 22/12/2022   |

## Mathematical & String Functions and Operators
### Mathematical Functions and Operators

Table Name: **person**
| id | fname |  lname  | january | july |
|----|-------|---------|---------|------|
| 1  | Adam  | Johnson |  5000   | 6000 |
| 2  | Eve   | Smith   |  5500   | 6200 |
| 3  | John  | Doe     |  6000   | 6500 |
| 4  | Jane  | Williams|  5200   | 6300 |
| 5  | Peter | Davis   |  5800   | 6700 |

```sql
SELECT ROUND(july / january *100 - 100, 2) AS raise FROM person;
```
| raise |
|-------|
|  20   |
| 12.73 |
|  8.33 |
| 21.15 |
| 15.52 |

Link: https://www.postgresql.org/docs/9.5/functions-math.html

### String Functions and Operators

```sql
SELECT fname || ' ' || lname AS full_name FROM person;
```
|      full_name      |
|---------------------|
|   Adam Johnson      |
|     Eve Smith       |
|     John Doe        |
|  Jane Williams      |
|   Peter Davis       |

```sql
SELECT LEFT(UPPER(fname), 2) || RIGHT(UPPER(lname),2) || '@gmail.com' AS full_name FROM person;
```
|   mail_address    |
|-------------------|
| ADON@gmail.com    |
| EVTH@gmail.com    |
| JOOE@gmail.com    |
| JAMS@gmail.com    |
| PEIS@gmail.com    |

Link: https://www.postgresql.org/docs/9.1/functions-string.html


## SubQuery
A sub query allows you to construct complex queries, essentially performing a query on the results of another query. The syntax is straightforward and involves two SELECT statements.

**Note:** The subquery is performed first since it is inside the parenthesis.

### Application
Table Name: ***employees***
| employee_id | employee_name | department_id | salary |
|--------------|---------------|----------------|--------|
| 1            | John Doe       | 1              | 5000   |
| 2            | Jane Smith     | 2              | 6000   |
| 3            | Bob Johnson    | 1              | 5500   |
| 4            | Alice Williams | 3              | 7000   |
| 5            | Charlie Davis  | 2              | 6500   |

Table Name: ***departments***
| department_id | department_name |
|---------------|------------------|
| 1             | Sales            |
| 2             | Marketing        |
| 3             | Finance          |

**Note:** We can also use the IN operator in conjunction with a subquery to check against multiple results returned.

To find employees in the 'Sales' department:
```sql
SELECT employee_name FROM employees
WHERE department_id IN (
    SELECT department_id FROM departments WHERE department_name = 'Sales'
);
```

**Note:** The EXISTS operator is used to test for existence of rows in a subquery. Typically a subquery is passed in the EXISTS() function to check if any rows are returned with the subquery.

To find departments that have at least one employee with salary > 6000:
```sql
SELECT department_name FROM departments
WHERE EXISTS (
    SELECT * FROM employees
    WHERE employees.department_id = departments.department_id
      AND employees.salary > 6000
);
```

## Self-Join
A self-join is a query in which a table is joined to itself. Self-joins are useful for comparing values in a column of rows within the same table. The self join can be viewed as a join of two copies of the same table. The table is not actually copied, but SQL performs the command as though it were. There is no special keyword for a self join, its simply standard JOIN syntax with the same table in both parts. However, when using a self join it is necessary to use an alias for the table, otherwise the table names would be ambiguous.

### Application
Table Name: ***employees***
| employee_id | employee_name   | manager_id |
|-------------|------------------|------------|
| 1           | John Doe         | NULL       |
| 2           | Jane Smith       | 1          |
| 3           | Bob Johnson      | 1          |
| 4           | Alice Williams   | 2          |
| 5           | Charlie Davis    | 2          |

To find the employees and their respective managers:
```sql
SELECT e.employee_name AS employee, m.employee_name AS manager
FROM employees AS e
LEFT JOIN employees AS m ON e.manager_id = m.employee_id;
```
| employee         | manager         |
|------------------|-----------------|
| John Doe         | NULL            |
| Jane Smith       | John Doe        |
| Bob Johnson      | John Doe        |
| Alice Williams   | Jane Smith      |
| Charlie Davis    | Jane Smith      |
