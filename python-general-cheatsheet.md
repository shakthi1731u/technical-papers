# Python General Cheat Sheet

## 1. Python Basics
- Python is a high-level, interpreted programming language.
- Python uses indentation to define code blocks.
- Statements usually end with a new line.
- Comments begin with `#`.
- Python is case-sensitive.

## 2. Variables
- Variables store values.
- A variable is created when a value is assigned.
- Variable names can contain letters, numbers, and underscores.
- Variable names cannot begin with a number.
- Use descriptive variable names.

## 3. Data Types

| Type | Description |
|---|---|
| `int` | Whole numbers |
| `float` | Decimal numbers |
| `complex` | Complex numbers |
| `str` | Text |
| `bool` | `True` or `False` |
| `list` | Ordered, mutable collection |
| `tuple` | Ordered, immutable collection |
| `set` | Unordered collection of unique values |
| `dict` | Key-value collection |
| `NoneType` | Represents no value |

- Check a value's type with `type()`.
- Convert between compatible types using `int()`, `float()`, `str()`, and `bool()`.

## 4. Operators

### Arithmetic
| Operator | Purpose |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `//` | Floor division |
| `%` | Modulus |
| `**` | Exponentiation |

### Comparison
- `==` equal to
- `!=` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to

### Logical
- `and`
- `or`
- `not`

### Assignment
- `=`, `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=`

### Membership and Identity
- `in`, `not in`
- `is`, `is not`

## 5. Conditional Statements
- `if` checks a condition.
- `elif` checks another condition when earlier conditions are false.
- `else` handles the remaining case.
- Conditions can use comparison and logical operators.

## 6. Loops
- `for` iterates over an iterable.
- `while` repeats while a condition remains true.
- `range()` generates a sequence of numbers.
- `break` exits a loop.
- `continue` skips to the next iteration.
- `pass` acts as an empty placeholder.
- Nested loops contain one loop inside another.

## 7. Strings
- Strings are ordered and immutable sequences of characters.
- Access characters using indexing.
- Extract parts using slicing.
- Common methods:

| Method | Purpose |
|---|---|
| `lower()` | Convert to lowercase |
| `upper()` | Convert to uppercase |
| `title()` | Convert to title case |
| `capitalize()` | Capitalize the first character |
| `strip()` | Remove surrounding whitespace |
| `replace()` | Replace part of a string |
| `split()` | Split into a list |
| `join()` | Join an iterable of strings |
| `find()` | Return the position of a substring |
| `count()` | Count occurrences |
| `startswith()` | Check the beginning |
| `endswith()` | Check the ending |

- Use `len()` to get the string length.
- Use f-strings for formatted strings.

## 8. Lists
- Lists are ordered and mutable.
- Lists allow duplicate values.
- Lists can contain values of different data types.
- Access elements using indexing and slicing.

| Method | Purpose |
|---|---|
| `append()` | Add one item at the end |
| `extend()` | Add multiple items |
| `insert()` | Add an item at a position |
| `remove()` | Remove the first matching item |
| `pop()` | Remove and return an item |
| `clear()` | Remove all items |
| `index()` | Find the position of an item |
| `count()` | Count matching items |
| `sort()` | Sort the list in place |
| `reverse()` | Reverse the list in place |
| `copy()` | Create a shallow copy |

## 9. Tuples
- Tuples are ordered and immutable.
- Tuples allow duplicate values.
- Access elements using indexing and slicing.
- Common methods: `count()` and `index()`.
- A single-item tuple requires a trailing comma.

## 10. Dictionaries
- Dictionaries store key-value pairs.
- Keys must be unique and hashable.
- Dictionaries are mutable.
- Access values using their keys.

| Method | Purpose |
|---|---|
| `keys()` | Return dictionary keys |
| `values()` | Return dictionary values |
| `items()` | Return key-value pairs |
| `get()` | Access a value safely |
| `update()` | Add or update entries |
| `pop()` | Remove an entry by key |
| `popitem()` | Remove the last inserted entry |
| `clear()` | Remove all entries |
| `copy()` | Create a shallow copy |
| `setdefault()` | Get a value or insert a default |

## 11. Sets
- Sets are unordered collections of unique elements.
- Sets are mutable.
- Sets do not support indexing.
- Use sets to remove duplicates and perform set operations.

| Method | Purpose |
|---|---|
| `add()` | Add an element |
| `update()` | Add multiple elements |
| `remove()` | Remove an element; raises an error if absent |
| `discard()` | Remove an element without an error if absent |
| `pop()` | Remove and return an arbitrary element |
| `clear()` | Remove all elements |

### Set Operations
- `union()` or `|`
- `intersection()` or `&`
- `difference()` or `-`
- `symmetric_difference()` or `^`
- `issubset()`
- `issuperset()`
- `isdisjoint()`

## 12. Functions
- Define a function using `def`.
- Parameters receive values passed to a function.
- Arguments are the values supplied when calling a function.
- `return` sends a result back to the caller.
- A function without an explicit return value returns `None`.
- Python supports positional, keyword, default, and variable-length arguments.
- `*args` collects extra positional arguments.
- `**kwargs` collects extra keyword arguments.
- A lambda is a small anonymous function.

## 13. Scope
- Local variables are defined inside a function.
- Global variables are defined outside functions.
- `global` allows reassignment of a global variable inside a function.
- `nonlocal` refers to a variable in an enclosing function scope.

## 14. Comprehensions
- List comprehension creates a list.
- Set comprehension creates a set.
- Dictionary comprehension creates a dictionary.
- Comprehensions can include conditions.
- Generator expressions produce values lazily.

## 15. Useful Built-in Functions

| Function | Purpose |
|---|---|
| `print()` | Display output |
| `input()` | Read user input |
| `len()` | Get length |
| `type()` | Check data type |
| `isinstance()` | Check an object's type |
| `int()` | Convert to integer |
| `float()` | Convert to float |
| `str()` | Convert to string |
| `bool()` | Convert to Boolean |
| `range()` | Generate a number sequence |
| `enumerate()` | Get index-value pairs |
| `zip()` | Combine iterables |
| `sum()` | Calculate total |
| `min()` | Get the minimum |
| `max()` | Get the maximum |
| `abs()` | Get absolute value |
| `round()` | Round a number |
| `sorted()` | Return a sorted list |
| `reversed()` | Return a reverse iterator |
| `any()` | Check whether any item is true |
| `all()` | Check whether all items are true |

## 16. Exception Handling
- Exceptions are errors that occur while a program runs.
- `try` contains code that may raise an exception.
- `except` handles an exception.
- `else` runs when no exception occurs.
- `finally` runs whether an exception occurs or not.
- `raise` triggers an exception.
- Use specific exception types when handling errors.

## 17. File Handling
- `open()` opens a file.
- Use `with` to manage files safely.
- `read()` reads file content.
- `readline()` reads one line.
- `readlines()` reads lines into a list.
- `write()` writes text.
- `writelines()` writes multiple strings.
- `close()` closes a file.

### File Modes
| Mode | Purpose |
|---|---|
| `r` | Read |
| `w` | Write; replaces existing content |
| `a` | Append |
| `x` | Create; fails if the file exists |
| `b` | Binary mode |
| `t` | Text mode |
| `+` | Read and write |

## 18. Modules and Packages
- A module is a Python file containing reusable code.
- A package groups related modules.
- `import` loads a module.
- `from ... import ...` imports selected names.
- `as` assigns an alias.
- `pip` installs external Python packages.
- `__name__` identifies the module's execution context.
- `if __name__ == "__main__":` marks code intended to run when the file is executed directly.

## 19. Object-Oriented Programming
- A class defines a type of object.
- An object is an instance of a class.
- `__init__()` initializes an object.
- `self` refers to the current instance.
- Encapsulation groups data and related methods.
- Inheritance allows a class to reuse another class's features.
- Polymorphism allows a common interface to have different implementations.
- Abstraction hides implementation details.

## 20. Common Keywords
`if`, `elif`, `else`, `for`, `while`, `break`, `continue`, `pass`, `def`, `return`, `class`, `try`, `except`, `finally`, `raise`, `import`, `from`, `as`, `with`, `lambda`, `yield`, `global`, `nonlocal`, `in`, `is`, `and`, `or`, `not`, `True`, `False`, `None`.

## 21. Important Reminders
- Python uses indentation to define blocks.
- Lists, dictionaries, and sets are mutable.
- Strings and tuples are immutable.
- `=` assigns a value; `==` compares values.
- `is` checks object identity; `==` checks equality.
- `append()` adds one item; `extend()` adds items from an iterable.
- `sort()` changes a list; `sorted()` returns a new sorted list.
