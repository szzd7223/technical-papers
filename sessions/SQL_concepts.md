# DBMS Concepts – Session Notes

## 1. ACID Properties

ACID is a set of four properties that ensure database transactions are processed reliably.

### 1. Atomicity
- A transaction should either complete entirely or not happen at all.
- If one operation fails, all previous operations in that transaction are rolled back.

**Example:** Transferring ₹500 from Account A to Account B.
- ₹500 is deducted from A.
- ₹500 is added to B.
- If adding money to B fails, the deduction from A must also be reversed.

### 2. Consistency
- A transaction must keep the database in a valid state.
- It should follow all database rules and constraints.

**Example:** If a bank account cannot have a negative balance, a transaction should not allow the balance to become negative.

### 3. Isolation
- Multiple transactions running simultaneously should not interfere with each other.
- Each transaction should behave as if it is executing independently.

**Example:** Two people trying to book the same seat should not both successfully book it.

### 4. Durability
- Once a transaction is committed, its changes must be permanently saved.
- Even if the system crashes, committed data should remain.

**Example:** Once a bank transfer is successful, the updated balance should remain even after a server restart.

---

## 2. CAP Theorem

CAP theorem describes three properties of distributed databases.

It states that a distributed system cannot guarantee all three properties simultaneously during a network partition.

### 1. Consistency (C)
- Every user should see the latest available data.
- All nodes should return the same value for the same data.

**Example:** If your bank balance is updated to ₹5,000, every server should return ₹5,000.

### 2. Availability (A)
- Every request should receive a response, even if some servers are unavailable.
- The response might not contain the latest data.

**Example:** An online shopping application should continue showing products even if one server goes down.

### 3. Partition Tolerance (P)
- The system should continue operating even when communication between servers is interrupted.

**Example:** If two servers cannot communicate because of a network failure, the system should handle that situation.

### Understanding CAP

Imagine a database has two servers:

- Server A is in Bangalore.
- Server B is in Mumbai.
- Both normally communicate with each other.

Now imagine their network connection breaks.

The system has to choose between:

- **Consistency:** Reject or delay some requests to avoid returning potentially outdated data.
- **Availability:** Continue accepting requests, even if some responses might contain outdated data.

**Important:**
- During a network partition, a distributed system must trade off between Consistency and Availability.
- Partition Tolerance is generally necessary in distributed systems.

---

## 3. Database Transactions

A transaction is a group of database operations treated as a single unit of work.

A transaction either completes successfully or gets rolled back.

### Example: Bank Transfer

Suppose we want to transfer ₹500 from Account A to Account B.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

UPDATE accounts
SET balance = balance + 500
WHERE account_id = 2;

COMMIT;
```

### Important Transaction Commands

**1. BEGIN**
- Starts a new transaction.

**2. COMMIT**
- Saves all changes made during the transaction.

**3. ROLLBACK**
- Cancels all changes made during the transaction.

### Example of ROLLBACK

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

ROLLBACK;
```

The balance will remain unchanged because the transaction was rolled back.

**Remember:**
- COMMIT = Save changes.
- ROLLBACK = Undo changes.

---

## 4. Database Locking Mechanism

Locking is a mechanism used to control simultaneous access to database records.

It prevents multiple transactions from modifying the same data incorrectly.

### Types of Locks

### 1. Shared Lock (Read Lock)
- Allows multiple transactions to read the same data.
- Prevents other transactions from modifying that data while the lock is held.

**Example:** Multiple users viewing the same bank account balance.

### 2. Exclusive Lock (Write Lock)
- Allows only one transaction to modify the locked data.
- Prevents other transactions from reading or modifying it in conflicting ways, depending on the lock type.

**Example:** One user updating a bank account balance while another transaction tries to update the same record.

### Example in PostgreSQL

```sql
BEGIN;

SELECT *
FROM accounts
WHERE account_id = 1
FOR UPDATE;

UPDATE accounts
SET balance = balance - 500
WHERE account_id = 1;

COMMIT;
```

**What does FOR UPDATE do?**
- Locks the selected row.
- Other transactions trying to update or lock that row must wait until the transaction finishes.

**Remember:** Locks help prevent conflicting operations when multiple transactions access the same data.

---

## 5. Database Isolation Levels

Isolation levels define how much one transaction is isolated from other simultaneously running transactions.

They control which changes made by other transactions can be seen.

### Common Problems

**1. Dirty Read**
- Reading data that another transaction has modified but not yet committed.
- If that transaction rolls back, the data you read was never actually saved.

**2. Non-Repeatable Read**
- Reading the same row twice and getting different values because another transaction updated it in between.

**3. Phantom Read**
- Running the same query twice and getting a different number of rows because another transaction inserted or deleted matching records.

### Four Isolation Levels

### 1. Read Uncommitted
- Lowest isolation level.
- Allows dirty reads in database systems that support them.
- Other transaction changes may be visible before they are committed.

### 2. Read Committed
- A transaction can only read committed data.
- Prevents dirty reads.
- The same query may return different results if another transaction commits changes between executions.

### 3. Repeatable Read
- Ensures that rows read during a transaction remain consistent when read again.
- Prevents dirty reads and non-repeatable reads.
- Phantom reads may be possible depending on the database.

### 4. Serializable
- Highest isolation level.
- Transactions behave as if they were executed one after another.
- Prevents the major concurrency problems, though transactions may need to be retried.

### PostgreSQL Example

```sql
BEGIN TRANSACTION
ISOLATION LEVEL SERIALIZABLE;

SELECT *
FROM accounts;

COMMIT;
```

**Important PostgreSQL points:**
- Default isolation level is READ COMMITTED.
- READ UNCOMMITTED behaves like READ COMMITTED in PostgreSQL.
- PostgreSQL's REPEATABLE READ also prevents phantom reads.

---

## 6. Database Triggers

A trigger is a database operation that automatically executes when a specific event occurs on a table.

Triggers are commonly used for:
- Maintaining audit logs.
- Tracking changes.
- Automatically updating related information.

### Example: Employee Audit Log

Suppose we have two tables:

**Employee table**

```sql
CREATE TABLE employee (
    employee_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    salary INTEGER
);
```

**Employee Audit table**

```sql
CREATE TABLE employee_audit (
    audit_id SERIAL PRIMARY KEY,
    employee_id INTEGER,
    action VARCHAR(20),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

We want to automatically record whenever an employee is inserted or deleted.

### Step 1: Create a Trigger Function

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

**Understanding the important parts:**

- `RETURNS TRIGGER`: This function is designed to be used by a trigger.
- `BEGIN ... END`: Contains the operations to execute.
- `NEW`: Contains the new row (for INSERT).
- `OLD`: Contains the previous row (for DELETE).
- `TG_OP`: Tells us which operation happened, such as INSERT or DELETE.
- `COALESCE()`: Returns the first non-NULL value.
- `RETURN NULL`: Appropriate for this AFTER trigger because we don't need to modify the original row.

### Step 2: Create the Trigger

```sql
CREATE TRIGGER employee_log_trigger
AFTER INSERT OR DELETE
ON employee
FOR EACH ROW
EXECUTE FUNCTION log_employee();
```

**Understanding the trigger:**

- `CREATE TRIGGER`: Creates a trigger.
- `AFTER INSERT OR DELETE`: Runs after an insert or delete operation.
- `ON employee`: Trigger is attached to the employee table.
- `FOR EACH ROW`: Runs once for every affected row.
- `EXECUTE FUNCTION`: Specifies which function to execute.

### Step 3: Test the Trigger

```sql
INSERT INTO employee (name, salary)
VALUES ('John', 30000);
```

Now check the audit table:

```sql
SELECT * FROM employee_audit;
```

You will see a new record containing:
- Employee ID.
- Action: INSERT.
- Timestamp.

The audit record was created automatically without explicitly inserting anything into the audit table yourself.

**Remember:**
- Function contains the logic.
- Trigger decides when that function executes.
- Triggers execute automatically when their specified event occurs.
