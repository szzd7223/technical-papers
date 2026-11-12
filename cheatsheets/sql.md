# SQL

## Basics
- Create DB
```bash
CREATE DATABASE database_name;
```
- List Database
```bash
\l
```
- Connect to a Database
```bash
\c database_name
```
- Create Table
```bash
CREATE TABLE employees (
    id INT NOT NULL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    salary DECIMAL(10, 2)
);
```
- Manage Primary and Foreign Keys
```

```
- Describe Table
```bash
\d table_name
```
- Drop table
```bash
DROP TABLE table_name;
```

## Working with tables
- Insert records into table
```bash
INSERT INTO person (
    first_name,
    last_name,
    gender,
    date_of_birth
)
VALUES (
    'Anne',
    'Smith',
    'FEMALE',
    DATE '1988-01-09'
);
```

## ALTER TABLE

Used to modify the structure of an existing table.

**Syntax:**
```sql
-- Add a column
ALTER TABLE table_name
ADD COLUMN column_name DATA_TYPE;

-- Modify column data type
ALTER TABLE table_name
ALTER COLUMN column_name TYPE DATA_TYPE;

-- Rename a column
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;

-- Drop a column
ALTER TABLE table_name
DROP COLUMN column_name;

-- Rename a table
ALTER TABLE old_name
RENAME TO new_name;
```

## INSERT INTO

Used to insert new records (rows) into a table.

**Syntax:**
```sql
-- Insert a single row
INSERT INTO table_name (column1, column2)
VALUES (value1, value2);

-- Insert multiple rows
INSERT INTO table_name (column1, column2)
VALUES
    (value1, value2),
    (value3, value4);
```

**Example:**
```sql
INSERT INTO customers (name, email)
VALUES
    ('John', 'john@gmail.com'),
    ('Alice', 'alice@gmail.com');
```

## Adding Primary Key (PK) and Foreign Key (FK)

### 1. Primary Key (PK)

Uniquely identifies each row in a table. Cannot contain NULL or duplicate values.

**Syntax:**
```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
PRIMARY KEY (column_name);
```

**Example:**
```sql
ALTER TABLE customers
ADD CONSTRAINT customers_pkey
PRIMARY KEY (customer_id);
```

### 2. Foreign Key (FK)

Creates a relationship between two tables by referencing a Primary Key or another eligible unique key.

**Syntax:**
```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name
FOREIGN KEY (column_name)
REFERENCES referenced_table(referenced_column);
```

**Example:**
```sql
ALTER TABLE orders
ADD CONSTRAINT orders_customer_fk
FOREIGN KEY (customer_id)
REFERENCES customers(customer_id);
```

### 3. ON DELETE / ON UPDATE

Defines what happens to referencing rows when the referenced key is deleted or updated.

```sql
ALTER TABLE orders
ADD CONSTRAINT orders_customer_fk
FOREIGN KEY (customer_id)
REFERENCES customers(customer_id)
ON DELETE CASCADE
ON UPDATE CASCADE;
```

**Common actions:**
- `CASCADE` – Automatically propagates the delete or update.
- `SET NULL` – Sets the referencing FK to NULL.
- `SET DEFAULT` – Sets the referencing FK to its default value.
- `RESTRICT` – Prevents the operation if referencing rows exist.
- `NO ACTION` – Checks the constraint at the normal constraint-checking time (default).

## Users and Roles
- Create a new user
```sql
CREATE USER john WITH PASSWORD 'john123';
```
- Give User Permission to Create DB
```sql
ALTER USER john CREATEDB;
```
- Give permission to user to create Other roles
```sql
ALTER USER john CREATEROLE;
```
- Check Existing Users
```sql
\du
```
- Delete User
```sql
DROP USER john;
```

## Basic Functions
- Read SQL files
```bash
psql -U username -d dbname -f /path/to/script.sql
```
Generally postgres user is root so we can use
```bash
sudo -u postgres psql -f sql/01_setup.sql
```
What this means:
- sudo -u postgres: Run the command as the Linux postgres user.
- psql: Launch PostgreSQL's command-line client.
- -f: Execute SQL commands from the specified file.

## SQL JOINS
Joins are used to combine data from two or more tables based on a related column between them.

### Types of JOINs
1. INNER JOIN
Returns only the rows that have matching values in both tables.
```sql
SELECT *
FROM deliveries
INNER JOIN matches
ON deliveries.match_id = matches.id;
```

2. LEFT JOIN (LEFT OUTER JOIN)
Returns all rows from the left table and matching rows from the right table. If there is no match, the right table columns contain NULL.
```sql
SELECT *
FROM deliveries
LEFT JOIN matches
ON deliveries.match_id = matches.id;
```

3. RIGHT JOIN (RIGHT OUTER JOIN)
Returns all rows from the right table and matching rows from the left table. If there is no match, the left table columns contain NULL.
```sql
SELECT *
FROM deliveries
RIGHT JOIN matches
ON deliveries.match_id = matches.id;
```

4. FULL OUTER JOIN
Returns all rows from both tables. If there is no match, the missing columns contain NULL.
```sql
SELECT *
FROM deliveries
FULL OUTER JOIN matches
ON deliveries.match_id = matches.id;
```

5. CROSS JOIN
Returns every possible combination of rows from both tables (Cartesian product).
```sql
SELECT *
FROM deliveries
CROSS JOIN matches;
```

6. SELF JOIN
A table is joined with itself. Useful when comparing rows within the same table.
```sql
SELECT *
FROM employees e1
JOIN employees e2
ON e1.manager_id = e2.id;
```

## SQL Aggregate Functions
Aggregate functions are used to perform calculations on multiple rows of data and return a single result.

### Types of Aggregate Functions
1. COUNT()
Returns the number of rows.
```sql
SELECT COUNT(*)
FROM deliveries;
```

2. SUM()
Returns the total sum of a column.
```sql
SELECT SUM(total_runs)
FROM deliveries;
```

3. AVG()
Returns the average value of a column.
```sql
SELECT AVG(total_runs)
FROM deliveries;
```

4. MIN()
Returns the smallest value in a column.
```sql
SELECT MIN(total_runs)
FROM deliveries;
```

5. MAX()
Returns the largest value in a column.
```sql
SELECT MAX(total_runs)
FROM deliveries;
```

## GROUP BY
GROUP BY is used to group rows that have the same values in a column. It is commonly used with aggregate functions to perform calculations on each group.
```sql
SELECT
    bowler,
    SUM(total_runs) AS total_runs
FROM
    deliveries
GROUP BY
    bowler;
```

## HAVING
HAVING is used to filter groups after they have been created using GROUP BY. It is commonly used with aggregate functions.
```sql
SELECT
    bowler,
    SUM(total_runs) AS total_runs
FROM
    deliveries
GROUP BY
    bowler
HAVING
    SUM(total_runs) > 500;
```

## Order of Execution of a Query
```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

1. FROM and JOINs
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. DISTINCT
7. ORDER BY
8. LIMIT / OFFSET

## SQL Logical Operators

### AND
Simple AND operator
```sql
SELECT * FROM students WHERE city = 'Jaipur' AND marks > 80;
```

### OR
```sql
SELECT * FROM students WHERE city = 'Jaipur' OR city = 'Mumbai';
```

### IN
```sql
SELECT * FROM students WHERE city IN ('Jaipur', 'Mumbai');
```

## SQL Case Statements
SQL Case Statement is SQL way of handling conditional logic within a query.

Syntax:
```sql
SELECT column_name,
  CASE 
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
  END AS new_column_alias
FROM table_name;
```

Example:
```sql
SELECT name,
  CASE
    WHEN monthlymaintenance > 100 THEN 'expensive'
    WHEN monthlymaintenance <= 100 THEN 'cheap'
  END AS cost
FROM cd.facilities;
```

## SQL UNION
It is used to combine two columns from different tables
```sql
SELECT surname FROM cd.members
UNION
SELECT name FROM cd.facilities
```

## SQL Subqueries
A subquery is a query inside another query. It is used to get data from one query and use it in another query.
```sql
SELECT
    *
FROM
    players
WHERE
    runs > (
        SELECT AVG(runs)
        FROM players
    );
```
In this example:
  The inner query calculates the average runs.
  The outer query selects players whose runs are greater than that average.

## SQL FILTER Statement
It is used to filter out the data and perform operations on it
```sql
SELECT
    bowler,
    SUM(total_runs - bye_runs - legbye_runs) AS runs_conceded,
    COUNT(*) FILTER(
        WHERE wide_runs = 0 AND noball_runs = 0
    ) AS legal_balls
FROM
    deliveries
```

## SQL CTE (Common Table Expression)
A CTE is a temporary result set that you can create using WITH and use inside another query. It makes complex queries easier to read and organize.
```sql
WITH bowler_stats AS (
    SELECT
        bowler,
        SUM(total_runs - bye_runs - legbye_runs) AS runs_conceded
    FROM
        deliveries
    GROUP BY
        bowler
)
SELECT
    *
FROM
    bowler_stats;
```

## SQL Date Time
Postgres gives various functions to extract date and time

1. EXTRACT
```sql
EXTRACT(DAY FROM order_date);
EXTRACT(MONTH FROM order_date);
EXTRACT(YEAR FROM order_date);
```

## SQL String functions

### 1. CONCAT()
```sql
CONCAT(str1, ' ', str2)
```

### 2. UPPER() and LOWER()
```sql
UPPER(str)
LOWER(str)
```

### 3. TRIM()
```sql
TRIM(str)
```

### 4. REPLACE()
Replaces the substring with another char(s)
```sql
REPLACE(string, substring_to_replace, replace_by)
```

### 5. LENGTH()
Returns the length of a string.
```sql
LENGTH(string)
```

### 6. LEFT() and RIGHT()
LEFT() extracts specified number of chars from left of string and RIGHT() extracts from right of String
```sql
test_db=# SELECT 'John Doe' AS name;
   name   
----------
 John Doe
(1 row)

test_db=# SELECT LEFT('John Doe', 3) AS name;
 name 
------
 Joh
(1 row)

test_db=# SELECT RIGHT('John Doe', 3) AS name;
 name 
------
 Doe
(1 row)
```

### 7. SUBSTRING()
Extracts the substring from specified start to specified end
```sql
test_db=# SELECT 'John Doe' AS name;
   name   
----------
 John Doe
(1 row)

test_db=# SELECT SUBSTRING('John Doe', 3, 4) AS name;
 name 
------
 hn D
(1 row)
```

## References
- [SQL Cheat Sheet](https://www.dbvis.com/wp-content/uploads/2024/04/SQL-Cheat-Sheet.pdf)
- [SQL Bolt](https://sqlbolt.com/)
- [PostgreSQL Exercises](https://pgexercises.com)
- [SQL Date & Time Functions by Data with Barra](https://www.youtube.com/watch?v=7RCxH9x5StM&t=2251s)
- [SQL String Functions by Data with Barra](https://www.youtube.com/watch?v=NOTWx5s-s1o)
