**PostgreSQL Database Project – Cheatsheet**  
A practical reference for the PostgreSQL IPL Data Project and the database concepts required for the technical paper.  
  
---  
**1. PostgreSQL Database and User Management**  
**Create a user**  
```  
CREATE USER ipl_user WITH PASSWORD 'your_password';  
   
```  
`CREATE USER` creates a PostgreSQL role that has the `LOGIN` privilege.  
**Create a database owned by that user**  
```  
CREATE DATABASE ipl_database  
 OWNER ipl_user;  
   
```  
The `OWNER` clause makes `ipl_user` the owner of the database.  
**Grant permissions**  
Database-level connection permission:  
```  
GRANT CONNECT ON DATABASE ipl_database TO ipl_user;  
   
```  
After connecting to `ipl_database`, grant schema permissions:  
```  
GRANT USAGE, CREATE ON SCHEMA public TO ipl_user;  
   
```  
For existing tables:  
```  
GRANT SELECT, INSERT, UPDATE, DELETE  
 ON ALL TABLES IN SCHEMA public  
 TO ipl_user;  
   
```  
For existing sequences:  
```  
GRANT USAGE, SELECT, UPDATE  
 ON ALL SEQUENCES IN SCHEMA public  
 TO ipl_user;  
   
```  
To apply table and sequence permissions automatically to objects created in the future by the current role:  
```  
ALTER DEFAULT PRIVILEGES IN SCHEMA public  
 GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO ipl_user;  
   
 ALTER DEFAULT PRIVILEGES IN SCHEMA public  
 GRANT USAGE, SELECT, UPDATE ON SEQUENCES TO ipl_user;  
   
```  
**Remember:** If the user owns the database and creates its objects, extra grants may not be necessary for those objects. Default privileges apply to objects created by the role that runs `ALTER DEFAULT PRIVILEGES`.  
**Connect to a database using psql**  
From the terminal:  
```  
psql -U ipl_user -d ipl_database -h localhost  
   
```  
Useful psql commands:  
  
|  |  |  
| - | - |  
| **Command** |  **Purpose** |  
| `\l` |  List databases |  
| `\du` |  List roles/users |  
| `\dt` |  List tables |  
| `\d table_name` |  Describe a table |  
| `\dn` |  List schemas |  
| `\conninfo` |  Show current connection |  
| `\q` |  Exit psql |  
  
   
**Cleanup script**  
Run database and role removal as a sufficiently privileged user, while connected to a different database such as `postgres`.  
```  
DROP DATABASE IF EXISTS ipl_database;  
 DROP ROLE IF EXISTS ipl_user;  
   
```  
**Important:** PostgreSQL does not allow dropping a database while connected to that database. Make sure other sessions are disconnected. Do not use `CASCADE` casually; understand which dependent objects it will remove.  
  
---  
**2. Loading CSV Data**  
**Create a table**  
The column names and data types must match the CSV structure.  
```  
CREATE TABLE matches (  
     id INTEGER PRIMARY KEY,  
     season INTEGER,  
     city VARCHAR(100),  
     date DATE,  
     team1 VARCHAR(100),  
     team2 VARCHAR(100),  
     winner VARCHAR(100)  
 );  
   
```  
Example for deliveries:  
```  
CREATE TABLE deliveries (  
     match_id INTEGER,  
     inning INTEGER,  
     batting_team VARCHAR(100),  
     bowling_team VARCHAR(100),  
     batsman VARCHAR(100),  
     bowler VARCHAR(100),  
     total_runs INTEGER,  
     batsman_runs INTEGER,  
     extra_runs INTEGER  
 );  
   
```  
These are example schemas. Check the actual CSV headers and values before creating your final tables.  
**Method 1: COPY**  
`COPY` reads a file from the PostgreSQL server's file system.  
```  
COPY matches (  
     id, season, city, date, team1, team2, winner  
 )  
 FROM '/absolute/path/matches.csv'  
 WITH (FORMAT CSV, HEADER TRUE, DELIMITER ',');  
   
```  
**Method 2: psql** `\copy`  
`\copy` reads a file from the computer running the psql client. This is often easier for a local project.  
```  
\copy matches FROM '/absolute/path/matches.csv' WITH (FORMAT CSV, HEADER TRUE, DELIMITER ',')  
   
```  
**Difference:**  
  
|  |  |  
| - | - |  
| **`COPY`** |  **`\copy`** |  
| PostgreSQL server reads/writes the file |  psql client reads/writes the file |  
| Server needs file access and suitable privileges |  Client needs file access |  
| SQL command |  psql meta-command |  
  
   
**Verify imported data**  
```  
SELECT COUNT(*) FROM matches;  
   
 SELECT *  
 FROM matches  
 LIMIT 5;  
   
```  
Check missing values:  
```  
SELECT COUNT(*)  
 FROM matches  
 WHERE winner IS NULL;  
   
```  
  
---  
**3. IPL Data Analysis – SQL Patterns**  
Use the actual table and column names from your imported datasets. The following queries are common IPL analysis patterns.  
**Count matches played in each season**  
```  
SELECT season, COUNT(*) AS total_matches  
 FROM matches  
 GROUP BY season  
 ORDER BY season;  
   
```  
**Count matches won by each team**  
```  
SELECT winner, COUNT(*) AS total_wins  
 FROM matches  
 WHERE winner IS NOT NULL  
 GROUP BY winner  
 ORDER BY total_wins DESC;  
   
```  
**Find the most successful team**  
```  
SELECT winner, COUNT(*) AS total_wins  
 FROM matches  
 WHERE winner IS NOT NULL  
 GROUP BY winner  
 ORDER BY total_wins DESC  
 LIMIT 1;  
   
```  
**Find the number of matches played in each city**  
```  
SELECT city, COUNT(*) AS total_matches  
 FROM matches  
 WHERE city IS NOT NULL  
 GROUP BY city  
 ORDER BY total_matches DESC;  
   
```  
**Find toss decisions**  
```  
SELECT toss_decision, COUNT(*) AS total  
 FROM matches  
 GROUP BY toss_decision;  
   
```  
**Find extra runs conceded by each bowling team in a season**  
```  
SELECT m.season,  
        d.bowling_team,  
        SUM(d.extra_runs) AS extra_runs  
 FROM deliveries AS d  
 JOIN matches AS m  
     ON m.id = d.match_id  
 WHERE m.season = 2016  
 GROUP BY m.season, d.bowling_team  
 ORDER BY extra_runs DESC;  
   
```  
**Find the top run scorers**  
```  
SELECT batsman, SUM(batsman_runs) AS total_runs  
 FROM deliveries  
 GROUP BY batsman  
 ORDER BY total_runs DESC  
 LIMIT 10;  
   
```  
**Find the most economical bowlers**  
Economy rate is runs conceded divided by overs bowled. In this simplified example, legal balls are identified using `wide_runs` and `noball_runs`; use the exact columns in your dataset.  
```  
SELECT bowler,  
        SUM(total_runs - COALESCE(bye_runs, 0)  
                        - COALESCE(legbye_runs, 0)) AS runs_conceded,  
        COUNT(*) FILTER (  
            WHERE COALESCE(wide_runs, 0) = 0  
              AND COALESCE(noball_runs, 0) = 0  
        ) AS legal_balls,  
        ROUND(  
            SUM(total_runs - COALESCE(bye_runs, 0)  
                            - COALESCE(legbye_runs, 0))::numeric  
            /  
            NULLIF(  
                COUNT(*) FILTER (  
                    WHERE COALESCE(wide_runs, 0) = 0  
                      AND COALESCE(noball_runs, 0) = 0  
                ), 0  
            ) * 6,  
            2  
        ) AS economy_rate  
 FROM deliveries  
 GROUP BY bowler  
 HAVING COUNT(*) FILTER (  
     WHERE COALESCE(wide_runs, 0) = 0  
       AND COALESCE(noball_runs, 0) = 0  
 ) > 0  
 ORDER BY economy_rate;  
   
```  
This is an illustrative calculation. Confirm the assignment's definition of runs conceded and overs, and apply any minimum-overs condition it requires.  
**Useful PostgreSQL query features**  
  
|  |  |  |  
| - | - | - |  
| **Feature** |  **Purpose** |  **Example** |  
| `WHERE` |  Filter rows before grouping |  `WHERE season = 2016` |  
| `GROUP BY` |  Group rows for aggregation |  `GROUP BY season` |  
| `HAVING` |  Filter groups after aggregation |  `HAVING COUNT(*) > 10` |  
| `ORDER BY` |  Sort the result |  `ORDER BY total_runs DESC` |  
| `LIMIT` |  Restrict returned rows |  `LIMIT 5` |  
| `DISTINCT` |  Return unique values |  `SELECT DISTINCT city` |  
| `COALESCE` |  Replace NULL with a value |  `COALESCE(winner, 'Unknown')` |  
| `NULLIF` |  Return NULL when values match |  `NULLIF(total, 0)` |  
| `FILTER` |  Filter rows for one aggregate |  `COUNT(*) FILTER (WHERE ...)` |  
| `CASE` |  Conditional result |  `CASE WHEN ... THEN ... END` |  
  
   
  
---  
**4. Joins**  
A join combines rows from two or more tables using a related column.  
Assume these tables:  
**employees**  
  
|  |  |  |  
| - | - | - |  
| **employee_id** |  **name** |  **department_id** |  
| 1 |  Arun |  10 |  
| 2 |  Bala |  20 |  
| 3 |  Chitra |  30 |  
  
   
**departments**  
  
|  |  |  
| - | - |  
| **department_id** |  **department_name** |  
| 10 |  Sales |  
| 20 |  IT |  
| 40 |  HR |  
  
   
**INNER JOIN**  
Returns only rows with matching values in both tables.  
```  
SELECT e.name, d.department_name  
 FROM employees AS e  
 INNER JOIN departments AS d  
     ON e.department_id = d.department_id;  
   
```  
Result: Arun–Sales and Bala–IT.  
**LEFT JOIN**  
Returns every row from the left table and matching rows from the right table. Unmatched right-side columns become `NULL`.  
```  
SELECT e.name, d.department_name  
 FROM employees AS e  
 LEFT JOIN departments AS d  
     ON e.department_id = d.department_id;  
   
```  
Result includes Chitra with a `NULL` department name.  
**RIGHT JOIN**  
Returns every row from the right table and matching rows from the left table.  
```  
SELECT e.name, d.department_name  
 FROM employees AS e  
 RIGHT JOIN departments AS d  
     ON e.department_id = d.department_id;  
   
```  
**FULL OUTER JOIN**  
Returns matched rows and unmatched rows from both tables.  
```  
SELECT e.name, d.department_name  
 FROM employees AS e  
 FULL OUTER JOIN departments AS d  
     ON e.department_id = d.department_id;  
   
```  
**CROSS JOIN**  
Returns every possible combination of rows.  
```  
SELECT e.name, d.department_name  
 FROM employees AS e  
 CROSS JOIN departments AS d;  
   
```  
**SELF JOIN**  
Joins a table with itself.  
```  
SELECT employee.name AS employee,  
        manager.name AS manager  
 FROM employees AS employee  
 LEFT JOIN employees AS manager  
     ON employee.manager_id = manager.employee_id;  
   
```  
  
---  
**5. Aggregations and Filtering**  
Aggregate functions calculate a result from multiple rows.  
  
|  |  |  
| - | - |  
| **Function** |  **Purpose** |  
| `COUNT()` |  Count rows or non-NULL values |  
| `SUM()` |  Add values |  
| `AVG()` |  Calculate average |  
| `MIN()` |  Find minimum |  
| `MAX()` |  Find maximum |  
  
   
Example:  
```  
SELECT department_id,  
        COUNT(*) AS employee_count,  
        AVG(salary) AS average_salary,  
        MAX(salary) AS highest_salary  
 FROM employees  
 GROUP BY department_id;  
   
```  
**WHERE vs HAVING**  
- `WHERE` filters individual rows before grouping.  
- `HAVING` filters groups after aggregation.  
```  
SELECT department_id, COUNT(*) AS employee_count  
 FROM employees  
 WHERE salary > 20000  
 GROUP BY department_id  
 HAVING COUNT(*) >= 3;  
   
```  
**SQL logical processing order**  
A useful simplified order is:  
1. `FROM` and `JOIN`  
2. `WHERE`  
3. `GROUP BY`  
4. `HAVING`  
5. `SELECT`  
6. `DISTINCT`  
7. `ORDER BY`  
8. `LIMIT` / `OFFSET`  
  
---  
**6. ACID Properties**  
ACID describes four properties that help database transactions remain reliable.  
  
|  |  |  |  
| - | - | - |  
| **Property** |  **Meaning** |  **Example** |  
| Atomicity |  All operations succeed or none are applied |  A bank transfer must debit and credit together |  
| Consistency |  A transaction preserves database constraints and rules |  An account balance cannot violate an enforced constraint |  
| Isolation |  Concurrent transactions are controlled so they do not interfere incorrectly |  Two users update accounts at the same time |  
| Durability |  Committed changes survive a crash |  A completed transfer remains saved after restart |  
  
   
**Memory aid:** Atomicity, Consistency, Isolation, Durability.  
  
---  
**7. CAP Theorem**  
CAP theorem describes a trade-off in distributed data systems when a network partition occurs.  
- **Consistency (C):** Every read receives the latest successful write, or an error.  
- **Availability (A):** Every request to a non-failing node receives a response, even if it may not contain the latest data.  
- **Partition Tolerance (P):** The system continues operating despite communication failures between parts of the system.  
**Main idea**  
When a network partition occurs, a distributed system must choose how to balance consistency and availability. It cannot guarantee both full consistency and full availability during every partition.  
  
|  |  |  
| - | - |  
| **Choice during partition** |  **Behaviour** |  
| CP |  Prioritizes consistency; some requests may be rejected or delayed |  
| AP |  Prioritizes availability; some responses may contain stale data |  
  
   
**Important:** CAP is about distributed systems during a network partition. It is not a rule that every database must permanently choose only two properties.  
  
---  
**8. Normalization**  
Normalization organizes relational tables to reduce duplicate data and prevent data anomalies.  
**Why normalize?**  
- Avoid unnecessary data duplication.  
- Prevent update anomalies.  
- Prevent insertion anomalies.  
- Prevent deletion anomalies.  
- Improve data consistency.  
**Unnormalized example**  
  
|  |  |  |  
| - | - | - |  
| **order_id** |  **customer_name** |  **products** |  
| 1 |  Arun |  Mouse, Keyboard |  
| 2 |  Bala |  Monitor |  
  
   
The `products` column contains multiple values in one cell.  
**First Normal Form (1NF)**  
Each column should contain atomic values, with no repeating groups.  
  
|  |  |  |  
| - | - | - |  
| **order_id** |  **customer_name** |  **product** |  
| 1 |  Arun |  Mouse |  
| 1 |  Arun |  Keyboard |  
| 2 |  Bala |  Monitor |  
  
   
**Second Normal Form (2NF)**  
- Must satisfy 1NF.  
- Every non-key attribute must depend on the whole candidate key, not just part of a composite key.  
**Third Normal Form (3NF)**  
- Must satisfy 2NF.  
- Non-key attributes should not depend transitively on a candidate key.  
**Simple design**  
Instead of repeatedly storing customer details in every order:  
**customers**  
  
|  |  |  
| - | - |  
| **customer_id** |  **customer_name** |  
| 1 |  Arun |  
| 2 |  Bala |  
  
   
**orders**  
  
|  |  |  
| - | - |  
| **order_id** |  **customer_id** |  
| 101 |  1 |  
| 102 |  2 |  
  
   
**order_items**  
  
|  |  |  
| - | - |  
| **order_id** |  **product** |  
| 101 |  Mouse |  
| 101 |  Keyboard |  
| 102 |  Monitor |  
  
   
  
---  
**9. Indexes**  
An index is a database object that helps PostgreSQL find rows without scanning the entire table.  
**Example**  
```  
CREATE INDEX idx_employees_name  
 ON employees(name);  
   
```  
Query:  
```  
SELECT *  
 FROM employees  
 WHERE name = 'Arun';  
   
```  
Inspect the execution plan:  
```  
EXPLAIN ANALYZE  
 SELECT *  
 FROM employees  
 WHERE name = 'Arun';  
   
```  
**Common PostgreSQL index types**  
  
|  |  |  
| - | - |  
| **Index type** |  **Typical use** |  
| B-tree |  Equality and range comparisons; default index type |  
| Hash |  Equality comparisons |  
| GIN |  Composite values such as arrays, JSONB and full-text search |  
| GiST |  Geometric data and other specialized search strategies |  
| BRIN |  Very large tables where values correlate with physical row order |  
  
   
**Create a composite index**  
```  
CREATE INDEX idx_employee_department_salary  
 ON employees(department_id, salary);  
   
```  
Column order matters. This index is generally most useful for queries filtering by `department_id`, or by both `department_id` and `salary`.  
**Unique index**  
```  
CREATE UNIQUE INDEX idx_employees_email  
 ON employees(email);  
   
```  
Prevents duplicate non-NULL email values. For a `NOT NULL` email column, this enforces unique email addresses.  
**Remove an index**  
```  
DROP INDEX idx_employees_name;  
   
```  
**Advantages**  
- Can improve read/query performance.  
- Can help with filtering, joins and sorting.  
- A unique index can enforce uniqueness.  
**Disadvantages**  
- Uses additional storage.  
- Adds work to `INSERT`, `UPDATE` and `DELETE`.  
- Too many indexes can reduce write performance.  
**Remember:** PostgreSQL's query planner decides whether an index is useful. An index does not guarantee that every query will use it.  
  
---  
**10. Transactions**  
A transaction is a group of database operations treated as one logical unit.  
**Example: Bank transfer**  
```  
BEGIN;  
   
 UPDATE accounts  
 SET balance = balance - 1000  
 WHERE account_id = 1;  
   
 UPDATE accounts  
 SET balance = balance + 1000  
 WHERE account_id = 2;  
   
 COMMIT;  
   
```  
**Main transaction commands**  
  
|  |  |  
| - | - |  
| **Command** |  **Purpose** |  
| `BEGIN` |  Start a transaction |  
| `COMMIT` |  Save transaction changes |  
| `ROLLBACK` |  Undo uncommitted changes |  
| `SAVEPOINT` |  Create a point to roll back to |  
| `ROLLBACK TO SAVEPOINT` |  Undo changes after a savepoint |  
| `RELEASE SAVEPOINT` |  Remove a savepoint |  
  
   
**Savepoint example**  
```  
BEGIN;  
   
 UPDATE accounts  
 SET balance = balance - 500  
 WHERE account_id = 1;  
   
 SAVEPOINT after_debit;  
   
 UPDATE accounts  
 SET balance = balance + 500  
 WHERE account_id = 2;  
   
 ROLLBACK TO SAVEPOINT after_debit;  
   
 COMMIT;  
   
```  
The second update is undone, while the first update remains part of the transaction and is committed. This is only a demonstration; a real transfer should not commit an incomplete debit/credit pair.  
  
---  
**11. Locking Mechanism**  
Locks control concurrent access to database objects and rows.  
**Why are locks needed?**  
Imagine two users trying to update the same bank account at the same time. Without concurrency control, one update could overwrite another or read inconsistent data.  
**Common PostgreSQL lock concepts**  
  
|  |  |  
| - | - |  
| **Lock** |  **Purpose** |  
| `ACCESS SHARE` |  Used by ordinary `SELECT` queries |  
| `ROW SHARE` |  Used by locking reads such as `SELECT ... FOR UPDATE` |  
| `ROW EXCLUSIVE` |  Used by `INSERT`, `UPDATE` and `DELETE` |  
| `SHARE` |  Restricts certain concurrent table modifications |  
| `ACCESS EXCLUSIVE` |  Strong table lock; blocks ordinary reads and writes |  
  
   
PostgreSQL also uses row-level locks for operations that modify or explicitly lock rows.  
**Row-level locking example**  
```  
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
`FOR UPDATE` locks the selected rows against conflicting updates and locking reads until the transaction ends.  
**Deadlock**  
A deadlock happens when transactions wait for each other's locks in a cycle.  
Example:  
- Transaction A locks Row 1 and waits for Row 2.  
- Transaction B locks Row 2 and waits for Row 1.  
PostgreSQL detects deadlocks and aborts one transaction to break the cycle.  
  
---  
**12. Database Isolation Levels**  
Isolation levels control how much one transaction can observe the changes made by other concurrent transactions.  
**Common concurrency problems**  
  
|  |  |  
| - | - |  
| **Problem** |  **Meaning** |  
| Dirty read |  Reading another transaction's uncommitted changes |  
| Non-repeatable read |  Reading the same row twice and getting different committed values |  
| Phantom read |  Repeating a query and finding additional or missing rows due to another committed transaction |  
  
   
**PostgreSQL isolation levels**  
  
|  |  |  
| - | - |  
| **Isolation level** |  **Behaviour** |  
| Read Uncommitted |  In PostgreSQL, behaves like Read Committed |  
| Read Committed |  Each statement sees data committed before that statement began |  
| Repeatable Read |  A transaction uses a stable snapshot; PostgreSQL also prevents phantom reads under this level |  
| Serializable |  Provides the strongest isolation by making successful transactions behave as if executed serially; transactions may need to be retried |  
  
   
**Set isolation level**  
```  
BEGIN TRANSACTION  
 ISOLATION LEVEL REPEATABLE READ;  
   
 SELECT *  
 FROM accounts;  
   
 COMMIT;  
   
```  
**Important PostgreSQL detail**  
PostgreSQL's default isolation level is **Read Committed**.  
Under Serializable isolation, PostgreSQL can raise a serialization failure. Applications should be prepared to retry the entire transaction.  
  
---  
**13. Triggers**  
A trigger is a database action that runs automatically when a specified event occurs on a table or view.  
Common trigger events:  
- `INSERT`  
- `UPDATE`  
- `DELETE`  
- `TRUNCATE` (statement-level triggers)  
**Example: Create an audit table**  
```  
CREATE TABLE employee_audit (  
     audit_id SERIAL PRIMARY KEY,  
     employee_id INTEGER,  
     action VARCHAR(20),  
     changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP  
 );  
   
```  
**Create a trigger function**  
```  
CREATE OR REPLACE FUNCTION log_employee_insert()  
 RETURNS TRIGGER AS $$  
 BEGIN  
     INSERT INTO employee_audit (employee_id, action)  
     VALUES (NEW.employee_id, 'INSERT');  
   
     RETURN NEW;  
 END;  
 $$ LANGUAGE plpgsql;  
   
```  
**Create the trigger**  
```  
CREATE TRIGGER employee_insert_trigger  
 AFTER INSERT ON employees  
 FOR EACH ROW  
 EXECUTE FUNCTION log_employee_insert();  
   
```  
**Test the trigger**  
```  
INSERT INTO employees (employee_id, name, department_id)  
 VALUES (10, 'Kumar', 20);  
   
 SELECT *  
 FROM employee_audit;  
   
```  
**Important trigger terms**  
  
|  |  |  
| - | - |  
| **Term** |  **Meaning** |  
| `BEFORE` |  Runs before the triggering operation |  
| `AFTER` |  Runs after the triggering operation |  
| `FOR EACH ROW` |  Runs once for every affected row |  
| `FOR EACH STATEMENT` |  Runs once for the SQL statement |  
| `NEW` |  New row value in INSERT/UPDATE triggers |  
| `OLD` |  Previous row value in UPDATE/DELETE triggers |  
| `TG_OP` |  Identifies the operation, such as INSERT, UPDATE or DELETE |  
  
   
**Remove a trigger**  
```  
DROP TRIGGER employee_insert_trigger ON employees;  
   
```  
Remove its function:  
```  
DROP FUNCTION log_employee_insert();  
   
```  
  
---  
**14. Quick Revision**  
  
|  |  |  
| - | - |  
| **Topic** |  **Remember** |  
| Database creation |  `CREATE DATABASE` |  
| User creation |  `CREATE USER` |  
| Permissions |  `GRANT`, `REVOKE` |  
| Database cleanup |  `DROP DATABASE`, `DROP ROLE` |  
| CSV import |  `COPY`, `\copy` |  
| Joins |  Combine related tables |  
| Aggregation |  `COUNT`, `SUM`, `AVG`, `MIN`, `MAX` |  
| Filtering |  `WHERE`, `HAVING` |  
| ACID |  Atomicity, Consistency, Isolation, Durability |  
| CAP |  Consistency, Availability, Partition Tolerance |  
| Normalization |  Reduce duplication and anomalies |  
| Indexing |  Improve data lookup in suitable queries |  
| Transactions |  `BEGIN`, `COMMIT`, `ROLLBACK` |  
| Locking |  Control concurrent access |  
| Isolation |  Control visibility between transactions |  
| Triggers |  Automatically execute database logic on events |  
  
   
