# Python Arrays Cheat Sheet

## 1. Python Collections
- A list stores multiple values in one variable.
- Lists are ordered, mutable, and allow duplicate values.
- Lists can contain different data types.
- The built-in `array` module provides typed arrays.
- NumPy arrays are commonly used for numerical computing.

## 2. Creating a List
- Use square brackets to create a list.
- Use `list()` to convert an iterable into a list.
- Use `range()` to generate a sequence of integers.
- An empty list contains no elements.

## 3. Creating a Typed Array
- Import `array` from the `array` module.
- Create an array with a type code and an initializer.
- A typed array stores values of one specified type.
- Available type codes and item sizes can vary by platform.
- Check `typecode` and `itemsize` to inspect an array.

## 4. Indexing
- Indexing accesses an element by its position.
- Positive indexing starts at `0`.
- Negative indexing starts at `-1` from the end.
- An invalid index raises `IndexError`.

## 5. Slicing
- Slicing extracts part of a sequence.
- General form: `sequence[start:stop:step]`.
- The start position is included.
- The stop position is excluded.
- The step controls the interval between selected elements.
- Omitted values use the default positions and step.

## 6. Traversing
- Use `for` to visit each element.
- Use `enumerate()` when both index and value are needed.
- Use `while` when repetition depends on a condition.
- Avoid changing the collection's size while iterating over it.

## 7. Adding Elements

| Method | Purpose |
|---|---|
| `append()` | Add one element at the end |
| `extend()` | Add elements from an iterable |
| `insert()` | Add an element at a specified position |

- Lists and typed arrays support these methods.

## 8. Removing Elements

| Operation | Purpose |
|---|---|
| `remove(value)` | Remove the first matching value |
| `pop()` | Remove and return the last element |
| `pop(index)` | Remove and return an element at an index |
| `del` | Delete an element or slice |
| `clear()` | Remove all elements |

- `remove()` raises `ValueError` if the value is absent.
- `pop()` raises `IndexError` if the requested index is invalid.

## 9. Updating Elements
- Assign a new value using its index.
- Lists support slice assignment.
- Typed arrays accept only values compatible with their type code.

## 10. Searching
- `in` checks whether a value exists.
- `not in` checks whether a value is absent.
- `index()` returns the first matching position.
- `count()` returns the number of occurrences.
- `index()` raises `ValueError` when the value is absent.

## 11. Sorting and Reversing
- `sort()` sorts a list in place.
- `sorted()` returns a new sorted list.
- `reverse()` reverses a list in place.
- `reversed()` returns a reverse iterator.
- Sorting supports the `key` and `reverse` arguments.
- The default sorting order is ascending.

## 12. Common Operations

| Function | Purpose |
|---|---|
| `len()` | Return the number of elements |
| `min()` | Return the smallest element |
| `max()` | Return the largest element |
| `sum()` | Return the total of numeric elements |
| `any()` | Check whether at least one element is truthy |
| `all()` | Check whether every element is truthy |

- `min()`, `max()`, and `sum()` require compatible values.

## 13. Copying
- Assignment creates another reference to the same list.
- `copy()` creates a shallow copy.
- Slicing can also create a shallow list copy.
- A shallow copy does not independently copy nested mutable objects.
- Use `deepcopy()` from the `copy` module for a deep copy.

## 14. List Comprehension
- A list comprehension creates a list from an iterable.
- It can contain an expression, a loop, and an optional condition.
- Use a regular loop when the logic becomes difficult to read.

## 15. Two-Dimensional Lists
- A two-dimensional list contains nested lists.
- Use a row index and a column index to access an element.
- Nested loops can traverse rows and columns.
- Each inner list represents a row.
- Create rows independently to avoid shared references.

## 16. List, Typed Array, and NumPy Array

| Feature | List | `array.array` | NumPy array |
|---|---|---|---|
| Data types | Can be mixed | One type code | Usually one data type |
| Built into Python | Yes | Yes | No |
| Numerical operations | Basic | Basic | Extensive |
| Main use | General collections | Typed values | Numerical computing |

## 17. Common Errors
- `IndexError`: an index is outside the valid range.
- `ValueError`: a requested value is not present.
- `TypeError`: an operation or value has an incompatible type.
- Typed arrays reject values that do not match their type.
- Modifying a list during iteration can produce unexpected results.

## 18. Important Reminders
- Indexing starts at `0`.
- Negative indexing starts at `-1`.
- The stop position in a slice is excluded.
- Lists are mutable; tuples are immutable.
- `append()` adds one item; `extend()` adds items from an iterable.
- `remove()` deletes by value; `pop()` deletes by index and returns the item.
- `sort()` changes the original list; `sorted()` returns a new list.
- A Python list is different from a typed array created with the `array` module.
