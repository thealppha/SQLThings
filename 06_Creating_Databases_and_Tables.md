# Creating Databases and Tables

Section Overview
* Data Types
* Primary and Foreign Keys
* Constraints
* CREATE
* INSERT
* UPDATE
* DELETE, ALTER, DROP

## Data Types

* Boolean
  * True or False
* Character
  * char, varchar, and text
* Numeric
  * integer and floating-point number
* Temporal
  * date, time, timestamp, and interval
* UUID
  * Universally Unique Identifiers
* Array
  * Stores an array of strings, numbers, etc.
* JSON
* Hstore key-value pair
* Special types such as network address and geometric data.

Link: https://www.postgresql.org/docs/current/datatype.html

## Primary and Foreign Keys
A **primary key** is a column or a group of columns used to identify a row uniquely in a table. Primary keys are also important since they allow us to easily discern what columns should be used for joining tables together.

| EmployeeID | FirstName | LastName | BirthDate   | Department   | Salary   |
|------------|-----------|----------|-------------|--------------|----------|
| 1          | John      | Doe      | 1990-05-15  | IT           | 60000    |
| 2          | Jane      | Smith    | 1985-08-22  | HR           | 55000    |
| 3          | Mark      | Johnson  | 1993-11-10  | Finance      | 65000    |

In this example, the ***EmployeeID*** column serves as the primary key for the table. The primary key uniquely identifies each record (row) in the table. It must have a unique value for each row, and it cannot have NULL values. The primary key is used to establish relationships between tables in a relational database and ensures the integrity of the data.

A **foreign key** is a field or group of fields in a table that uniquely identifies a row in another table. A foreign key is defined in a table that references to the primary key of the other table. The table that contains the foreign key is called referencing table or child table. The table to which the foreign key references is called referenced table or parent table. A table can have multiple foreign keys depending on its relationships with other tables.

Table Name: ***Departments***
| DepartmentID | DepartmentName |
|--------------|-----------------|
| 1            | IT              |
| 2            | HR              |
| 3            | Finance         |

Table Name: ***Employees***
| EmployeeID | FirstName | LastName | BirthDate   | DepartmentID | Salary   |
|------------|-----------|----------|-------------|--------------|----------|
| 1          | John      | Doe      | 1990-05-15  | 1            | 60000    |
| 2          | Jane      | Smith    | 1985-08-22  | 2            | 55000    |
| 3          | Mark      | Johnson  | 1993-11-10  | 3            | 65000    |

In this example, the ***DepartmentID*** column in the ***Employees*** table is a foreign key that refers to the ***DepartmentID*** column in the ***Departments*** table. This establishes a relationship between the two tables, indicating that the ***DepartmentID*** in the ***Employees*** table is associated with a specific department in the ***Departments*** table.

**Note:** When creating tables and defining columns, we can use constraints to define columns as being a primary key, or attaching a foreign key relationship to another table.

## Constraints
Constraints are the rules enforced on data columns on table. These are used to prevent invalid data from being entered into the database. This ensures the accuracy and reliability of the data in the database.

Constraints can be divided into two main categories:
  * Column Constraints
    * Constrains the data in a column to adhere to certain conditions.
  * Table Constraints 
    * Applied to the entire table rather than to an individual column.

The most common constraints used:
  * **NOT NULL** Constraint 
    * Ensures that a column cannot have NULL value.
  * **UNIQUE** Constraint 
    * Ensures that all values in a column are different.
  * **PRIMARY Key**
    * Uniquely identifies each row/record in a database table.
  * **FOREIGN Key** 
    * Constrains data based on columns in other tables.
  * **CHECK** Constraint 
    * Ensures that all values in a column satisfy certain conditions.

Table Constraints:
  * CHECK (condition) 
    * To check a condition when inserting or updating data.
  * REFERENCES
    * To constrain the value stored in the column that must exist in a column in another table.
  * UNIQUE (column_list)
    * Forces the values stored in the columns listed inside the parentheses to be unique.
  * PRIMARY KEY(column_list)
    * Allows you to define the primary key that consists of multiple columns.

## CREATE Table
Full General Syntax:
```sql
CREATE TABLE table_name (
  column_name TYPE column_constraint,
  column_name TYPE column_constraint,
  table_constraint table_constraint
) INHERITS existing_table_name;
```

Common Simple Syntax:
```sql
CREATE TABLE table_name (
  column_name TYPE column_constraint,
  column_name TYPE column_constraint,
   );
```

SERIAL, in PostgreSQL, a sequence is a special kind of database object that generates a sequence of integers. A sequence is often used as the primary key column in a table. It will create a sequence object and set the next value generated by the sequence as the default value for the column. It logs unique integer entries for you automatically upon insertion.

### Application
We will create three tables.

```sql
CREATE TABLE account( 
  user_id SERIAL PRIMARY KEY, 
  username VARCHAR (50) UNIQUE NOT NULL, 
  password VARCHAR(50) NOT NULL, 
  email VARCHAR(250) UNIQUE NOT NULL, 
  created_on TIMESTAMP NOT NULL, 
  last_login TIMESTAMP 
)
```

```sql
CREATE TABLE job(
  job_id SERTIAL PRIMARY KEY,
  job_name VARCHAR(250) UNIQUE NOT NULL
)
```

```sql
CREATE TABLE account job(
  user_id INTEGER REFERENCES account(user_id),
  job_id INTEGER REFERENCES job(job_id),
  hire_date TIMESTAMP
)
```

## INSERT
INSERT allows you to add in rows to a table.

General Syntax:
```sql
INSERT INTO table (column1, column2, …)
VALUES
   (value1, value2, …),
   (value1, value2, …) ,...;
```

Syntax for inserting values from another table:
```sql
INSERT INTO table(column1,column2,...)
SELECT column1,column2,...
FROM another_table
WHERE condition;
```

**Note:** The inserted row values must match up for the table, including constraints. SERIAL columns do not need to be provided a value.

### Application
```sql
INSERT INTO account(username, password, email, created_on)
VALUES ('Jose', 'password', 'jose@mail.com', CURRENT_TIMESTAMP)
```

```sql
INSERT INTO job(job_name)
VALUES ('Astronaut')

INSERT INTO job(job_name)
VALUES ('President')
```

```sql
INSERT INTO account_job(user_id, job_id, hire_data)
VALUES (1, 1, CURRENT_TIMESTAMP)
```

The next one gives error:
```sql
INSERT INTO account_job(user_id, job_id, hire_data)
VALUES (10, 10, CURRENT_TIMESTAMP)
```

```
It will throw:
ERROR: insert or update on table "account_job" violates foreign key constraint
“account_job_user_id_fkey"
DETAIL: Key (user_id)=(10) is not present in table "account".
SOL state: 23503
```

This error occurs because there's an attempt to insert or update a record in the ***account job*** table with a foreign key reference to a ***user_id*** value (10), but there is no corresponding record with that ***user_id*** in the ***account*** table. Foreign key constraints ensure referential integrity, and this violation indicates a mismatch in the referenced keys.

## UPDATE
The UPDATE keyword allows for the changing of values of the columns in a table.

General Syntax:
```sql
UPDATE table
SET column1 = value1,
    column2 = value2 ,...
WHERE
   condition;
```

### Examples

UPDATE account table:
```sql
SET last_login = CURRENT_TIMESTAMP
    WHERE last_login IS NULL;
```

Reset everything without WHERE condition:
```sql
UPDATE account
SET last_login = CURRENT_TIMESTAMP
```

Set based on another column:
```sql
UPDATE account
SET last_login = created_on
```

Using another table’s values:
```sql
UPDATE TableA
SET original_col = TableB.new_col
FROM tableB
WHERE tableA.id = TableB.id
```

Return affected rows:
```sql
UPDATE account
SET last_login = created_on
RETURNING account_id,last_login
```

### Aplication
**Note:** Check table after every update

```sql
UPDATE account
SET last_login = CURRENT_TIMESTAMP
```

```sql
UPDATE account
SET last_login = created_on
```

```sql
UPDATE account_job
SET hire_date = account.created_on
FROM account
WHERE account_job.user_id = account.user_id
```

```sql
UPDATE account
SET last_login = CURRENT_TIMESTAMP
RETURNING email, created_on, last_login
```

## DELETE
We can use the DELETE clause to remove rows from a table.

Syntax example:
```sql
DELETE FROM table
WHERE row_id = 1
```

### Examples
To delete rows based on their presence in other tables:
```sql
DELETE FROM tableA
USING tableB
WHERE tableA.id=TableB.id
```

To delete all rows from a table:
```sql
DELETE FROM table
```

**Note:** Similar to UPDATE command, you can also add in a RETURNING call to return rows that were removed.

### Application
```sql
INSERT INTO job(job_name)
VALUES ('Cowboy')
```

```sql
DELETE FROM job
WHERE job_name = 'Cowboy'
RETURNING job_id, job_name
```

## ALTER
The ALTER clause allows for changes to an existing table structure, such as:
* Adding, dropping, or renaming columns
* Changing a column’s data type
* Set DEFAULT values for a column
* Add CHECK constraints
* Rename table

General syntax:
```sql
ALTER TABLE table_name action
```

Adding columns:
```sql
ALTER TABLE table_name 
ADD COLUMN new_col TYPE
```

Removing columns:
```sql
ALTER TABLE table_name 
DROP COLUMN col_name
```

Alter constraints:
```sql
ALTER TABLE table_name 
ALTER COLUMN col_name
SET DEFAULT value
```
```sql
ALTER TABLE table_name 
ALTER COLUMN col_name
DROP DEFAULT
```
```sql
ALTER TABLE table_name 
ALTER COLUMN col_name
SET NOT NULL
```
```sql
ALTER TABLE table_name 
ALTER COLUMN col_name
DROP NOT NULL
```
```sql
ALTER TABLE table_name 
ALTER COLUMN col_name
ADD CONSTRAINT constraint_name
```

Renaming table:
```sql
ALTER TABLE table_name
RENAME TO new_table_name
```

Renaming column:
```sql
ALTER TABLE table_name
RENAME COLUMN column_name TO new_column_name
```

## DROP
DROP allows for the complete removal of a column in a table. In PostgreSQL this will also automatically remove all of its indexes and constraints involving the column. However, it will not remove columns used in views, triggers, or stored procedures without the additional CASCADE clause.

General syntax:
```sql
ALTER TABLE table_name
DROP COLUMN col_name
```
Remove all dependencies:
```sql
ALTER TABLE table_name
DROP COLUMN col_name CASCADE
```
Check for existence to avoid error:
```sql
ALTER TABLE table_name
DROP COLUMN IF EXISTS col_name 
```
Drop multiple columns:
```sql
ALTER TABLE table_name
DROP COLUMN  col_one,
DROP COLUMN  col_two 
```

## CHECK
The CHECK constraint allows us to create more customized constraints that adhere to a certain condition. Such as making sure all inserted integer values fall below a certain threshold.

General syntax:
```sql
CREATE TABLE example(
      ex_id SERIAL PRIMARY KEY,
      age SMALLINT CHECK (age > 21),
      parent_age SMALLINT CHECK ( parent_age > age)
      );
```
