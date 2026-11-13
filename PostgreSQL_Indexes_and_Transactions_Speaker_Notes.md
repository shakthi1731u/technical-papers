**PostgreSQL: Indexes and Transactions**  
  
---  
**Slide 1 — PostgreSQL: Indexes and Transactions**  
Today, we will discuss two important PostgreSQL topics: indexes and transactions.  
Indexes help the database retrieve data efficiently. Transactions help the database perform related changes safely.  
These topics solve different problems:  
- Indexes focus on **query performance**.  
- Transactions focus on **data correctness and reliability**.  
**Slide 2 — Session outline**  
We will first understand what an index is and how PostgreSQL uses it. Then we will look at B-tree indexes, index creation, and query plans.  
After that, we will move to transactions. We will use a bank transfer example to understand `BEGIN`, `COMMIT`, and `ROLLBACK`, and finish with the ACID properties.  
**Slide 3 — What is an index?**  
An index is an additional data structure that helps PostgreSQL find rows without checking every row in a table.  
Think of a book. If we want to find a topic, we can use the index at the back of the book instead of reading every page.  
The database works in a similar way:  
- The **table** contains the actual records.  
- The **index** contains indexed key values and references to the corresponding table rows.  
- PostgreSQL uses the index to identify matching rows and then retrieves the required table data.  
Without a useful index, PostgreSQL may perform a **Sequential Scan**, checking rows one by one. With a suitable index, it may use an **Index Scan** or another index-based plan.  
An index is not guaranteed to be faster for every query. If a query returns a large percentage of the table, a sequential scan may be cheaper.  
**Slide 4 — B-tree index**  
B-tree is the default index method in PostgreSQL and is suitable for many common operations.  
A B-tree keeps keys in sorted order and maintains a balanced tree structure. PostgreSQL can navigate from the root through intermediate pages to the relevant leaf entries.  
For a balanced tree, the number of levels grows slowly as the number of indexed entries grows. This is why searching through a B-tree is generally more efficient than checking every row.  
B-tree indexes commonly support:  
- Equality comparisons, such as `department = 'IT'`.  
- Range comparisons, such as `salary > 50000`.  
- Ordered retrieval, such as `ORDER BY employee_id`.  
- Prefix conditions in some cases, depending on the operator and collation.  
The diagram is conceptual. PostgreSQL stores index data in database pages, and its real internal layout is more detailed than this simplified drawing.  
**Slide 5 — Creating and checking an index**  
We can create an index with `CREATE INDEX`.  
```  
CREATE INDEX idx_employees_department  
 ON employees (department);  
   
```  
Here:  
- `idx_employees_department` is the index name.  
- `employees` is the table.  
- `department` is the indexed column.  
Now inspect a query plan:  
```  
EXPLAIN  
 SELECT *  
 FROM employees  
 WHERE department = 'IT';  
   
```  
`EXPLAIN` displays the execution plan selected by PostgreSQL. It does not execute the query.  
Possible plan nodes include:  
- `Seq Scan` — scan table rows.  
- `Index Scan` — use an index to locate rows and fetch table data.  
- `Bitmap Index Scan` and `Bitmap Heap Scan` — use an index to identify matching rows, then visit table pages efficiently.  
The PostgreSQL query planner chooses the plan based on estimated costs and available statistics. Creating an index does not force PostgreSQL to use it.  
To execute the query and inspect actual timing and row counts, use:  
```  
EXPLAIN ANALYZE  
 SELECT *  
 FROM employees  
 WHERE department = 'IT';  
   
```  
`EXPLAIN ANALYZE` executes the query, so use it carefully with statements that modify data.  
**Slide 6 — Index design and trade-offs**  
A **single-column index** indexes one column:  
```  
CREATE INDEX idx_employees_department  
 ON employees (department);  
   
```  
A **composite index** indexes multiple columns:  
```  
CREATE INDEX idx_orders_customer_date  
 ON orders (customer_id, order_date);  
   
```  
Column order matters. For a B-tree index on `(customer_id, order_date)`, queries filtering by `customer_id` can generally use the leading part of the index. A query filtering only by `order_date` may not be able to use that index as effectively.  
A **unique index** prevents duplicate values for its indexed key. PostgreSQL automatically creates a unique index to enforce a `PRIMARY KEY` or `UNIQUE` constraint.  
Indexes also have costs:  
- They use additional disk space.  
- INSERT operations must maintain the index.  
- UPDATE operations may need to update index entries.  
- DELETE operations must maintain index consistency.  
Therefore, create indexes based on actual query patterns rather than indexing every column.  
**Slide 7 — What is a transaction?**  
A transaction is a group of database operations treated as one logical unit.  
For example, transferring money between two accounts requires two changes:  
1. Deduct money from the sender.  
2. Add the same amount to the receiver.  
If only the first operation succeeds, the data becomes incorrect. A transaction allows both changes to be committed together or rolled back together.  
The main commands are:  
- `BEGIN` — start an explicit transaction.  
- `COMMIT` — make the transaction's changes permanent.  
- `ROLLBACK` — cancel changes made by the current uncommitted transaction.  
PostgreSQL uses a transaction for each standalone statement when the client is in its usual autocommit mode. Explicit `BEGIN` is useful when multiple statements must succeed or fail together.  
**Slide 8 — Bank transfer example**  
Assume Arun has ₹5,000 and Ravi has ₹2,000. We want to transfer ₹1,000 from Arun to Ravi.  
After a successful transfer:  
- Arun has ₹4,000.  
- Ravi has ₹3,000.  
Both updates must be part of the same transaction. If the second update fails before the transaction is committed, we can roll back the first update as well.  
This prevents a partial transfer from being saved.  
**Slide 9 — Transaction commands**  
Example SQL:  
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
Explanation:  
- `BEGIN` starts the transaction.  
- The first `UPDATE` deducts ₹1,000 from account 1.  
- The second `UPDATE` adds ₹1,000 to account 2.  
- `COMMIT` saves both changes.  
If an error occurs before `COMMIT`, we can issue:  
```  
ROLLBACK;  
   
```  
That cancels the transaction's uncommitted changes.  
In real applications, also validate that the sender has sufficient funds and that both account IDs exist. The example focuses on transaction control rather than complete banking logic.  
**Slide 10 — ACID properties**  
ACID describes four important properties of reliable database transactions.  
**Atomicity**  
All operations in the transaction succeed as a unit, or none of their changes are committed. In the bank transfer example, we should not save the debit without the credit.  
**Consistency**  
A transaction must preserve database rules and constraints. For example, a constraint can prevent an account balance from becoming negative.  
Consistency depends on the rules defined by the application and database, and on the transaction respecting those rules.  
**Isolation**  
Concurrent transactions should not produce incorrect interference. PostgreSQL provides transaction isolation levels that control what one transaction can observe from other concurrent transactions.  
The default isolation level in PostgreSQL is `READ COMMITTED`.  
**Durability**  
After a transaction is committed, its changes are intended to survive a database crash or restart. Actual durability guarantees depend on PostgreSQL configuration and the storage system behaving correctly.  
**Slide 11 — Key takeaways**  
Remember these points:  
1. Indexes help PostgreSQL find rows efficiently, but they require storage and add write overhead.  
2. B-tree is the default index method for many common equality and range queries.  
3. `EXPLAIN` helps us understand the query plan chosen by PostgreSQL.  
4. Transactions group related changes into one logical unit.  
5. `COMMIT` saves changes; `ROLLBACK` cancels uncommitted changes.  
6. ACID explains the reliability properties of transactions.  
  
---  
**Quick SQL reference**  
**Indexes**  
```  
-- Create a single-column index  
 CREATE INDEX idx_employees_department  
 ON employees (department);  
   
 -- Create a composite index  
 CREATE INDEX idx_orders_customer_date  
 ON orders (customer_id, order_date);  
   
 -- Create a unique index  
 CREATE UNIQUE INDEX idx_users_email  
 ON users (email);  
   
 -- Inspect the query plan  
 EXPLAIN  
 SELECT *  
 FROM employees  
 WHERE department = 'IT';  
   
 -- Inspect actual execution  
 EXPLAIN ANALYZE  
 SELECT *  
 FROM employees  
 WHERE department = 'IT';  
   
```  
**Transactions**  
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
If the transaction should be cancelled before commit:  
```  
ROLLBACK;  
   
```  
**Important distinction**  
- `EXPLAIN` displays the estimated plan without running the query.  
- `EXPLAIN ANALYZE` runs the query and reports actual execution details.  
- `ROLLBACK` only reverses changes that are still part of the uncommitted transaction. It cannot undo a transaction after it has been committed.  
