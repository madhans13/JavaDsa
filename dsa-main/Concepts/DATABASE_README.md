# Database Concepts - Interview Preparation Guide

A comprehensive guide covering basic to advanced database concepts for technical interviews.

---

## Table of Contents
1. [Basic SQL Queries](#basic-sql-queries)
2. [Joins](#joins)
3. [Aggregations and GROUP BY](#aggregations-and-group-by)
4. [ORDER BY and Sorting](#order-by-and-sorting)
5. [Query Execution Order](#query-execution-order)
6. [Indexing](#indexing)
7. [ACID Principles](#acid-principles)
8. [Normalization](#normalization)
9. [Important Differences](#important-differences)
10. [Transactions](#transactions)
11. [Advanced Concepts](#advanced-concepts)
12. [Interview FAQs](#interview-faqs)

---

## Basic SQL Queries

### SELECT Statement
```sql
-- Basic SELECT
SELECT column1, column2 FROM table_name;

-- SELECT with WHERE clause
SELECT * FROM employees WHERE salary > 50000;

-- SELECT DISTINCT
SELECT DISTINCT department FROM employees;

-- SELECT with wildcards
SELECT * FROM employees WHERE name LIKE 'J%';
```

### INSERT Statement
```sql
INSERT INTO employees (id, name, salary) 
VALUES (1, 'John Doe', 60000);

-- Multiple rows
INSERT INTO employees VALUES 
(2, 'Jane Smith', 65000),
(3, 'Bob Johnson', 55000);
```

### UPDATE Statement
```sql
UPDATE employees 
SET salary = 70000 
WHERE id = 1;

-- Update multiple columns
UPDATE employees 
SET salary = salary * 1.1, department = 'IT' 
WHERE department = 'Tech';
```

### DELETE Statement
```sql
DELETE FROM employees WHERE id = 1;

-- Delete with condition
DELETE FROM employees WHERE hire_date < '2020-01-01';
```

---

## Joins

### Types of Joins

#### INNER JOIN
Returns records that have matching values in both tables.
```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
```

#### LEFT JOIN (LEFT OUTER JOIN)
Returns all records from the left table and matched records from the right table.
```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
```

#### RIGHT JOIN (RIGHT OUTER JOIN)
Returns all records from the right table and matched records from the left table.
```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
```

#### FULL OUTER JOIN
Returns all records when there is a match in either left or right table.
```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
```

#### CROSS JOIN
Returns the Cartesian product of both tables.
```sql
SELECT e.name, d.department_name
FROM employees e
CROSS JOIN departments d;
```

#### SELF JOIN
Join a table to itself.
```sql
SELECT e1.name AS Employee, e2.name AS Manager
FROM employees e1
INNER JOIN employees e2 ON e1.manager_id = e2.id;
```

---

## Aggregations and GROUP BY

### Aggregate Functions
```sql
-- COUNT
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department) FROM employees;

-- SUM
SELECT SUM(salary) FROM employees;

-- AVG
SELECT AVG(salary) FROM employees;

-- MIN and MAX
SELECT MIN(salary), MAX(salary) FROM employees;
```

### GROUP BY
```sql
-- Basic GROUP BY
SELECT department, COUNT(*) as emp_count
FROM employees
GROUP BY department;

-- GROUP BY with multiple columns
SELECT department, job_title, AVG(salary)
FROM employees
GROUP BY department, job_title;

-- GROUP BY with HAVING (filter after grouping)
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

### Difference: WHERE vs HAVING
- **WHERE**: Filters rows BEFORE grouping
- **HAVING**: Filters groups AFTER grouping

```sql
-- WHERE filters individual rows
SELECT department, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department;

-- HAVING filters grouped results
SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

---

## ORDER BY and Sorting

```sql
-- Ascending order (default)
SELECT * FROM employees ORDER BY salary;
SELECT * FROM employees ORDER BY salary ASC;

-- Descending order
SELECT * FROM employees ORDER BY salary DESC;

-- Multiple column sorting
SELECT * FROM employees 
ORDER BY department ASC, salary DESC;

-- ORDER BY with column position
SELECT name, salary FROM employees ORDER BY 2 DESC;

-- ORDER BY with expressions
SELECT name, salary, salary * 12 as annual_salary
FROM employees
ORDER BY annual_salary DESC;
```

---

## Query Execution Order

**Important for Interviews!**

### Logical Order of SQL Query Execution:

1. **FROM** - Tables are identified and joined
2. **WHERE** - Rows are filtered based on conditions
3. **GROUP BY** - Rows are grouped
4. **HAVING** - Groups are filtered
5. **SELECT** - Columns are selected and expressions calculated
6. **DISTINCT** - Duplicate rows are removed
7. **ORDER BY** - Results are sorted
8. **LIMIT/OFFSET** - Result set is limited

```sql
SELECT department, AVG(salary) as avg_sal          -- 5. Calculate averages
FROM employees                                      -- 1. Get data from table
WHERE hire_date > '2020-01-01'                     -- 2. Filter rows
GROUP BY department                                 -- 3. Group by department
HAVING AVG(salary) > 50000                         -- 4. Filter groups
ORDER BY avg_sal DESC                               -- 6. Sort results
LIMIT 10;                                          -- 7. Limit output
```

**Why This Matters:**
- You can't use column aliases in WHERE (alias created in SELECT)
- You CAN use aliases in ORDER BY (executed after SELECT)
- HAVING must use aggregate functions or GROUP BY columns

---

## Indexing

### What is an Index?
A database index is a data structure that improves the speed of data retrieval operations on a table at the cost of additional storage and slower writes.

### Types of Indexes

#### 1. Primary Index
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,  -- Automatically creates primary index
    name VARCHAR(100)
);
```

#### 2. Unique Index
```sql
CREATE UNIQUE INDEX idx_email ON employees(email);
```

#### 3. Composite Index
```sql
CREATE INDEX idx_name_dept ON employees(name, department);
```

#### 4. Clustered Index
- Determines the physical order of data in a table
- Only one clustered index per table
- Usually the primary key

#### 5. Non-Clustered Index
- Separate structure from the data rows
- Can have multiple non-clustered indexes per table

### Creating and Managing Indexes
```sql
-- Create index
CREATE INDEX idx_salary ON employees(salary);

-- Create composite index
CREATE INDEX idx_dept_salary ON employees(department, salary);

-- Drop index
DROP INDEX idx_salary;

-- View indexes
SHOW INDEX FROM employees;
```

### When to Use Indexes
✅ **Use indexes when:**
- Columns frequently used in WHERE clauses
- Columns used in JOIN conditions
- Columns used in ORDER BY
- Large tables with many reads
- Foreign key columns

❌ **Avoid indexes when:**
- Small tables
- Columns with frequent INSERT/UPDATE/DELETE
- Columns with low cardinality (few unique values)
- Columns rarely used in queries

### Index Performance Considerations
```sql
-- Good: Uses index
SELECT * FROM employees WHERE id = 100;

-- Bad: Function on indexed column prevents index usage
SELECT * FROM employees WHERE UPPER(name) = 'JOHN';

-- Good: Rewrite to use index
SELECT * FROM employees WHERE name = 'john' OR name = 'JOHN';
```

---

## ACID Principles

**ACID** properties ensure reliable database transactions.

### 1. Atomicity
**"All or Nothing"** - A transaction is treated as a single unit.

```sql
BEGIN TRANSACTION;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;  -- Both updates succeed

-- If any statement fails, entire transaction is rolled back
```

**Example:** If transferring money between accounts, either both debit and credit happen, or neither happens.

### 2. Consistency
**"Data Integrity Rules"** - Transaction brings database from one valid state to another.

**Example:** 
- All constraints are satisfied
- If a foreign key relationship exists, it must remain valid
- Check constraints are enforced

```sql
-- This maintains consistency
INSERT INTO orders (customer_id, product_id)
VALUES (1, 100);  -- customer_id 1 must exist in customers table
```

### 3. Isolation
**"Concurrent Transactions Don't Interfere"** - Transactions are isolated from each other.

**Isolation Levels:**
```sql
-- Read Uncommitted (lowest isolation)
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Read Committed
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Repeatable Read
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Serializable (highest isolation)
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### 4. Durability
**"Changes are Permanent"** - Once committed, changes persist even after system failure.

**Example:** After COMMIT, data is written to disk and survives crashes.

---

## Normalization

### Why Normalize?
- Eliminate data redundancy
- Ensure data dependencies make sense
- Reduce data anomalies

### Normal Forms

#### 1NF (First Normal Form)
- Each column contains atomic (indivisible) values
- Each column contains values of a single type
- Each row is unique

**Before 1NF:**
```
| Student | Courses              |
|---------|---------------------|
| John    | Math, Physics, CS   |
```

**After 1NF:**
```
| Student | Course  |
|---------|---------|
| John    | Math    |
| John    | Physics |
| John    | CS      |
```

#### 2NF (Second Normal Form)
- Must be in 1NF
- All non-key attributes fully dependent on primary key
- No partial dependencies

**Before 2NF:**
```
| StudentID | CourseID | StudentName | CourseName |
```
(StudentName depends only on StudentID, not on the full key)

**After 2NF:**
```
Students: | StudentID | StudentName |
Courses:  | CourseID  | CourseName  |
Enrollment: | StudentID | CourseID |
```

#### 3NF (Third Normal Form)
- Must be in 2NF
- No transitive dependencies
- Non-key attributes depend only on primary key

**Before 3NF:**
```
| EmployeeID | Department | DeptHead |
```
(DeptHead depends on Department, which depends on EmployeeID - transitive)

**After 3NF:**
```
Employees:   | EmployeeID | Department |
Departments: | Department | DeptHead   |
```

#### BCNF (Boyce-Codd Normal Form)
- Stricter version of 3NF
- Every determinant is a candidate key

---

## Important Differences

### 1. DELETE vs TRUNCATE vs DROP

| Feature | DELETE | TRUNCATE | DROP |
|---------|--------|----------|------|
| **Type** | DML | DDL | DDL |
| **WHERE clause** | ✅ Yes | ❌ No | ❌ No |
| **Rollback** | ✅ Yes | ❌ No (in most DBMS) | ❌ No |
| **Speed** | Slow | Fast | Fast |
| **Triggers** | Fires | Doesn't fire | Doesn't fire |
| **Identity reset** | ❌ No | ✅ Yes | ✅ Yes |
| **Locks** | Row-level | Table-level | Table-level |
| **Storage** | Doesn't release | Releases | Releases |

```sql
-- DELETE: Removes specific rows, can rollback, slower
DELETE FROM employees WHERE department = 'Sales';
ROLLBACK;  -- Can undo

-- TRUNCATE: Removes all rows, fast, can't use WHERE
TRUNCATE TABLE employees;  -- Removes all data, resets identity

-- DROP: Removes entire table structure
DROP TABLE employees;  -- Table no longer exists
```

### 2. WHERE vs HAVING

| WHERE | HAVING |
|-------|--------|
| Filters rows before grouping | Filters groups after grouping |
| Cannot use aggregate functions | Can use aggregate functions |
| Used with SELECT, UPDATE, DELETE | Used only with GROUP BY |
| Processed before GROUP BY | Processed after GROUP BY |

```sql
-- WHERE: Filter before grouping
SELECT department, COUNT(*)
FROM employees
WHERE salary > 50000  -- Filter individual rows
GROUP BY department;

-- HAVING: Filter after grouping
SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;  -- Filter grouped results
```

### 3. UNION vs UNION ALL

| UNION | UNION ALL |
|-------|-----------|
| Removes duplicates | Keeps duplicates |
| Slower | Faster |
| Sorts results | No sorting |

```sql
-- UNION: Removes duplicates
SELECT name FROM employees_2023
UNION
SELECT name FROM employees_2024;

-- UNION ALL: Keeps duplicates (faster)
SELECT name FROM employees_2023
UNION ALL
SELECT name FROM employees_2024;
```

### 4. Primary Key vs Unique Key

| Primary Key | Unique Key |
|-------------|------------|
| Only one per table | Multiple allowed |
| Cannot be NULL | Can be NULL (once) |
| Creates clustered index | Creates non-clustered index |
| Identifies each row | Ensures uniqueness |

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,           -- Only one primary key
    email VARCHAR(100) UNIQUE,    -- Can have multiple unique keys
    ssn VARCHAR(11) UNIQUE
);
```

### 5. Clustered vs Non-Clustered Index

| Clustered | Non-Clustered |
|-----------|---------------|
| Determines physical order | Separate structure |
| One per table | Multiple per table |
| Faster for range queries | Faster for specific lookups |
| Leaf nodes contain data | Leaf nodes contain pointers |

### 6. Inner Join vs Outer Join

```sql
-- INNER JOIN: Only matching records
SELECT * FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;

-- LEFT OUTER JOIN: All from left + matches from right
SELECT * FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;

-- RIGHT OUTER JOIN: All from right + matches from left
SELECT * FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;

-- FULL OUTER JOIN: All records from both
SELECT * FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
```

### 7. Stored Procedure vs Function

| Stored Procedure | Function |
|-----------------|----------|
| Can return zero, one, or many values | Must return a value |
| Can use DML (INSERT, UPDATE, DELETE) | Cannot use DML (in most DBMS) |
| Cannot be called from SELECT | Can be called from SELECT |
| Can have transactions | Cannot have transactions |

```sql
-- Stored Procedure
CREATE PROCEDURE GetEmployees()
BEGIN
    SELECT * FROM employees;
END;

CALL GetEmployees();

-- Function
CREATE FUNCTION GetEmployeeCount() RETURNS INT
BEGIN
    RETURN (SELECT COUNT(*) FROM employees);
END;

SELECT GetEmployeeCount();
```

### 8. VARCHAR vs CHAR

| VARCHAR | CHAR |
|---------|------|
| Variable length | Fixed length |
| Uses only needed space | Uses full allocated space |
| Better for varying lengths | Better for fixed lengths |
| Slight performance overhead | Faster for fixed data |

```sql
-- Use CHAR for fixed-length data
state CHAR(2)  -- Always 2 characters: 'CA', 'NY'

-- Use VARCHAR for variable-length data
name VARCHAR(100)  -- Can be 1-100 characters
```

### 9. RANK() vs DENSE_RANK() vs ROW_NUMBER()

```sql
-- Sample data: scores = 95, 95, 90, 85

-- ROW_NUMBER(): 1, 2, 3, 4 (unique sequential)
SELECT name, score,
    ROW_NUMBER() OVER (ORDER BY score DESC) as row_num
FROM students;

-- RANK(): 1, 1, 3, 4 (skips ranks after ties)
SELECT name, score,
    RANK() OVER (ORDER BY score DESC) as rank
FROM students;

-- DENSE_RANK(): 1, 1, 2, 3 (no gaps in ranking)
SELECT name, score,
    DENSE_RANK() OVER (ORDER BY score DESC) as dense_rank
FROM students;
```

### 10. SQL vs NoSQL

| SQL | NoSQL |
|-----|-------|
| Structured, relational | Unstructured, non-relational |
| Fixed schema | Dynamic schema |
| ACID compliant | BASE (eventual consistency) |
| Vertical scaling | Horizontal scaling |
| Examples: MySQL, PostgreSQL | Examples: MongoDB, Cassandra |
| Best for complex queries | Best for large data volumes |

---

## Transactions

### What is a Transaction?
A sequence of database operations that are treated as a single unit of work.

### Transaction Commands

```sql
-- Start transaction
BEGIN TRANSACTION;
-- or
START TRANSACTION;

-- Commit (save changes)
COMMIT;

-- Rollback (undo changes)
ROLLBACK;

-- Savepoint (partial rollback)
SAVEPOINT savepoint_name;
ROLLBACK TO savepoint_name;
```

### Transaction Example
```sql
BEGIN TRANSACTION;

UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;

-- Check if both updates succeeded
IF @@ERROR = 0
    COMMIT;
ELSE
    ROLLBACK;
```

### Isolation Levels and Problems

#### 1. Read Uncommitted
- Lowest isolation level
- **Dirty Reads**: Can read uncommitted changes
```sql
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

#### 2. Read Committed
- Prevents dirty reads
- **Non-Repeatable Reads**: Same query may return different results
```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

#### 3. Repeatable Read
- Prevents dirty and non-repeatable reads
- **Phantom Reads**: New rows can appear
```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

#### 4. Serializable
- Highest isolation level
- Prevents all concurrency issues
- Slowest performance
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### Concurrency Problems

| Problem | Description | Example |
|---------|-------------|---------|
| **Dirty Read** | Reading uncommitted data | T1 updates but hasn't committed; T2 reads the update |
| **Non-Repeatable Read** | Same query returns different results | T1 reads data; T2 updates and commits; T1 reads again |
| **Phantom Read** | New rows appear in repeated queries | T1 queries range; T2 inserts row; T1 queries again |
| **Lost Update** | One transaction overwrites another | T1 and T2 both read, then both update same row |

---

## Advanced Concepts

### 1. Subqueries

```sql
-- Scalar subquery (returns single value)
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Correlated subquery (references outer query)
SELECT e1.name, e1.salary
FROM employees e1
WHERE salary > (
    SELECT AVG(salary) 
    FROM employees e2 
    WHERE e2.department = e1.department
);

-- Subquery in FROM clause
SELECT dept, avg_salary
FROM (
    SELECT department as dept, AVG(salary) as avg_salary
    FROM employees
    GROUP BY department
) AS dept_avg
WHERE avg_salary > 60000;

-- EXISTS subquery
SELECT name
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.employee_id = e.id
);
```

### 2. Common Table Expressions (CTE)

```sql
-- Simple CTE
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 80000
)
SELECT * FROM high_earners WHERE department = 'IT';

-- Multiple CTEs
WITH 
dept_avg AS (
    SELECT department, AVG(salary) as avg_sal
    FROM employees
    GROUP BY department
),
high_avg_depts AS (
    SELECT department FROM dept_avg WHERE avg_sal > 60000
)
SELECT * FROM employees
WHERE department IN (SELECT department FROM high_avg_depts);

-- Recursive CTE (employee hierarchy)
WITH RECURSIVE emp_hierarchy AS (
    -- Base case
    SELECT id, name, manager_id, 1 as level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive case
    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    INNER JOIN emp_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM emp_hierarchy;
```

### 3. Window Functions

```sql
-- Running total
SELECT 
    date,
    amount,
    SUM(amount) OVER (ORDER BY date) as running_total
FROM sales;

-- Rank by department
SELECT 
    name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank
FROM employees;

-- Moving average
SELECT 
    date,
    sales,
    AVG(sales) OVER (
        ORDER BY date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) as moving_avg_3day
FROM daily_sales;

-- Lead and Lag
SELECT 
    date,
    price,
    LAG(price, 1) OVER (ORDER BY date) as prev_price,
    LEAD(price, 1) OVER (ORDER BY date) as next_price
FROM stock_prices;
```

### 4. Pivot and Unpivot

```sql
-- PIVOT: Rows to columns
SELECT *
FROM (
    SELECT year, quarter, sales
    FROM quarterly_sales
) AS source
PIVOT (
    SUM(sales)
    FOR quarter IN ([Q1], [Q2], [Q3], [Q4])
) AS pivoted;

-- Using CASE for PIVOT (standard SQL)
SELECT 
    year,
    SUM(CASE WHEN quarter = 'Q1' THEN sales ELSE 0 END) as Q1,
    SUM(CASE WHEN quarter = 'Q2' THEN sales ELSE 0 END) as Q2,
    SUM(CASE WHEN quarter = 'Q3' THEN sales ELSE 0 END) as Q3,
    SUM(CASE WHEN quarter = 'Q4' THEN sales ELSE 0 END) as Q4
FROM quarterly_sales
GROUP BY year;
```

### 5. Views

```sql
-- Create view
CREATE VIEW high_salary_employees AS
SELECT id, name, salary, department
FROM employees
WHERE salary > 80000;

-- Use view
SELECT * FROM high_salary_employees WHERE department = 'IT';

-- Materialized view (stores results physically)
CREATE MATERIALIZED VIEW dept_summary AS
SELECT department, COUNT(*) as emp_count, AVG(salary) as avg_salary
FROM employees
GROUP BY department;

-- Refresh materialized view
REFRESH MATERIALIZED VIEW dept_summary;
```

### 6. Triggers

```sql
-- BEFORE INSERT trigger
CREATE TRIGGER check_salary_before_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF NEW.salary < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary cannot be negative';
    END IF;
END;

-- AFTER UPDATE trigger (audit log)
CREATE TRIGGER log_salary_changes
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF OLD.salary != NEW.salary THEN
        INSERT INTO salary_audit (emp_id, old_salary, new_salary, changed_at)
        VALUES (NEW.id, OLD.salary, NEW.salary, NOW());
    END IF;
END;
```

### 7. Constraints

```sql
-- Primary Key
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

-- Foreign Key
CREATE TABLE orders (
    id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);

-- Unique
ALTER TABLE employees ADD CONSTRAINT uk_email UNIQUE (email);

-- Check
CREATE TABLE products (
    id INT PRIMARY KEY,
    price DECIMAL(10,2),
    CONSTRAINT chk_price CHECK (price > 0)
);

-- Not Null
ALTER TABLE employees MODIFY COLUMN name VARCHAR(100) NOT NULL;

-- Default
CREATE TABLE orders (
    id INT PRIMARY KEY,
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 8. Performance Optimization Tips

```sql
-- 1. Use EXPLAIN to analyze queries
EXPLAIN SELECT * FROM employees WHERE department = 'IT';

-- 2. Avoid SELECT *
-- Bad
SELECT * FROM employees;
-- Good
SELECT id, name, salary FROM employees;

-- 3. Use indexes on WHERE, JOIN, ORDER BY columns
CREATE INDEX idx_dept ON employees(department);

-- 4. Avoid functions on indexed columns
-- Bad
SELECT * FROM employees WHERE YEAR(hire_date) = 2023;
-- Good
SELECT * FROM employees WHERE hire_date >= '2023-01-01' AND hire_date < '2024-01-01';

-- 5. Use EXISTS instead of IN for subqueries
-- Less efficient
SELECT * FROM customers WHERE id IN (SELECT customer_id FROM orders);
-- More efficient
SELECT * FROM customers c WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.id);

-- 6. Use LIMIT for large result sets
SELECT * FROM employees ORDER BY salary DESC LIMIT 10;

-- 7. Avoid OR on different columns, use UNION
-- Slower
SELECT * FROM employees WHERE department = 'IT' OR salary > 100000;
-- Faster (if both columns are indexed)
SELECT * FROM employees WHERE department = 'IT'
UNION
SELECT * FROM employees WHERE salary > 100000;
```

---

## Interview FAQs

### Conceptual Questions

**Q1: What is a Database?**
A: A structured collection of data stored electronically, managed by a DBMS (Database Management System) that allows data to be easily accessed, managed, and updated.

**Q2: What is DBMS?**
A: Database Management System - software that handles the storage, retrieval, and updating of data in a database. Examples: MySQL, PostgreSQL, Oracle, SQL Server.

**Q3: Difference between DBMS and RDBMS?**
| DBMS | RDBMS |
|------|-------|
| Stores data as files | Stores data in tables |
| No relationships | Relationships via foreign keys |
| No ACID properties | ACID compliant |
| Example: File systems | Example: MySQL, PostgreSQL |

**Q4: What is a Schema?**
A: The logical structure of a database, defining tables, columns, relationships, constraints, and other database objects.

**Q5: What is Data Integrity?**
A: Accuracy, consistency, and reliability of data stored in a database. Maintained through:
- Entity Integrity (Primary Keys)
- Referential Integrity (Foreign Keys)
- Domain Integrity (Data types, constraints)
- User-Defined Integrity (Business rules)

**Q6: What is a Cursor?**
A: A database object used to retrieve, manipulate, and navigate through a result set row by row.

```sql
DECLARE cursor_name CURSOR FOR SELECT * FROM employees;
OPEN cursor_name;
FETCH NEXT FROM cursor_name;
CLOSE cursor_name;
```

**Q7: What are OLTP and OLAP?**
- **OLTP (Online Transaction Processing)**: Handles day-to-day transactions (INSERT, UPDATE, DELETE). Fast, normalized databases.
- **OLAP (Online Analytical Processing)**: Handles complex queries and analysis. Denormalized, optimized for reads.

**Q8: What is Denormalization?**
A: The process of adding redundant data to improve read performance by reducing the need for joins. Trade-off between storage space and query speed.

**Q9: What is a Deadlock?**
A: A situation where two or more transactions are waiting for each other to release locks, creating a circular dependency.

```
Transaction 1: Locks A, waits for B
Transaction 2: Locks B, waits for A
→ Deadlock!
```

**Q10: How to prevent Deadlocks?**
- Access objects in the same order
- Keep transactions short
- Use appropriate isolation levels
- Set lock timeouts
- Implement retry logic

### Query-Based Questions

**Q11: Find the Nth highest salary**
```sql
-- Method 1: Using LIMIT and OFFSET
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET N-1;

-- Method 2: Using subquery
SELECT MAX(salary)
FROM employees
WHERE salary < (
    SELECT MAX(salary) FROM employees
);  -- For 2nd highest

-- Method 3: Using DENSE_RANK
SELECT salary
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rank
    FROM employees
) ranked
WHERE rank = N;
```

**Q12: Find duplicate records**
```sql
-- Find duplicate names
SELECT name, COUNT(*)
FROM employees
GROUP BY name
HAVING COUNT(*) > 1;

-- Get all duplicate records
SELECT *
FROM employees
WHERE name IN (
    SELECT name
    FROM employees
    GROUP BY name
    HAVING COUNT(*) > 1
);
```

**Q13: Delete duplicate records, keep one**
```sql
-- Using ROW_NUMBER
WITH CTE AS (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY name, email ORDER BY id) as rn
    FROM employees
)
DELETE FROM CTE WHERE rn > 1;

-- Using self-join (MySQL)
DELETE e1 FROM employees e1
INNER JOIN employees e2
WHERE e1.id > e2.id AND e1.email = e2.email;
```

**Q14: Find employees with salary higher than their manager**
```sql
SELECT e.name as employee, e.salary as emp_salary,
       m.name as manager, m.salary as mgr_salary
FROM employees e
INNER JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

**Q15: Find departments with no employees**
```sql
SELECT d.department_name
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
WHERE e.id IS NULL;

-- Or using NOT EXISTS
SELECT department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1 FROM employees e WHERE e.department_id = d.id
);
```

**Q16: Display running total/cumulative sum**
```sql
SELECT 
    date,
    sales,
    SUM(sales) OVER (ORDER BY date) as cumulative_sales
FROM daily_sales;

-- Without window functions (less efficient)
SELECT 
    d1.date,
    d1.sales,
    (SELECT SUM(d2.sales) FROM daily_sales d2 WHERE d2.date <= d1.date) as cumulative_sales
FROM daily_sales d1;
```

**Q17: Find records present in one table but not in another**
```sql
-- Using NOT EXISTS
SELECT * FROM table1 t1
WHERE NOT EXISTS (
    SELECT 1 FROM table2 t2 WHERE t2.id = t1.id
);

-- Using LEFT JOIN
SELECT t1.*
FROM table1 t1
LEFT JOIN table2 t2 ON t1.id = t2.id
WHERE t2.id IS NULL;

-- Using EXCEPT (if supported)
SELECT id FROM table1
EXCEPT
SELECT id FROM table2;
```

**Q18: Self-join to find employees in same department**
```sql
SELECT e1.name as employee1, e2.name as employee2, e1.department
FROM employees e1
INNER JOIN employees e2 
    ON e1.department = e2.department 
    AND e1.id < e2.id;  -- Avoid duplicates
```

---

## SQL Practice Scenarios

### Scenario 1: Employee Database
```sql
-- Table structure
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    manager_id INT,
    hire_date DATE
);

-- Common queries
-- 1. Average salary by department
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;

-- 2. Employees hired in last 6 months
SELECT * FROM employees
WHERE hire_date >= DATE_SUB(CURRENT_DATE, INTERVAL 6 MONTH);

-- 3. Department with highest average salary
SELECT department, AVG(salary) as avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC
LIMIT 1;
```

### Scenario 2: E-commerce Database
```sql
-- Find top 5 customers by total purchase amount
SELECT c.name, SUM(o.amount) as total_spent
FROM customers c
INNER JOIN orders o ON c.id = o.customer_id
GROUP BY c.id, c.name
ORDER BY total_spent DESC
LIMIT 5;

-- Products never ordered
SELECT p.product_name
FROM products p
LEFT JOIN order_items oi ON p.id = oi.product_id
WHERE oi.product_id IS NULL;

-- Monthly sales trend
SELECT 
    DATE_FORMAT(order_date, '%Y-%m') as month,
    SUM(amount) as monthly_sales,
    COUNT(*) as order_count
FROM orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY month;
```

---

## Quick Reference Cheat Sheet

### Most Common Commands
```sql
-- Data Query
SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY, LIMIT

-- Data Modification
INSERT INTO, UPDATE, DELETE, TRUNCATE

-- Table Operations
CREATE TABLE, ALTER TABLE, DROP TABLE

-- Constraints
PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK, DEFAULT

-- Joins
INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN, CROSS JOIN

-- Aggregate Functions
COUNT(), SUM(), AVG(), MIN(), MAX()

-- String Functions
CONCAT(), SUBSTRING(), UPPER(), LOWER(), TRIM(), LENGTH()

-- Date Functions
NOW(), CURDATE(), DATE_FORMAT(), DATEDIFF(), DATE_ADD()

-- Conditional
CASE WHEN, IF(), COALESCE(), NULLIF()

-- Window Functions
ROW_NUMBER(), RANK(), DENSE_RANK(), LEAD(), LAG()
```

---

## Best Practices for Interviews

1. **Understand the problem before coding**
2. **Ask clarifying questions** about data volume, expected performance
3. **Think about edge cases** (NULL values, duplicates, empty tables)
4. **Optimize your queries** (use indexes, avoid unnecessary joins)
5. **Explain your approach** before writing the query
6. **Test with sample data** mentally
7. **Know time complexity** of your queries
8. **Be ready to explain** ACID, normalization, indexing
9. **Practice on platforms**: LeetCode, HackerRank, SQLZoo
10. **Review query execution plans** using EXPLAIN

---

## Additional Resources

- **Practice Platforms**: LeetCode SQL, HackerRank, SQLZoo, Mode Analytics
- **Documentation**: MySQL Docs, PostgreSQL Docs, Oracle Docs
- **Books**: "SQL Performance Explained", "Database System Concepts"
- **YouTube Channels**: TechTFQ, Programming with Mosh

---

**Remember**: Interviews focus on both theoretical knowledge and practical problem-solving. Practice writing queries by hand and explaining your thought process clearly!

---

*Last Updated: 2024*
*Good luck with your interviews! 🚀*