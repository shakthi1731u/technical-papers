# Python Dictionary Cheatsheet

## 1. What is a dictionary?

A dictionary is a Python data type that stores information as **key-value pairs**.

Each key must be unique. We use a key to access its value.

```python
student = {
    "name": "Arun",
    "age": 22,
    "course": "B.E."
}
```

Here, `name`, `age`, and `course` are keys. Their related information is stored as values.

## 2. Create a dictionary

Create an empty dictionary:

```python
student = {}
```

Create a dictionary with values:

```python
student = {"name": "Arun", "age": 22}
```

Using the `dict()` constructor:

```python
student = dict(name="Arun", age=22)
```

## 3. Access dictionary values

Use the key inside square brackets:

```python
student["name"]
```

Use `get()`:

```python
student.get("age")
```

Give a default value if the key does not exist:

```python
student.get("city", "Not available")
```

**Difference:** Square brackets raise a `KeyError` when the key is missing. `get()` returns `None` by default, or the default value you provide.

## 4. Add and update values

Add a new key:

```python
student["city"] = "Chennai"
```

Update an existing value:

```python
student["age"] = 23
```

Update multiple values:

```python
student.update({
    "age": 24,
    "course": "Computer Science"
})
```

## 5. Remove dictionary items

Remove an item and return its value:

```python
student.pop("age")
```

Remove an item using its key:

```python
del student["city"]
```

Remove the last inserted item:

```python
student.popitem()
```

Remove all items:

```python
student.clear()
```

## 6. Check whether a key exists

Use the `in` operator:

```python
"name" in student
```

Check whether a key is missing:

```python
"phone" not in student
```

The `in` operator checks dictionary keys, not values.

## 7. Get keys, values, and items

Get all keys:

```python
student.keys()
```

Get all values:

```python
student.values()
```

Get key-value pairs:

```python
student.items()
```

## 8. Loop through a dictionary

Loop through keys:

```python
for key in student:
    print(key)
```

Loop through values:

```python
for value in student.values():
    print(value)
```

Loop through keys and values:

```python
for key, value in student.items():
    print(key, value)
```

## 9. Useful dictionary methods

| Method | Purpose |
|---|---|
| `get(key)` | Get a value without raising an error for a missing key |
| `keys()` | Get the dictionary's keys |
| `values()` | Get the dictionary's values |
| `items()` | Get key-value pairs |
| `update()` | Add or update multiple items |
| `pop(key)` | Remove an item by key |
| `popitem()` | Remove the last inserted item |
| `clear()` | Remove all items |
| `copy()` | Create a shallow copy |
| `setdefault()` | Get a value; add the key with a default if missing |

Example of `setdefault()`:

```python
student.setdefault("country", "India")
```

This adds `country` only if it does not already exist.

## 10. Dictionary length and copy

Find the number of items:

```python
len(student)
```

Create a shallow copy:

```python
student_copy = student.copy()
```

A shallow copy creates a new dictionary, but nested objects are still shared.

## 11. Nested dictionaries

A dictionary can contain another dictionary as a value.

```python
students = {
    "student1": {"name": "Arun", "age": 22},
    "student2": {"name": "Bala", "age": 21}
}
```

Access a nested value:

```python
students["student1"]["name"]
```

## 12. Dictionary comprehension

Dictionary comprehension creates a dictionary in a short form.

```python
squares = {
    number: number * number
    for number in range(1, 4)
}
```

Result:

```python
{1: 1, 2: 4, 3: 9}
```

## 13. Important points

- Dictionary keys must be unique and hashable.
- Values can have different data types.
- Dictionaries preserve insertion order in modern Python.
- Adding a value with an existing key updates that value.
- The `in` operator checks keys.
- A missing key used with square brackets raises `KeyError`.
- `get()` is useful when a key may not exist.

## Quick revision

| Task | Syntax |
|---|---|
| Create | `data = {}` |
| Access | `data["key"]` |
| Safe access | `data.get("key")` |
| Add or update | `data["key"] = value` |
| Remove | `data.pop("key")` |
| Check key | `"key" in data` |
| Loop | `for key, value in data.items():` |
| Count items | `len(data)` |
| Copy | `data.copy()` |

## Remember

A dictionary is useful when we want to store related information and access it using meaningful keys instead of numeric positions.
