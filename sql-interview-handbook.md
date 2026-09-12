# 🗄️ Ultimate SQL Interview Handbook & Complete Engineering Guide

A comprehensive, production-grade, and interview-ready guide covering Relational Database Management Systems (RDBMS), SQL fundamentals, query optimization, high-frequency coding patterns, schema design, and concurrency internals.

---

## 📑 Table of Contents
1. [Core DBMS & Relational Architecture](#1-core-dbms--relational-architecture)
2. [SQL Sub-Languages & Core Commands](#2-sql-sub-languages--core-commands)
3. [Keys & Constraints Architecture](#3-keys--constraints-architecture)
4. [DML, DDL & DQL Essentials (`DELETE` vs `DROP` vs `TRUNCATE`, `WHERE` vs `HAVING`)](#4-dml-ddl--dql-essentials)
5. [SQL Logical Execution Order](#5-sql-logical-execution-order)
6. [Joins Deep Dive & Visual Venn Mechanics](#6-joins-deep-dive--visual-venn-mechanics)
7. [Aggregations & Grouping Masterclass](#7-aggregations--grouping-masterclass)
8. [🔥 Top Must-Code SQL Interview Queries](#8--top-must-code-sql-interview-queries)
9. [Advanced SQL (Window Functions, CTEs, Recursive Queries, Subqueries)](#9-advanced-sql)
10. [Set Operations, Conditional Logic & NULL Mechanics](#10-set-operations-conditional-logic--null-mechanics)
11. [Indexing, Storage Internals & Performance Tuning](#11-indexing-storage-internals--performance-tuning)
12. [Database Normalization & Schema Design](#12-database-normalization--schema-design)
13. [Transactions, ACID & Concurrency Control (Isolation Levels & Deadlocks)](#13-transactions-acid--concurrency-control)

---

# 1. Core DBMS & Relational Architecture

### Q1: What is a DBMS?
**Answer:**
A **Database Management System (DBMS)** is system software that enables users and applications to create, define, query, update, and administer databases. It acts as an interface between end-users/applications and the underlying physical storage.

**Core Responsibilities:**
- **Data Definition & Storage:** Managing how data sits on disk / pages.
- **Data Retrieval & Manipulation:** Parsing, optimizing, and executing queries.
- **Data Integrity & Security:** Enforcing schema constraints, access control, and user permissions.
- **Concurrency & Recovery:** Handling multiple simultaneous users and restoring data after system crashes.

---

### Q2: DBMS vs RDBMS vs NoSQL

| Feature | DBMS (Traditional / File / Hierarchical) | RDBMS (Relational) | NoSQL (Document, Key-Value, Graph, Columnar) |
| :--- | :--- | :--- | :--- |
| **Data Structure** | Flat files, Hierarchical trees, or Network records | Tabular (Rows/Tuples and Columns/Attributes) | JSON documents, Key-Value pairs, Column families, Graph nodes/edges |
| **Relationship Model** | No explicit relationship enforcement | Enforced via Foreign Keys & Referential Integrity | Typically denormalized / embedded or graph edges |
| **Schema** | Unstructured or proprietary | Fixed, strictly defined schema | Dynamic / Schema-on-read |
| **Data Integrity & Normalization** | Low; high redundancy | High; normalized to 3NF/BCNF | Redundancy embraced for read performance |
| **Transaction Model** | Often Non-ACID | **Strict ACID** compliant | **BASE** (Basically Available, Soft state, Eventual consistency) or Tunable ACID |
| **Scaling** | Vertical | Primarily Vertical (Scale-up); Sharding possible | Horizontal (Scale-out across clusters) |
| **Examples** | File System, MS Access, XML databases | PostgreSQL, MySQL, Oracle, MS SQL Server, SQLite | MongoDB, Redis, Cassandra, Neo4j, DynamoDB |

---

# 2. SQL Sub-Languages & Core Commands

### Q3: What are the sub-languages of SQL?
**Answer:**
SQL is broadly divided into 5 specialized sub-languages:

```
                  ┌───────────────────────────────┐
                  │      SQL Sub-Languages        │
                  └───────────────┬───────────────┘
         ┌────────────┬───────────┼───────────┬────────────┐
         ▼            ▼           ▼           ▼            ▼
     ┌───────┐    ┌───────┐   ┌───────┐   ┌───────┐    ┌───────┐
     │  DDL  │    │  DML  │   │  DQL  │   │  DCL  │    │  TCL  │
     └───────┘    └───────┘   └───────┘   └───────┘    └───────┘
```

1. **DDL (Data Definition Language):** Modifies the database schema and object definitions. Auto-commits in most engines.
   - `CREATE`: Creates tables, views, indexes, databases (`CREATE TABLE ...`).
   - `ALTER`: Modifies an existing schema (`ALTER TABLE ... ADD COLUMN ...`).
   - `DROP`: Deletes an entire database object including its structure and data (`DROP TABLE ...`).
   - `TRUNCATE`: Removes all data rows from a table while preserving structure (`TRUNCATE TABLE ...`).
   - `RENAME`: Renames database objects.
2. **DML (Data Manipulation Language):** Modifies table records. Operates within transactions.
   - `INSERT`: Inserts new rows (`INSERT INTO ... VALUES ...`).
   - `UPDATE`: Modifies existing rows (`UPDATE ... SET ... WHERE ...`).
   - `DELETE`: Deletes specific rows (`DELETE FROM ... WHERE ...`).
   - `MERGE` / `UPSERT`: Inserts or updates depending on row existence.
3. **DQL (Data Query Language):** Fetches and retrieves data.
   - `SELECT`: Retrieves data matching criteria (`SELECT ... FROM ... WHERE ...`).
4. **DCL (Data Control Language):** Manages user privileges and permissions.
   - `GRANT`: Gives permissions to users/roles (`GRANT SELECT ON ... TO user1`).
   - `REVOKE`: Withdraws previously granted permissions (`REVOKE ALL ON ... FROM user1`).
5. **TCL (Transaction Control Language):** Controls transactional boundaries.
   - `COMMIT`: Persists all modifications permanently.
   - `ROLLBACK`: Reverts changes back to the start of transaction or to a savepoint.
   - `SAVEPOINT`: Marks an intermediate point inside a transaction.

---

# 3. Keys & Constraints Architecture

### Q4: Database Keys Overview

| Key Name | Definition & Purpose | Allows NULL? | Multiple per Table? |
| :--- | :--- | :---: | :---: |
| **Super Key** | Any set of attributes that uniquely identifies a record. | Depends | ✅ Yes |
| **Candidate Key** | A minimal Super Key (no redundant attributes). | ❌ No (for PK candidates) | ✅ Yes |
| **Primary Key (PK)** | The selected Candidate Key used as the primary row identifier. | ❌ Strictly No | ❌ Only 1 |
| **Alternate Key** | Candidate keys that were not chosen as the Primary Key. | ✅ Yes (unless `NOT NULL`) | ✅ Yes |
| **Unique Key (UK)** | Ensures all column values are distinct. | ✅ Yes (engine-dependent) | ✅ Yes |
| **Foreign Key (FK)** | A column referencing the PK or UK of another table to guarantee referential integrity. | ✅ Yes | ✅ Yes |
| **Composite Key** | A Primary or Unique Key composed of **2 or more columns**. | ❌ No | ❌ Only 1 PK |

#### Code Example:
```sql
CREATE TABLE Departments (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE Employees (
    emp_id INT,
    company_code VARCHAR(10),
    email VARCHAR(255) UNIQUE NOT NULL,      -- Unique Key
    dept_id INT,
    salary DECIMAL(12, 2) CHECK (salary > 0), -- Check Constraint
    status VARCHAR(20) DEFAULT 'ACTIVE',     -- Default Constraint
    
    PRIMARY KEY (emp_id, company_code),      -- Composite Primary Key
    CONSTRAINT fk_dept FOREIGN KEY (dept_id) 
        REFERENCES Departments(dept_id)
        ON DELETE SET NULL                   -- Foreign Key Action
        ON UPDATE CASCADE
);
```

---

### Q5: Foreign Key Referential Actions (`ON DELETE` / `ON UPDATE`)
- **`RESTRICT` / `NO ACTION` (Default):** Rejects the deletion/update of the parent row if child records exist.
- **`CASCADE`:** Automatically deletes/updates matching child records when the parent row is deleted/updated.
- **`SET NULL`:** Sets child foreign key columns to `NULL` when the parent row is deleted/updated.
- **`SET DEFAULT`:** Sets child foreign key columns to their default value when parent row changes.

---

### Q6: Core Constraints Cheat Sheet

| Constraint | Syntax Example | Rule Enforced |
| :--- | :--- | :--- |
| `NOT NULL` | `age INT NOT NULL` | Value cannot be omitted or set to `NULL`. |
| `UNIQUE` | `phone VARCHAR(15) UNIQUE` | Every row must have a distinct value (allows `NULL`). |
| `CHECK` | `age INT CHECK (age >= 18)` | Evaluates a boolean expression on every insert/update. |
| `DEFAULT` | `created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP` | Sets fallback value when omitted. |
| `PRIMARY KEY`| `id INT PRIMARY KEY` | Enforces `UNIQUE` + `NOT NULL` + creates Clustered Index (by default). |
| `FOREIGN KEY`| `FOREIGN KEY (dept_id) REFERENCES Dept(id)` | Restricts values to those present in parent table. |

---

# 4. DML, DDL & DQL Essentials

### Q7: `DELETE` vs `TRUNCATE` vs `DROP` (Deep Dive)

| Criteria | `DELETE` | `TRUNCATE` | `DROP` |
| :--- | :--- | :--- | :--- |
| **Language Category** | **DML** (Data Manipulation) | **DDL** (Data Definition) | **DDL** (Data Definition) |
| **Operation Type** | Row-by-row deletion | Bulk data page deallocation | Complete object destruction |
| **`WHERE` Filtering** | ✅ Supported (`DELETE FROM t WHERE id = 5`) | ❌ Not supported (all rows wiped) | ❌ Not supported |
| **Auto-Increment Reset**| ❌ Counter is preserved | ✅ Counter resets to initial seed | ❌ Entire table deleted |
| **Transaction Logging**| High (logs each deleted row in undo/redo logs) | Low (only deallocated pages are logged) | Minimal |
| **Performance** | Slow on large tables ($O(N)$) | Ultra fast ($O(1)$ page unlinking) | Ultra fast |
| **Rollback Support** | ✅ Fully supported inside transactions | ✅ Supported in Postgres/SQL Server; Engine-dependent in MySQL | ✅ Supported in transactional DDL systems |
| **Triggers** | Fires `ON DELETE` row-level triggers | ❌ Does NOT fire delete triggers | ❌ Does NOT fire triggers |
| **Schema & Indexes** | Preserved | Preserved | Completely removed from data dictionary |

---

### Q8: `WHERE` vs `HAVING`

| Feature | `WHERE` Clause | `HAVING` Clause |
| :--- | :--- | :--- |
| **Filtering Target** | Individual rows **before** aggregation | Aggregated groups **after** `GROUP BY` |
| **Aggregate Functions** | ❌ Cannot use (`WHERE AVG(salary) > 50k` $\rightarrow$ ERROR) | ✅ Designed for aggregates (`HAVING AVG(salary) > 50k`) |
| **Execution Timing** | Stage 2 (before grouping) | Stage 4 (after grouping) |
| **Index Utilization** | Can leverage B-Tree indexes directly | Cannot leverage indexes directly on aggregated values |

```sql
SELECT 
    dept_id, 
    AVG(salary) AS avg_salary,
    COUNT(*) AS total_employees
FROM Employees
WHERE status = 'ACTIVE'               -- 1. Filters individual raw rows
GROUP BY dept_id                      -- 2. Groups remaining rows
HAVING COUNT(*) >= 3 AND AVG(salary) > 60000; -- 3. Filters grouped aggregates
```

---

# 5. SQL Logical Execution Order

One of the most critical interview questions is: **In what order does the database engine actually execute an SQL query?**

```
┌────────────────────────────────────────────────────────┐
│               Logical Query Processing                 │
│                                                        │
│   1. FROM & JOIN     (Identify tables & build cross-product/joins)
│   2. WHERE           (Filter individual base rows)     │
│   3. GROUP BY        (Aggregate rows into groups)      │
│   4. HAVING          (Filter aggregated groups)        │
│   5. SELECT          (Evaluate expressions & projections)
│   6. DISTINCT        (Deduplicate result set)          │
│   7. ORDER BY        (Sort output rows)                │
│   8. LIMIT / OFFSET  (Paginate and slice output)       │
└────────────────────────────────────────────────────────┘
```

> **Interview Trap:** Why can't you use a column alias defined in `SELECT` inside the `WHERE` clause?
> **Answer:** Because `WHERE` (Step 2) is evaluated **before** `SELECT` (Step 5). The alias does not exist yet when `WHERE` is executed! (However, `ORDER BY` in Step 7 *can* use aliases because it runs after `SELECT`).

---

# 6. Joins Deep Dive & Visual Venn Mechanics

### Schema Setup for Join Examples
```
Employees Table (Left):                  Departments Table (Right):
+--------+----------+---------+--------+  +---------+-------------+
| emp_id | emp_name | dept_id | salary |  | dept_id | dept_name   |
+--------+----------+---------+--------+  +---------+-------------+
| 1      | Alice    | 10      | 90000  |  | 10      | Engineering |
| 2      | Bob      | 20      | 75000  |  | 20      | Marketing   |
| 3      | Charlie  | NULL    | 60000  |  | 30      | Human Res.  |
| 4      | David    | 10      | 85000  |  +---------+-------------+
+--------+----------+---------+--------+
```

---

### 1. `INNER JOIN`
Returns only rows with a matching key in **both** tables.
```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
INNER JOIN Departments d ON e.dept_id = d.dept_id;
```
**Output:**
- Alice (Engineering)
- Bob (Marketing)
- David (Engineering)

---

### 2. `LEFT (OUTER) JOIN`
Returns **all** rows from the left table, plus matched rows from the right table. Non-matching right columns are populated with `NULL`.
```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
LEFT JOIN Departments d ON e.dept_id = d.dept_id;
```
**Output:**
- Alice (Engineering)
- Bob (Marketing)
- Charlie (`NULL`)
- David (Engineering)

---

### 3. `RIGHT (OUTER) JOIN`
Returns **all** rows from the right table, plus matched rows from the left table.
```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
RIGHT JOIN Departments d ON e.dept_id = d.dept_id;
```
**Output:**
- Alice (Engineering)
- David (Engineering)
- Bob (Marketing)
- `NULL` (Human Res.)

---

### 4. `FULL (OUTER) JOIN`
Returns all rows when there is a match in **either** left or right table. Unmatched columns contain `NULL`.
```sql
-- Standard ANSI SQL (PostgreSQL, Oracle, SQL Server):
SELECT e.emp_name, d.dept_name
FROM Employees e
FULL OUTER JOIN Departments d ON e.dept_id = d.dept_id;

-- MySQL Emulation (using UNION):
SELECT e.emp_name, d.dept_name FROM Employees e LEFT JOIN Departments d ON e.dept_id = d.dept_id
UNION
SELECT e.emp_name, d.dept_name FROM Employees e RIGHT JOIN Departments d ON e.dept_id = d.dept_id;
```

---

### 5. `CROSS JOIN`
Returns the Cartesian product ($M \times N$ combinations).
```sql
SELECT e.emp_name, d.dept_name
FROM Employees e
CROSS JOIN Departments d;
```

---

### 6. `SELF JOIN`
A table is joined with itself. Frequently used for **hierarchical data** (Manager $\leftrightarrow$ Employee, Category trees).
```sql
SELECT 
    e.emp_name AS employee,
    m.emp_name AS manager
FROM Employees e
LEFT JOIN Employees m ON e.manager_id = m.emp_id;
```

---

### 7. Anti-Joins: Finding Missing Records

#### A. Find Employees Without a Department:
```sql
SELECT e.emp_id, e.emp_name
FROM Employees e
LEFT JOIN Departments d ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;
```

#### B. Find Departments Without Any Employees:
```sql
SELECT d.dept_id, d.dept_name
FROM Departments d
LEFT JOIN Employees e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
```

---

# 7. Aggregations & Grouping Masterclass

### Core Aggregate Functions
- `COUNT(*)`: Counts **all rows**, including those with `NULL`s.
- `COUNT(column)`: Counts rows where `column` is **not `NULL`**.
- `COUNT(DISTINCT column)`: Counts unique non-`NULL` values.
- `SUM(column)`, `AVG(column)`, `MIN(column)`, `MAX(column)`: Evaluate mathematical aggregations, ignoring `NULL` values.

---

### High-Frequency Aggregation Questions:

#### 1. Count Employees by Department
```sql
SELECT 
    d.dept_name,
    COUNT(e.emp_id) AS total_employees
FROM Departments d
LEFT JOIN Employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name;
```

#### 2. Highest Salary by Department
```sql
SELECT 
    d.dept_name,
    MAX(e.salary) AS max_salary
FROM Departments d
INNER JOIN Employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name;
```

#### 3. Average Salary by Department with Filter
```sql
SELECT 
    dept_id,
    AVG(salary) AS avg_salary
FROM Employees
GROUP BY dept_id
HAVING AVG(salary) > 70000;
```

---

# 8. 🔥 Top Must-Code SQL Interview Queries

*Reference Schemas:*
- `Employees(emp_id, emp_name, dept_id, salary, manager_id, joining_date, email)`
- `Departments(dept_id, dept_name)`
- `Customers(customer_id, customer_name, email)`
- `Orders(order_id, customer_id, order_date, total_amount)`

---

### Problem 1: Find Second-Highest Salary

#### Approach 1: Window Function (`DENSE_RANK()`) — ⭐ Recommended (Handles ties)
```sql
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employees
)
SELECT DISTINCT salary AS second_highest_salary
FROM RankedSalaries
WHERE rnk = 2;
```

#### Approach 2: Correlated / Subquery with `MAX()`
```sql
SELECT MAX(salary) AS second_highest_salary
FROM Employees
WHERE salary < (SELECT MAX(salary) FROM Employees);
```

#### Approach 3: `LIMIT` & `OFFSET` (MySQL/Postgres)
```sql
SELECT DISTINCT salary
FROM Employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

---

### Problem 2: Find Third-Highest Salary
```sql
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employees
)
SELECT DISTINCT salary AS third_highest_salary
FROM RankedSalaries
WHERE rnk = 3;
```

---

### Problem 3: Find Nth-Highest Salary (Universal Formulation)

#### Method 1: CTE with `DENSE_RANK()` (Best Modern SQL)
```sql
WITH RankedSalaries AS (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employees
)
SELECT DISTINCT salary
FROM RankedSalaries
WHERE rnk = :N; -- replace :N with desired rank number
```

#### Method 2: Correlated Subquery (Universal ANSI SQL)
```sql
SELECT DISTINCT e1.salary
FROM Employees e1
WHERE :N - 1 = (
    SELECT COUNT(DISTINCT e2.salary)
    FROM Employees e2
    WHERE e2.salary > e1.salary
);
```

---

### Problem 4: Find Top 3 Salaries Company-Wide
```sql
SELECT DISTINCT salary
FROM Employees
ORDER BY salary DESC
LIMIT 3;
```

---

### Problem 5: Find Top 3 Employees in Each Department
```sql
WITH DeptRankedEmployees AS (
    SELECT 
        emp_id,
        emp_name,
        dept_id,
        salary,
        DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
    FROM Employees
)
SELECT emp_id, emp_name, dept_id, salary, rnk
FROM DeptRankedEmployees
WHERE rnk <= 3;
```

---

### Problem 6: Find Duplicate Records
```sql
SELECT email, COUNT(*) AS duplicate_count
FROM Employees
GROUP BY email
HAVING COUNT(*) > 1;
```

---

### Problem 7: Delete Duplicate Records (Keep only 1 instance)

#### Method 1: Using CTE & `ROW_NUMBER()` (PostgreSQL, SQL Server, Oracle)
```sql
WITH DuplicateCTE AS (
    SELECT 
        emp_id,
        ROW_NUMBER() OVER (PARTITION BY email ORDER BY emp_id ASC) AS row_num
    FROM Employees
)
DELETE FROM Employees
WHERE emp_id IN (
    SELECT emp_id FROM DuplicateCTE WHERE row_num > 1
);
```

#### Method 2: Self-Join `DELETE` (MySQL)
```sql
DELETE e1
FROM Employees e1
INNER JOIN Employees e2 
ON e1.email = e2.email AND e1.emp_id > e2.emp_id;
```

---

### Problem 8: Find Employees Earning More Than the Average Salary
```sql
SELECT emp_id, emp_name, salary
FROM Employees
WHERE salary > (SELECT AVG(salary) FROM Employees);
```

---

### Problem 9: Find Employees Earning More Than Their Manager
```sql
SELECT 
    e.emp_id,
    e.emp_name AS employee_name,
    e.salary   AS emp_salary,
    m.emp_name AS manager_name,
    m.salary   AS manager_salary
FROM Employees e
INNER JOIN Employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;
```

---

### Problem 10: Find Departments Having More Than 5 Employees
```sql
SELECT 
    d.dept_id,
    d.dept_name,
    COUNT(e.emp_id) AS total_employees
FROM Departments d
INNER JOIN Employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name
HAVING COUNT(e.emp_id) > 5;
```

---

### Problem 11: Find Highest-Paid Employee in Each Department
```sql
WITH RankedByDept AS (
    SELECT 
        emp_id,
        emp_name,
        dept_id,
        salary,
        DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
    FROM Employees
)
SELECT emp_id, emp_name, dept_id, salary
FROM RankedByDept
WHERE rnk = 1;
```

---

### Problem 12: Find Second-Highest Salary per Department
```sql
WITH RankedByDept AS (
    SELECT 
        dept_id,
        salary,
        DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
    FROM Employees
)
SELECT dept_id, salary AS second_highest_salary
FROM RankedByDept
WHERE rnk = 2;
```

---

### Problem 13: Find Employees Who Joined in a Particular Year (e.g. 2024)

```sql
-- SARGable / Index-friendly query (Best Practice):
SELECT emp_id, emp_name, joining_date
FROM Employees
WHERE joining_date >= '2024-01-01' AND joining_date < '2025-01-01';

-- Using Functions (Caution: Prevents regular B-tree index scan):
SELECT emp_id, emp_name, joining_date
FROM Employees
WHERE EXTRACT(YEAR FROM joining_date) = 2024; -- PostgreSQL/Oracle
-- OR WHERE YEAR(joining_date) = 2024;        -- MySQL/SQL Server
```

---

### Problem 14: Pattern Matching & Wildcards (`LIKE`)
```sql
-- Starts with 'A':
SELECT * FROM Employees WHERE emp_name LIKE 'A%';

-- Ends with 's':
SELECT * FROM Employees WHERE emp_name LIKE '%s';

-- Contains 'an':
SELECT * FROM Employees WHERE emp_name LIKE '%an%';

-- Exactly 5 characters long:
SELECT * FROM Employees WHERE emp_name LIKE '_____';

-- Starts with 'J' and is at least 3 characters long:
SELECT * FROM Employees WHERE emp_name LIKE 'J__%';
```

---

### Problem 15: Find Employees Whose Salary Is Between X and Y
```sql
SELECT emp_id, emp_name, salary
FROM Employees
WHERE salary BETWEEN 50000 AND 90000; -- BETWEEN is inclusive: [50000, 90000]
```

---

### Problem 16: Find Employees With No Manager (Hierarchy Root)
```sql
SELECT emp_id, emp_name
FROM Employees
WHERE manager_id IS NULL;
```

---

### Problem 17: Find Duplicate Email Addresses
```sql
SELECT email, COUNT(*) AS count
FROM Employees
WHERE email IS NOT NULL
GROUP BY email
HAVING COUNT(*) > 1;
```

---

### Problem 18: Find Customers Who Never Placed an Order

#### Method 1: `LEFT JOIN` (High Performance with Index)
```sql
SELECT c.customer_id, c.customer_name
FROM Customers c
LEFT JOIN Orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

#### Method 2: `NOT EXISTS` (Clean short-circuit logic)
```sql
SELECT c.customer_id, c.customer_name
FROM Customers c
WHERE NOT EXISTS (
    SELECT 1 FROM Orders o WHERE o.customer_id = c.customer_id
);
```

#### Method 3: `NOT IN` (⚠️ WARNING: NULL Trap)
```sql
SELECT customer_id, customer_name
FROM Customers
WHERE customer_id NOT IN (
    SELECT customer_id FROM Orders WHERE customer_id IS NOT NULL -- NULL check is MANDATORY!
);
```

---

### Problem 19: Find Customers Who Placed More Than 3 Orders
```sql
SELECT 
    c.customer_id,
    c.customer_name,
    COUNT(o.order_id) AS order_count
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING COUNT(o.order_id) > 3;
```

---

### Problem 20: Find the Latest (Most Recent) Order for Every Customer
```sql
WITH RankedOrders AS (
    SELECT 
        order_id,
        customer_id,
        order_date,
        total_amount,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id 
            ORDER BY order_date DESC, order_id DESC
        ) AS rn
    FROM Orders
)
SELECT order_id, customer_id, order_date, total_amount
FROM RankedOrders
WHERE rn = 1;
```

---

# 9. Advanced SQL

### Q9: Window Functions (`ROW_NUMBER()` vs `RANK()` vs `DENSE_RANK()`)

| Salary | `ROW_NUMBER()` | `RANK()` | `DENSE_RANK()` | Explanation |
| :---: | :---: | :---: | :---: | :--- |
| **10,000** | 1 | 1 | 1 | Highest salary (tie 1) |
| **10,000** | 2 | 1 | 1 | Highest salary (tie 2) |
| **8,500** | 3 | **3** | **2** | `RANK()` skips rank 2; `DENSE_RANK()` has **no gaps** |
| **7,000** | 4 | 4 | 3 | Next distinct rank |

```sql
SELECT 
    emp_name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,
    RANK()       OVER (ORDER BY salary DESC) AS rnk,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rnk
FROM Employees;
```

#### Other Essential Window Functions:
- `LEAD(col, offset)`: Fetches value from the next row.
- `LAG(col, offset)`: Fetches value from the previous row (great for Year-over-Year growth calculations).
- `FIRST_VALUE(col)` / `LAST_VALUE(col)`: Returns the first/last value in the window frame.
- `NTILE(n)`: Divides ordered partition into $n$ equal buckets (e.g. quartiles, percentiles).

---

### Q10: Common Table Expressions (CTEs) & Recursive CTEs

#### Standard CTE:
```sql
WITH HighEarners AS (
    SELECT emp_id, emp_name, salary, dept_id
    FROM Employees
    WHERE salary > 80000
)
SELECT dept_id, COUNT(*) AS count
FROM HighEarners
GROUP BY dept_id;
```

#### Recursive CTE (Organizational Chart Traversal):
```sql
WITH RECURSIVE OrgChart AS (
    -- 1. Anchor Member: CEO / Top-level Manager (manager_id IS NULL)
    SELECT emp_id, emp_name, manager_id, 1 AS level
    FROM Employees
    WHERE manager_id IS NULL

    UNION ALL

    -- 2. Recursive Member: Direct reports of the previous level
    SELECT e.emp_id, e.emp_name, e.manager_id, o.level + 1
    FROM Employees e
    INNER JOIN OrgChart o ON e.manager_id = o.emp_id
)
SELECT * FROM OrgChart ORDER BY level, emp_id;
```

---

### Q11: Subquery vs Correlated Subquery

| Feature | Regular (Non-Correlated) Subquery | Correlated Subquery |
| :--- | :--- | :--- |
| **Independence** | Can run independently of the outer query | Cannot run independently; references outer query columns |
| **Evaluation Frequency** | Evaluates **once** for the entire statement | Evaluates **once per row** of the outer query |
| **Performance** | Typically fast ($O(1)$ subquery execution) | Can be slow on large tables unless optimized to a join by the query planner |

```sql
-- Non-correlated Subquery:
SELECT emp_name, salary 
FROM Employees 
WHERE salary > (SELECT AVG(salary) FROM Employees);

-- Correlated Subquery (Find employees earning more than their department's average):
SELECT e1.emp_name, e1.salary, e1.dept_id
FROM Employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM Employees e2
    WHERE e2.dept_id = e1.dept_id
);
```

---

### Q12: `EXISTS` vs `IN`

| Dimension | `EXISTS` | `IN` |
| :--- | :--- | :--- |
| **Operation** | Checks for row existence; short-circuits on first match (`TRUE`/`FALSE`) | Pulls all matching values into an in-memory set and checks membership |
| **NULL Behavior** | Safe with `NULL`s | `NOT IN (..., NULL)` always evaluates to `UNKNOWN`/Empty! |
| **Optimization** | Best for large datasets with correlated conditions | Best for small, static value lists (`status IN ('A', 'B')`) |

---

# 10. Set Operations, Conditional Logic & NULL Mechanics

### Q13: `UNION` vs `UNION ALL`

| Feature | `UNION` | `UNION ALL` |
| :--- | :--- | :--- |
| **Duplicate Rows** | Deduplicates rows (distinct set) | Preserves all rows including duplicates |
| **Performance** | Slower (requires an internal sort / hash pass) | Fast (simple append of result buffers) |
| **Memory** | Higher | Minimal |

---

### Q14: `CASE` & `COALESCE`

#### `CASE` Expression:
```sql
SELECT 
    emp_name, 
    salary,
    CASE 
        WHEN salary >= 100000 THEN 'Executive Tier'
        WHEN salary >= 60000  THEN 'Mid Tier'
        ELSE 'Junior Tier'
    END AS compensation_tier
FROM Employees;
```

#### `COALESCE` (First Non-NULL Value):
```sql
SELECT emp_name, COALESCE(personal_email, work_email, 'no-email@company.com') AS primary_contact
FROM Employees;
```

#### `NULLIF(a, b)`:
Returns `NULL` if $a = b$, otherwise returns $a$. (Commonly used to prevent **Divide by Zero** errors):
```sql
SELECT total_sales / NULLIF(total_orders, 0) AS avg_order_value FROM Sales;
```

---

### Q15: Three-Valued Logic in SQL
SQL conditions evaluate to `TRUE`, `FALSE`, or **`UNKNOWN`** (due to `NULL`).
- `NULL = NULL` $\rightarrow$ **`UNKNOWN`** (Never use `= NULL`, always use `IS NULL`).
- `NULL != 5` $\rightarrow$ **`UNKNOWN`**.
- `NOT (UNKNOWN)` $\rightarrow$ **`UNKNOWN`**.

---

# 11. Indexing, Storage Internals & Performance Tuning

### Q16: Clustered vs Non-Clustered Index

| Feature | Clustered Index | Non-Clustered Index (Secondary Index) |
| :--- | :--- | :--- |
| **Physical Storage** | Dictates the physical sort order of data pages on disk | Separate B-Tree structure storing indexed keys + row pointers |
| **Count Per Table** | **Exactly 1** per table | **Multiple** (e.g. 5–20+) per table |
| **Leaf Nodes** | Contain the actual **data rows** | Contain the indexed keys + row locator (heap RID or clustered key) |
| **Default** | Primary Key | Created explicitly or via `UNIQUE` constraints |

```
Clustered Index (Table IS the Index):
Root Node ──> Intermediate Nodes ──> [Leaf: Full Row Data Pages (1, 2, 3...)]

Non-Clustered Index:
Root Node ──> Intermediate Nodes ──> [Leaf: Key + Clustered Key Pointer] ──> Fetch Row
```

---

### Q17: Composite Index & The Leftmost Prefix Rule
If you create an index on `(dept_id, salary, joining_date)`:

| Filter in WHERE Clause | Can Index Be Used? |
| :--- | :---: |
| `WHERE dept_id = 10` | ✅ Yes |
| `WHERE dept_id = 10 AND salary > 50000` | ✅ Yes |
| `WHERE dept_id = 10 AND salary = 50000 AND joining_date > '2024-01-01'` | ✅ Yes |
| `WHERE salary = 50000` (Skipped leftmost column `dept_id`) | ❌ No (Full table / index scan) |
| `WHERE joining_date > '2024-01-01'` | ❌ No |

---

### Q18: Covering Index
A **Covering Index** contains all columns requested by the `SELECT`, `WHERE`, and `JOIN` clauses.
- **Benefit:** The engine satisfies the entire query directly from the index B-tree without performing a secondary disk lookup ("Bookmark / Key Lookup").

---

### Q19: When Should You NOT Use an Index?
1. On **small tables** (a full table scan in RAM is faster than multi-level B-tree traversal).
2. On columns with **frequent write/update operations** (every `INSERT`/`UPDATE`/`DELETE` must rebalance the index tree).
3. On columns with **low cardinality** (e.g. `gender` or `is_active` with only 2 distinct values).
4. On columns that are rarely or never queried in `WHERE`, `JOIN`, or `ORDER BY`.

---

# 12. Database Normalization & Schema Design

### Normalization Pipeline

```
Unnormalized Data (Repeating groups, multi-valued cells)
       │
       ▼ [1NF] Atomic values + Primary Key identified
First Normal Form
       │
       ▼ [2NF] 1NF + No Partial Dependencies (Non-keys depend on ENTIRE composite PK)
Second Normal Form
       │
       ▼ [3NF] 2NF + No Transitive Dependencies (Non-keys depend ONLY on PK)
Third Normal Form
       │
       ▼ [BCNF] Stricter 3NF: For every X -> Y, X must be a Super Key
Boyce-Codd Normal Form
```

---

### Normal Forms Detailed:

#### 1. First Normal Form (1NF):
- Each column value must be **atomic** (no comma-separated lists like `skills = "Java, Python, SQL"`).
- No repeating column groups (`skill1`, `skill2`, `skill3`).
- Each row is uniquely identifiable via a Primary Key.

#### 2. Second Normal Form (2NF):
- Table must be in **1NF**.
- **No Partial Dependency**: Applies when the PK is composite. Every non-key column must depend on the **entire composite key**, not just a part of it.

#### 3. Third Normal Form (3NF):
- Table must be in **2NF**.
- **No Transitive Dependency**: Non-key columns must not depend on other non-key columns ($X \rightarrow Y \rightarrow Z$).
- *Example:* In `Employees(emp_id, dept_id, dept_name)`, `dept_name` depends on `dept_id`, which depends on `emp_id`. Split `Departments(dept_id, dept_name)` into its own table.

#### 4. Boyce-Codd Normal Form (BCNF):
- Stricter variant of 3NF (also called 3.5NF).
- For every functional dependency $X \rightarrow Y$, $X$ **must be a Super Key**.

#### 5. Denormalization:
- **Definition:** The deliberate introduction of redundancy into a normalized schema.
- **Why?** To avoid expensive multi-table joins in high-traffic read-heavy systems (e.g., OLAP, Data Warehousing, Reporting dashboards).

---

# 13. Transactions, ACID & Concurrency Control

### Q20: ACID Properties Explained

| Property | Core Concept | Implementation Mechanism |
| :--- | :--- | :--- |
| **Atomicity** | "All or Nothing" — If any step fails, all preceding steps are rolled back. | Undo Logs, Write-Ahead Logging (WAL) |
| **Consistency** | Database transitions from one valid state to another, respecting all constraints & foreign keys. | Schema rules, cascading constraints, validations |
| **Isolation** | Intermediate states of concurrent transactions are invisible to each other. | Locks (Shared/Exclusive), MVCC (Multi-Version Concurrency Control) |
| **Durability** | Once committed, changes are permanent and survive crashes or power failures. | Redo Logs, WAL flushed to non-volatile disk |

---

### Q21: Concurrency Read Phenomena (Anomalies)

1. **Dirty Read:** Transaction A modifies a row without committing. Transaction B reads that uncommitted value. Transaction A rolls back $\rightarrow$ Transaction B read invalid phantom data.
2. **Non-Repeatable Read (Fuzzy Read):** Transaction A reads a row. Transaction B updates/deletes that row and commits. Transaction A re-reads the same row $\rightarrow$ gets different data.
3. **Phantom Read:** Transaction A queries a range of rows (`WHERE salary > 50000`). Transaction B inserts new rows matching the range and commits. Transaction A re-runs the range query $\rightarrow$ sees new "phantom" rows.

---

### Q22: ANSI SQL Isolation Levels vs Anomalies

| Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read | Typical Implementation |
| :--- | :---: | :---: | :---: | :--- |
| **Read Uncommitted** | ❌ Allowed | ❌ Allowed | ❌ Allowed | Read with no locks |
| **Read Committed** *(Default: Postgres, Oracle, SQL Server)* | 🛡️ Prevented | ❌ Allowed | ❌ Allowed | Short-lived read locks / MVCC snapshots per statement |
| **Repeatable Read** *(Default: MySQL InnoDB)* | 🛡️ Prevented | 🛡️ Prevented | ❌ Allowed (Prevented via Next-Key locks in InnoDB) | Read locks held until commit / MVCC snapshot per transaction |
| **Serializable** | 🛡️ Prevented | 🛡️ Prevented | 🛡️ Prevented | Two-Phase Locking (2PL), Range locks, or SSI |

---

### Q23: Deadlocks & Resolution

#### What is a Deadlock?
A deadlock occurs when two or more transactions hold exclusive locks on separate resources and each waits for the other to release its lock, forming a circular dependency:

```
Transaction 1: Holds Lock on Table A ──── Requests Lock on Table B ───┐
      ▲                                                              │
      │                                                              ▼
      └──── Requests Lock on Table A ──── Holds Lock on Table B : Transaction 2
```

#### How Relational Engines Handle Deadlocks:
1. **Deadlock Detection:** The database runs a background engine thread that maintains a **Wait-For Graph (WFG)**. If a cycle is detected, the database engine picks a **victim transaction** (usually the one with lowest rollback cost) and aborts/rolls it back with a deadlock error.
2. **Deadlock Prevention Best Practices:**
   - Always access tables and update rows in the **exact same consistent order** across all transactions.
   - Keep transactions as short as possible.
   - Avoid long interactive user inputs inside open transactions.
   - Use row-level locking instead of table locks.
