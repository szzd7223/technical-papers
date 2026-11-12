# SQL Theory

## ACID Properties

ACID is a set of four properties that ensure database transactions are processed reliably and safely.

**ACID stands for:**
- Atomicity
- Consistency
- Isolation
- Durability

### 1. Atomicity
- Either **ALL operations happen or NONE happen**.
- If one operation fails, the entire transaction is rolled back.
- Prevents incomplete transactions.

**Example:** Transferring ₹1,000 from Account A to Account B.

1. Deduct ₹1,000 from Account A.
2. Add ₹1,000 to Account B.

If the second operation fails, the first operation is also reversed.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

COMMIT;
```

### 2. Consistency
- A transaction must keep the database in a **valid state**.
- All database rules and constraints must be followed before and after a transaction.
- The total money in the system should remain the same during a transfer.

**Example:** Account A has ₹5,000 and Account B has ₹3,000. Total money = ₹8,000.

After transferring ₹1,000:
- Account A = ₹4,000
- Account B = ₹4,000
- Total money = ₹8,000

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE account_id = 2;

COMMIT;
```

### 3. Isolation
- Multiple transactions can run simultaneously without interfering with each other.
- Each transaction should behave as if it is running independently.
- Prevents incorrect results when multiple users access the same data.

**Example:** Two users try to book the last available movie ticket at the same time. Isolation ensures that both transactions don't successfully book the same seat.

```sql
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

UPDATE tickets
SET available_seats = available_seats - 1
WHERE available_seats > 0;

COMMIT;
```

### 4. Durability
- Once a transaction is successfully committed, its changes are permanent.
- Data remains saved even if the database crashes or the system restarts.

**Example:** You transfer ₹1,000 to another account. Once committed, the updated balance remains saved even if the database server shuts down.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

COMMIT;
```

## CAP Theorm

CAP Theorem describes three properties of distributed databases. It states that when a network partition occurs, a distributed database can guarantee either Consistency or Availability, but not both at the same time.

**CAP stands for:**
- Consistency
- Availability
- Partition Tolerance

### 1. Consistency
- Every read receives the latest successful write or an error.
- All nodes in the database return the same data.

**Example:** If you update your bank balance to ₹5,000, every node should return ₹5,000.

```sql
UPDATE accounts
SET balance = 5000
WHERE account_id = 1;
```

### 2. Availability
- Every request receives a response, even if some nodes are down.
- The response might contain older data.

**Example:** If one database server goes down, another server continues responding to user requests.

```text
Server 1: DOWN
Server 2: UP

User Request -> Server 2 -> Response
```

### 3. Partition Tolerance
- The system continues operating even when network communication between nodes fails.
- Nodes may not be able to communicate with each other.

**Example:** Two database servers are running in different locations, but a network failure prevents them from communicating.

```text
Server 1  X  Server 2

Network connection failed
```

### CAP Trade-off
- During a network partition, a distributed database must choose between Consistency and Availability.
- It cannot guarantee both simultaneously.

**Example:**

- **CP (Consistency + Partition Tolerance):** Rejects or delays some requests to prevent returning outdated data.
- **AP (Availability + Partition Tolerance):** Continues responding to requests, even if some responses contain outdated data.

## SQL Joins

Joins are used to combine data from two or more tables based on a related column between them.

**Types of Joins:**
- INNER JOIN
- LEFT JOIN
- RIGHT JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

### 1. INNER JOIN
- Returns only the rows that have matching values in both tables.
- Rows without a match are excluded.

**Example:** Get employees along with their department names.

```sql
SELECT
    employees.name,
    departments.department_name
FROM employees
INNER JOIN departments
    ON employees.department_id = departments.id;
```

### 2. LEFT JOIN
- Returns all rows from the left table and matching rows from the right table.
- If there is no match, the right table columns contain NULL.

**Example:** Get all employees, including those who don't belong to any department.

```sql
SELECT
    employees.name,
    departments.department_name
FROM employees
LEFT JOIN departments
    ON employees.department_id = departments.id;
```

### 3. RIGHT JOIN
- Returns all rows from the right table and matching rows from the left table.
- If there is no match, the left table columns contain NULL.

**Example:** Get all departments, including those that have no employees.

```sql
SELECT
    employees.name,
    departments.department_name
FROM employees
RIGHT JOIN departments
    ON employees.department_id = departments.id;
```

### 4. FULL OUTER JOIN
- Returns all rows from both tables.
- Matching rows are combined.
- Unmatched rows from either table contain NULL for the missing columns.

**Example:** Get all employees and all departments, whether they have matching records or not.

```sql
SELECT
    employees.name,
    departments.department_name
FROM employees
FULL OUTER JOIN departments
    ON employees.department_id = departments.id;
```

### 5. CROSS JOIN
- Returns every possible combination of rows from both tables.
- If the first table has 3 rows and the second has 2 rows, the result contains 6 rows.

**Example:** Generate all possible combinations of employees and projects.

```sql
SELECT
    employees.name,
    projects.project_name
FROM employees
CROSS JOIN projects;
```

### 6. SELF JOIN
- A table is joined with itself.
- Useful when rows in the same table are related to each other.

**Example:** Get employees along with their managers.

```sql
SELECT
    emp.name AS employee,
    mgr.name AS manager
FROM employees emp
LEFT JOIN employees mgr
    ON emp.manager_id = mgr.id;
```

## Aggregations, Filters in Queries

### 1. Aggregate Functions
- Aggregate functions perform calculations on multiple rows and return a single value.
- Common aggregate functions:
  - `COUNT()` – Counts the number of rows.
  - `SUM()` – Calculates the total.
  - `AVG()` – Calculates the average.
  - `MIN()` – Returns the minimum value.
  - `MAX()` – Returns the maximum value.

**Example:** Find the total, average and highest employee salary.

```sql
SELECT
    SUM(salary) AS total_salary,
    AVG(salary) AS average_salary,
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary,
    COUNT(*) AS total_employees
FROM employees;
```

### 2. WHERE
- Used to filter rows based on a condition.
- Filters individual rows before grouping or aggregation.

**Example:** Get employees whose salary is greater than ₹30,000.

```sql
SELECT *
FROM employees
WHERE salary > 30000;
```

### 3. HAVING
- Used to filter grouped data.
- Usually used with `GROUP BY` and aggregate functions.
- Filters groups after aggregation.

**Example:** Get departments where the average salary is greater than ₹30,000.

```sql
SELECT
    department_id,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 30000;
```

### 4. FILTER
- Used with aggregate functions to apply conditions to individual aggregate calculations.
- Different aggregate functions can use different conditions in the same query.

**Example:** Count employees in each department who earn more than ₹30,000 and those who earn less.

```sql
SELECT
    department_id,
    COUNT(*) FILTER (
        WHERE salary > 30000
    ) AS high_salary_employees,
    COUNT(*) FILTER (
        WHERE salary <= 30000
    ) AS low_salary_employees
FROM employees
GROUP BY department_id;
```

### 5. GROUP BY
- Groups rows that have the same values in specified columns.
- Commonly used with aggregate functions to perform calculations on each group.

**Example:** Find the total salary paid by each department.

```sql
SELECT
    department_id,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department_id;
```

### 6. DISTINCT
- Removes duplicate values from the result.
- Returns only unique values.

**Example:** Get all unique department IDs.

```sql
SELECT DISTINCT department_id
FROM employees;
```

### 7. ORDER BY
- Sorts the result based on one or more columns.
- `ASC` sorts in ascending order (default).
- `DESC` sorts in descending order.

**Example:** Get employees sorted by salary from highest to lowest.

```sql
SELECT *
FROM employees
ORDER BY salary DESC;
```

### 8. LIMIT
- Restricts the number of rows returned by a query.
- Useful when retrieving only a specific number of records.

**Example:** Get the top 5 highest-paid employees.

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 5;
```

## Normalization

Normalization is the process of organizing data in database tables to reduce data duplication and avoid data inconsistencies.

**Main goals:**
- Reduce duplicate data.
- Avoid insertion, update and deletion anomalies.
- Maintain data consistency.
- Organize data efficiently.

### 1. First Normal Form (1NF)
- Each column must contain atomic (single) values.
- Each row should be unique.
- No multiple values or repeating groups in a single column.

**Example:** Instead of storing multiple phone numbers in one column, store each phone number in a separate row.

```sql
-- Not in 1NF
id | name  | phone_numbers
1  | John  | 12345, 67890

-- In 1NF
id | name  | phone_number
1  | John  | 12345
1  | John  | 67890
```

### 2. Second Normal Form (2NF)
- Must already satisfy 1NF.
- Every non-key column must depend on the entire primary key, not just part of it.
- Mainly applies when a table has a composite primary key.

**Example:** Consider a student enrollment table:

**Before 2NF:**

| student_id | course_id | student_name | course_name |
|---|---|---|---|
| 1 | 101 | John | Python |
| 1 | 102 | John | SQL |
| 2 | 101 | Alice | Python |

Here, `(student_id, course_id)` is the composite primary key.

But:
- `student_name` depends only on `student_id`.
- `course_name` depends only on `course_id`.

This is called a **partial dependency**, which violates 2NF.

**Convert it into 2NF by splitting the table:**

**Students**

| student_id (PK) | student_name |
|---|---|
| 1 | John |
| 2 | Alice |

**Courses**

| course_id (PK) | course_name |
|---|---|
| 101 | Python |
| 102 | SQL |

**Enrollments**

| student_id (FK) | course_id (FK) |
|---|---|
| 1 | 101 |
| 1 | 102 |
| 2 | 101 |

Now:
- Student details depend entirely on `student_id`.
- Course details depend entirely on `course_id`.
- Enrollment depends on the combination of `student_id` and `course_id`.

**Result:** Partial dependencies are eliminated, and the tables satisfy 2NF.

### 3. Third Normal Form (3NF)
- Must already satisfy 2NF.
- Non-key columns should not depend on other non-key columns.
- Every non-key column should depend directly on the primary key.

**Example:** Consider an employee table:

**Before 3NF:**

| employee_id | employee_name | department_id | department_name |
|---|---|---|---|
| 1 | John | 101 | Engineering |
| 2 | Alice | 102 | HR |
| 3 | Bob | 101 | Engineering |

Here, `employee_id` is the primary key.

But:
- `employee_name` depends on `employee_id`.
- `department_id` depends on `employee_id`.
- `department_name` depends on `department_id`, not directly on `employee_id`.

This creates a **transitive dependency**:

`employee_id -> department_id -> department_name`

**Convert it into 3NF by splitting the table:**

**Employees**

| employee_id (PK) | employee_name | department_id (FK) |
|---|---|---|
| 1 | John | 101 |
| 2 | Alice | 102 |
| 3 | Bob | 101 |

**Departments**

| department_id (PK) | department_name |
|---|---|
| 101 | Engineering |
| 102 | HR |

Now:
- Employee details depend on `employee_id`.
- Department details depend on `department_id`.
- No non-key column depends on another non-key column.

**Result:** Transitive dependencies are eliminated, and the tables satisfy 3NF.

### 4. Boyce-Codd Normal Form (BCNF)
- A stronger version of 3NF.
- For every non-trivial functional dependency, the determinant must be a superkey.
- Helps handle certain cases that 3NF does not fully resolve.

**Example:** Consider a student-teacher-subject table:

**Before BCNF:**

| student_id | teacher_id | subject |
|---|---|---|
| 1 | 101 | Python |
| 2 | 101 | Python |
| 1 | 102 | SQL |
| 3 | 102 | SQL |

Assume:
- A teacher teaches only one subject.
- A student can have multiple teachers.
- Multiple students can have the same teacher.

Here, `(student_id, teacher_id)` is a candidate key.

But:
- `teacher_id` determines `subject`.
- `teacher_id` is not a superkey because multiple students can have the same teacher.

This violates BCNF.

**Convert it into BCNF by splitting the table:**

**Teacher_Subject**

| teacher_id (PK) | subject |
|---|---|
| 101 | Python |
| 102 | SQL |

**Student_Teacher**

| student_id | teacher_id |
|---|---|
| 1 | 101 |
| 2 | 101 |
| 1 | 102 |
| 3 | 102 |

Now:
- `teacher_id` determines `subject` and is a superkey in `Teacher_Subject`.
- The student-teacher relationship is stored separately.

**Result:** Every non-trivial functional dependency has a superkey as its determinant, and the tables satisfy BCNF.


## Indexes
- An index is a database object that helps retrieve data faster from a table.
- It works like an index in a book, allowing the database to find records without scanning the entire table.
- Indexes improve SELECT query performance but require additional storage and can slow down INSERT, UPDATE and DELETE operations.

### Index Syntax
```sql
CREATE INDEX index_name
ON table_name (column_name);
```

### Example: Employee Table

Suppose we frequently search for employees using their `employee_id`.

**1. Create an index**
```sql
CREATE INDEX idx_employee_id
ON employees (employee_id);
```

**2. Use the index automatically**
```sql
SELECT * FROM employees
WHERE employee_id = 101;
```

PostgreSQL can use the index to find the employee faster.

### Other Useful Commands

**View indexes:**
```sql
\d employees
```

**Delete an index:**
```sql
DROP INDEX idx_employee_id;
```

### Important Points
- Indexes speed up data retrieval.
- They require additional storage.
- They can slow down INSERT, UPDATE and DELETE operations.
- Primary keys and UNIQUE constraints automatically create indexes in PostgreSQL.


## Transactions
- A unit of work in DB terms is called a Transaction.
- Example: A bank system fund transfer.
- It Should be ACID compliant

## Database Locking Mechanism

- When several transactions try to access the same data simultaneously, one transaction might interfere with another, causing incorrect results.
- Database locking controls concurrent access to data and prevents such conflicts.
- A lock restricts how transactions can read or modify data.

### 1. Types of Locks

**A. Shared Lock (Read Lock)**
- Allows multiple transactions to read the same data simultaneously.
- Does not allow modification while the shared lock is held.

**B. Exclusive Lock (Write Lock)**
- Allows a transaction to modify data.
- Prevents other transactions from acquiring conflicting locks on the same data.

### 2. Locking Granularity

Refers to the amount of data being locked.

- **Database-level:** Locks the entire database.
- **Table-level:** Locks an entire table.
- **Page-level:** Locks a page containing multiple records.
- **Row-level:** Locks an individual row.

Smaller locking granularity generally allows more concurrent transactions.

### 3. Deadlock

- A deadlock occurs when two or more transactions wait for each other to release locks.
- Neither transaction can proceed.

**Example:**
- Transaction A locks Row 1 and waits for Row 2.
- Transaction B locks Row 2 and waits for Row 1.
- Both transactions are stuck waiting for each other.

**Solution:** DBMS detects the deadlock and rolls back one of the transactions to allow the other to proceed.

### 4. Example

```sql
BEGIN;

SELECT *
FROM accounts
WHERE account_id = 1
FOR UPDATE;

UPDATE accounts
SET balance = balance - 2000
WHERE account_id = 1;

COMMIT;
```


## Isolation Levels
- It determines the degree to which the operations in one transaction are isolated from those in other concurrent transactions.
- It controls how and when changes made by one transaction become visible to other transactions.
- Higher isolation levels provide greater data consistency but may reduce concurrency.

### Anomalies
These are unexpected behaviours that can occur when multiple transactions execute simultaneously.

1. **Dirty Read:** A transaction reads data modified by another transaction that has not yet been committed.

2. **Non-Repeatable Read:** A transaction reads the same row twice but gets different values because another transaction updated and committed that row in between.

3. **Phantom Read:** A transaction executes the same query twice but gets a different number of rows because another transaction inserted or deleted matching rows in between.

### Types of Isolation Levels

#### 1. Read Uncommitted
- Lowest isolation level.
- Transactions can read uncommitted changes made by other transactions.
- Dirty reads, non-repeatable reads and phantom reads are possible.

**Example:**
```sql
-- Transaction 1
UPDATE accounts SET balance = 500 WHERE id = 1;
-- Not committed yet

-- Transaction 2
SELECT balance FROM accounts WHERE id = 1;
-- Can read 500
```

#### 2. Read Committed
- A transaction can only read data that has been committed.
- Prevents dirty reads.
- Non-repeatable reads and phantom reads are possible.

**Example:**
```sql
-- Transaction 1
UPDATE accounts SET balance = 500 WHERE id = 1;
COMMIT;

-- Transaction 2
SELECT balance FROM accounts WHERE id = 1;
-- Reads 500
```
If another transaction updates the balance and commits before the next SELECT, the result may change.

#### 3. Repeatable Read
- Ensures that if a transaction reads a row multiple times, it gets the same values.
- Prevents dirty reads and non-repeatable reads.
- Phantom reads may be possible depending on the database implementation.

**Example:**
```sql
-- Transaction 1
BEGIN;
SELECT balance FROM accounts WHERE id = 1;
-- Reads 1000

-- Transaction 2
UPDATE accounts SET balance = 500 WHERE id = 1;
COMMIT;

-- Transaction 1
SELECT balance FROM accounts WHERE id = 1;
-- Still reads 1000
```

#### 4. Serializable
- Highest isolation level.
- Transactions behave as if they were executed one after another instead of simultaneously.
- Prevents dirty reads, non-repeatable reads and phantom reads.
- May cause transactions to be rolled back if they cannot be safely executed concurrently.

**Example:**
```sql
-- Transaction 1
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

SELECT SUM(balance) FROM accounts;

-- Transaction 2
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

INSERT INTO accounts (balance) VALUES (500);
```
The database ensures that the final result is equivalent to some serial execution order. One transaction may need to retry if a serialization conflict occurs.



## Triggers
- A trigger is a database object that automatically executes when a specific event occurs on a table.
- Commonly used for maintaining audit logs and tracking changes.

### Trigger Syntax
```sql
CREATE TRIGGER trigger_name
AFTER INSERT OR UPDATE OR DELETE
ON table_name
FOR EACH ROW
EXECUTE FUNCTION function_name();
```

- **WHEN:** Specifies when the trigger executes (BEFORE or AFTER).
- **WHAT:** Specifies the operation (INSERT, UPDATE, DELETE).

### Example: Employee Audit Log

Suppose we want to track whenever an employee is inserted, updated or deleted.

**1. Create an audit table**
```sql
CREATE TABLE employee_audit (
    employee_id INT,
    action VARCHAR(10),
    changed_at TIMESTAMP DEFAULT NOW()
);
```

**2. Create a trigger function**
```sql
CREATE OR REPLACE FUNCTION log_employee()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO employee_audit (employee_id, action)
    VALUES (
        COALESCE(NEW.employee_id, OLD.employee_id),
        TG_OP
    );

    RETURN NULL;
END;
$$ LANGUAGE plpgsql;
```

**3. Create the trigger**
```sql
CREATE TRIGGER employee_trigger
AFTER INSERT OR UPDATE OR DELETE
ON employees
FOR EACH ROW
EXECUTE FUNCTION log_employee();
```

Now, whenever an employee is inserted, updated or deleted, a record is automatically added to `employee_audit`.

## References
- [Learn Database Normalization by Decomplexify](https://www.youtube.com/watch?v=GFQaEYEc8_8)
- [Database Indexing For Dumb Developers by Laiture](https://www.youtube.com/watch?v=lYh6LrSIDvY)
- [What is a Database Transaction by Be a Better Dev](https://www.youtube.com/watch?v=wHUOeXbZCYA)
- [Isolation levels and its types by Riti Kumari](https://www.youtube.com/watch?v=8aOAgyOw420)
- [SQL Triggers (Visually Explained) by Data with Barra](https://www.youtube.com/watch?v=V81Smc6xrBY)
