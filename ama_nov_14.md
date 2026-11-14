# AMA Questions – Cheatsheet

Simple explanations and examples for all 16 questions from today's AMA session.

---

## 1. What is RANK() in SQL?

`RANK()` is a window function that assigns a ranking number to rows based on an order.

```sql
SELECT employee_name, salary,
       RANK() OVER (ORDER BY salary DESC) AS salary_rank
FROM employees;
```

If two employees have the same salary, they receive the same rank and the next rank is skipped. For example: 1, 2, 2, 4.

Related functions:
- `RANK()` – leaves gaps after ties.
- `DENSE_RANK()` – does not leave gaps.
- `ROW_NUMBER()` – gives every row a unique sequential number.

## 2. What is a schema in SQL?

A schema is a logical container inside a database. It groups objects such as tables, views and functions.

```sql
CREATE SCHEMA company;

CREATE TABLE company.employees (
    employee_id INTEGER,
    employee_name VARCHAR(100)
);
```

Here, `company` is the schema and `employees` is a table inside it. In psql, use `\\dn` to list schemas.

## 3. How can we check whether an object belongs to a class in Python?

Use `isinstance()`.

```python
class Animal:
    pass

class Dog(Animal):
    pass

dog = Dog()

print(isinstance(dog, Dog))     # True
print(isinstance(dog, Animal))  # True
print(isinstance(dog, str))     # False
```

`isinstance()` also considers inheritance. `type(dog) is Dog` checks only the exact class.

## 4. Difference between static method and class method

A static method does not receive an automatic first argument. A class method receives the class as `cls`.

```python
class Calculator:
    @staticmethod
    def add(a, b):
        return a + b

class Employee:
    company = "ABC"

    @classmethod
    def change_company(cls, new_name):
        cls.company = new_name

print(Calculator.add(10, 20))
Employee.change_company("XYZ")
```

| Static method | Class method |
|---|---|
| `@staticmethod` | `@classmethod` |
| No automatic `self` or `cls` | Receives `cls` |
| Utility related to the class | Works with class-level state |

## 5. What is a recursive function, and how does it work?

A recursive function calls itself to solve a problem in smaller steps. It needs a base case to stop.

```python
def factorial(number):
    if number == 1:
        return 1
    return number * factorial(number - 1)

print(factorial(5))  # 120
```

Flow: `5 * 4 * 3 * 2 * 1 = 120`.

Common uses include tree traversal, directory traversal and divide-and-conquer algorithms. Without a valid base case, recursion may cause a `RecursionError`.

## 6. Difference between DBMS and RDBMS

DBMS means Database Management System. RDBMS means Relational Database Management System.

| DBMS | RDBMS |
|---|---|
| General system for managing data | A type of DBMS based on the relational model |
| Data organization depends on the system | Data is organized into related tables |
| Relational keys are not a defining requirement | Uses keys and relational constraints |

Examples of RDBMSs include PostgreSQL, MySQL, Oracle Database and SQL Server.

## 7. How can we remove an element from a set in Python?

```python
numbers = {10, 20, 30}

numbers.remove(20)   # Raises KeyError if 20 is absent
numbers.discard(40)  # Does nothing if 40 is absent
removed = numbers.pop()  # Removes and returns an arbitrary element
```

| Method | Behaviour when element is absent |
|---|---|
| `remove()` | Raises `KeyError` |
| `discard()` | Does nothing |
| `pop()` | Removes an arbitrary element; raises `KeyError` if set is empty |

## 8. How can we update a specific row and column in SQL?

Use `UPDATE`, `SET` and `WHERE`.

```sql
UPDATE employees
SET salary = 50000
WHERE employee_id = 101;
```

Update multiple columns:

```sql
UPDATE employees
SET salary = 55000,
    department = 'IT'
WHERE employee_id = 101;
```

**Important:** Without `WHERE`, every row in the table will be updated.

## 9. Difference between LIKE and ILIKE

In PostgreSQL:
- `LIKE` performs case-sensitive pattern matching.
- `ILIKE` performs case-insensitive pattern matching.

```sql
SELECT * FROM employees
WHERE employee_name LIKE 'a%';

SELECT * FROM employees
WHERE employee_name ILIKE 'a%';
```

Wildcards:
- `%` matches zero or more characters.
- `_` matches exactly one character.

Examples:

```sql
WHERE employee_name ILIKE 'A%'    -- Starts with A or a
WHERE employee_name ILIKE '%n'    -- Ends with n or N
WHERE employee_name ILIKE '%ar%'  -- Contains ar, ignoring case
WHERE employee_name LIKE '____'   -- Exactly four characters
```

`ILIKE` is PostgreSQL-specific; other databases may use different methods.

## 10. What are the types of locks in SQL?

Locks control concurrent access to database objects and rows.

Important PostgreSQL table-level lock modes:

| Lock mode | General purpose |
|---|---|
| `ACCESS SHARE` | Ordinary `SELECT` |
| `ROW SHARE` | Locking reads such as `SELECT ... FOR UPDATE` |
| `ROW EXCLUSIVE` | `INSERT`, `UPDATE`, `DELETE` |
| `SHARE` | Allows reads but conflicts with certain write-related locks |
| `ACCESS EXCLUSIVE` | Strongest table lock; conflicts with all other table lock modes |

Example of a row-level lock:

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

`FOR UPDATE` locks selected rows against conflicting updates and locking reads until the transaction ends. PostgreSQL also supports advisory locks.

## 11. What are naming conventions in SQL?

Naming conventions are consistent rules for naming tables, columns, indexes and constraints.

Common practices:
- Use meaningful names.
- Use `snake_case`.
- Avoid spaces and reserved keywords.
- Be consistent with singular or plural table names.
- Use descriptive names.

Examples: `employee_details`, `department_id`, `idx_employee_name`.

PostgreSQL folds unquoted identifiers to lowercase. Quoted identifiers preserve case and must be quoted consistently.

## 12. What does L mean in SOLID principles?

`L` stands for **Liskov Substitution Principle (LSP)**.

Objects of a child class should be usable wherever objects of the parent class are expected, without breaking the program's correctness.

```python
class Bird:
    def move(self):
        print("Bird is moving")

class Sparrow(Bird):
    def move(self):
        print("Sparrow is flying")

def make_bird_move(bird):
    bird.move()

make_bird_move(Sparrow())
```

The child class should preserve the expected behaviour of its parent class.

## 13. Difference between EXISTS and NOT EXISTS

Both check whether a subquery returns rows.

`EXISTS` is true if at least one row is returned:

```sql
SELECT department_name
FROM departments AS d
WHERE EXISTS (
    SELECT 1
    FROM employees AS e
    WHERE e.department_id = d.department_id
);
```

This finds departments with employees.

`NOT EXISTS` is true if no rows are returned:

```sql
SELECT department_name
FROM departments AS d
WHERE NOT EXISTS (
    SELECT 1
    FROM employees AS e
    WHERE e.department_id = d.department_id
);
```

This finds departments without employees.

`SELECT 1` is commonly used because the query checks whether a row exists, not the selected value.

## 14. How do we use DELETE in SQL?

`DELETE` removes rows from a table.

```sql
-- Delete one matching row
DELETE FROM employees
WHERE employee_id = 101;

-- Delete all rows
DELETE FROM employees;
```

The table structure remains. Without `WHERE`, all rows are deleted. A `DELETE` can be rolled back if it is inside a transaction that is rolled back.

## 15. Can we write comments in SQL?

Yes. SQL supports single-line and block comments.

```sql
-- Single-line comment

/*
Multi-line comment.
This query retrieves IT employees.
*/
SELECT *
FROM employees
WHERE department = 'IT';
```

Comments are ignored by the SQL parser and help explain queries.

## 16. Difference between TRUNCATE, DELETE and DROP

### DELETE

Removes selected rows. Supports `WHERE`; table structure remains.

```sql
DELETE FROM employees
WHERE employee_id = 101;
```

### TRUNCATE

Removes all rows efficiently. Does not support `WHERE`; table structure remains.

```sql
TRUNCATE TABLE employees;
```

PostgreSQL supports rolling back `TRUNCATE` inside a transaction. It does not fire `ON DELETE` triggers, but can fire `ON TRUNCATE` triggers.

To reset identity sequences:

```sql
TRUNCATE TABLE employees RESTART IDENTITY;
```

### DROP

Removes the table object and its data.

```sql
DROP TABLE employees;
```

### Comparison

| Feature | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| Removes rows | Yes | All rows | Removes table itself |
| Supports `WHERE` | Yes | No | No |
| Keeps table structure | Yes | Yes | No |
| PostgreSQL rollback in a transaction | Yes | Yes | Yes |

**Remember:**
- `DELETE` – remove selected rows.
- `TRUNCATE` – empty the table.
- `DROP` – remove the table itself.

---

## Quick Revision

| No. | Question | Main answer |
|---:|---|---|
| 1 | RANK in SQL | Window function that assigns ranks |
| 2 | Schema | Logical container for database objects |
| 3 | Check object class | `isinstance()` |
| 4 | Static vs class method | No automatic argument vs `cls` |
| 5 | Recursive function | Function calls itself and needs a base case |
| 6 | DBMS vs RDBMS | General database system vs relational database system |
| 7 | Remove set element | `remove()`, `discard()`, `pop()` |
| 8 | Update specific row | `UPDATE ... SET ... WHERE ...` |
| 9 | LIKE vs ILIKE | Case-sensitive vs case-insensitive in PostgreSQL |
| 10 | Locks | Control concurrent access |
| 11 | Naming conventions | Consistent object naming rules |
| 12 | L in SOLID | Liskov Substitution Principle |
| 13 | EXISTS vs NOT EXISTS | Matching row exists vs no matching row exists |
| 14 | DELETE | Remove rows |
| 15 | SQL comments | `--` and `/* ... */` |
| 16 | TRUNCATE vs DELETE vs DROP | Delete rows vs empty table vs remove table |

## Practice Tip

Try explaining each answer aloud without reading the notes. Run the SQL examples in PostgreSQL and the Python examples in a Python interpreter.
