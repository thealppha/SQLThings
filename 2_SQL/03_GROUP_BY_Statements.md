# GROUP BY Statements

## Aggregate Functions

SQL provides a variety of aggregate functions. The main idea behind an aggregate function is to take multiple inputs and return a single output.
https://www.postgresql.org/docs/current/functions-aggregate.html

Most Common Aggregate Functions:
* AVG() - returns average value
* COUNT() - returns number of values
* MAX() - returns maximum value
* MIN() - returns minimum value
* SUM() - returns the sum of all values

**Note:** Aggregate function calls happen only in the SELECT clause or the HAVING clause.

### **Application**
Table Name: ***person***
| id  | first_name | last_name | age | joining_date |
|-----|------------|-----------|-----|--------------|
| 101 | John       | Smith     | 25  | 2017-02-15   |
| 102 | Alice      | Johnson   | 30  | 2020-03-20   |
| 103 | Bob        | Brown     | 28  | 2016-04-25   |
| 104 | Alice      | Adams     | 22  | 2019-05-30   |
| 105 | John       | Doe       | 27  | 2023-06-04   |

To learn the min and max ***age***s in the person table:
``` sql
SELECT MIN(age), MAX(age) FROM person;
```
| MIN(age) | MAX(age) |
|----------|----------|
|    22    |    30    |


To learn the average ***age*** in the person table:
``` sql
SELECT AVG(age) FROM person;
```
| AVG(age) |
|----------|
|   26.4   |


**Note:** AVG() returns a floating point value many decimal places (e.g. 2.342418…). You can use ROUND() to specify precision after the decimal.
``` sql
SELECT ROUND(AVG(age)) FROM person;
```
| ROUND(AVG(age))  |
|------------------|
|        26        |

**Note:** Aggregate functions return a single value.

## GROUP BY

GROUP BY allows us to aggregate columns per some category. We need to choose a categorical column to GROUP BY. Categorical columns are non-continuous.

**Note:** Keep in mind, they can still be numerical, such as cabin class categories on a ship (e.g. Class 1, Class 2, Class 3).

### **Application**
Table Name: ***example***
| category | value1 | value2 |
|----------|--------|--------|
|    A     |   1    |  114   |
|    B     |   0    |  105   |
|    A     |   1    |  117   |
|    C     |   0    |  110   |
|    C     |   1    |  101   |
|    B     |   1    |  118   |
|    A     |   0    |  113   |
|    B     |   1    |  107   |
|    C     |   0    |  116   |

To select unique ***categories*** from the example table:
``` sql
SELECT category FROM example GROUP BY category;
```
| category |
|----------|
|    A     |
|    B     |
|    C     |

To find out the average of the ***value1***s of each ***category*** class:
``` sql
SELECT category , ROUND(AVG(value2),2) FROM example GROUP BY category;
```
| category | AVG(value2)                |
|----------|----------------------------|
| A        | 114.67                     |
| B        | 110                        |
| C        | 109                        |

To find out the average of the ***value2***s of each ***category*** class except class A:
``` sql
SELECT category , AVG(value2) FROM example WHERE category != 'A' GROUP BY category;
```
| category | AVG(value2)                |
|----------|----------------------------|
| B        | 110                        |
| C        | 109                        |

**Note:** The GROUP BY clause must appear right after a FROM or WHERE statement.

**Note:** In the SELECT statement, columns must either have an aggregate function or be in the GROUP BY call.

**Note:** WHERE statements should not refer to the aggregation result, later on we will learn to use HAVING to filter on those results.

To sort each class in the ***category*** column by their average: ***value***
``` sql
SELECT category , ROUND(AVG(value2),2) FROM example GROUP BY category ORDER BY AVG(value) DESC;
```
| category | AVG(value2)                |
|----------|----------------------------|
| A        | 114.67                     |
| B        | 110                        |
| C        | 109                        |


**Note:** If you want to sort results based on the aggregate, make sure to reference the entire function.

To group average ***value2*** by ***category*** and ***value1***:
``` sql
SELECT category, value1, ROUND(AVG(value2)) FROM example GROUP BY category, value1;
```
| category | value1 | ROUND(AVG(value2)) |
|----------|--------|---------------------|
|    A     |   0    |        113          |
|    A     |   1    |        116          |
|    B     |   0    |        105          |
|    B     |   1    |        113          |
|    C     |   0    |        113          |
|    C     |   1    |        101          |

To sort the last table by the AVG value of ***value2***:
``` sql
SELECT category, value1, ROUND(AVG(value2)) FROM example GROUP BY category, value1 ORDER BY ROUND(AVG(value2)) DESC;
```
| category | value1 | ROUND(AVG(value2)) |
|----------|--------|---------------------|
|    A     |   1    |        116          |
|    A     |   0    |        113          |
|    B     |   1    |        113          |
|    C     |   0    |        113          |
|    B     |   0    |        105          |
|    C     |   1    |        101          |

## HAVING


