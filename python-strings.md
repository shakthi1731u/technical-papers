# Python Strings

## 1. What is a string?

A string is a sequence of characters. We use strings to store text such as names, messages, and sentences.

Strings can use single quotes, double quotes, or triple quotes.

```python
first_name = 'Arun'
last_name = "Kumar"
message = """Welcome to Python"""
```

## 2. Access characters

String indexing starts at `0`.

```python
word = "Python"

print(word[0])   # P
print(word[1])   # y
print(word[-1])  # n
```

Negative indexing starts from the end. `-1` refers to the last character.

## 3. String slicing

Slicing extracts part of a string.

Syntax:

```python
string[start:stop:step]
```

The `stop` position is not included.

```python
word = "Python"

print(word[0:2])   # Py
print(word[2:])    # thon
print(word[:4])    # Pyth
print(word[::2])   # Pto
print(word[::-1])  # nohtyP
```

## 4. Strings are immutable

Strings are immutable. We cannot replace one character directly after creating a string.

This raises an error:

```python
word = "Python"
word[0] = "J"
```

Instead, create a new string:

```python
word = "Python"
word = "J" + word[1:]
```

## 5. Common string methods

| Method | Purpose |
|---|---|
| `lower()` | Converts letters to lowercase |
| `upper()` | Converts letters to uppercase |
| `title()` | Capitalises each word |
| `capitalize()` | Capitalises the first character |
| `swapcase()` | Changes uppercase to lowercase and vice versa |
| `strip()` | Removes whitespace from both ends |
| `lstrip()` | Removes whitespace from the left |
| `rstrip()` | Removes whitespace from the right |
| `replace(old, new)` | Replaces part of a string |
| `find(value)` | Returns first position, or `-1` |
| `count(value)` | Counts occurrences |
| `startswith(value)` | Checks the beginning |
| `endswith(value)` | Checks the ending |

Example:

```python
message = "  hello python  "

print(message.strip())
print(message.upper())
print(message.replace("python", "world"))
```

## 6. Split and Join

### `split()`

Splits a string into a list.

```python
sentence = "Python is easy"
words = sentence.split()
```

Output:

```python
["Python", "is", "easy"]
```

Provide a separator when needed:

```python
data = "apple,banana,mango"
fruits = data.split(",")
```

### `join()`

Joins strings from an iterable. The string before `.join()` becomes the separator.

```python
words = ["Python", "is", "easy"]
sentence = " ".join(words)
```

## 7. Check string contents

| Method | Returns `True` when |
|---|---|
| `isalpha()` | All characters are letters |
| `isdigit()` | All characters are digits |
| `isalnum()` | All characters are letters or digits |
| `isspace()` | All characters are whitespace |
| `islower()` | All cased characters are lowercase |
| `isupper()` | All cased characters are uppercase |
| `istitle()` | The string follows title-case rules |

Example:

```python
value = "Python123"

print(value.isalnum())
print(value.isalpha())
print(value.isdigit())
```

These methods return Boolean values: `True` or `False`.

## 8. String concatenation

The `+` operator joins strings.

```python
first_name = "Arun"
last_name = "Kumar"

full_name = first_name + " " + last_name
```

Both values must be strings when using `+` for concatenation.

## 9. String formatting

### f-strings

F-strings insert values into a string.

```python
name = "Arun"
age = 22

message = f"My name is {name} and I am {age} years old."
```

### `format()`

```python
message = "My name is {} and I am {} years old.".format(
    "Arun", 22
)
```

### Format decimal places

```python
price = 25.6789
print(f"{price:.2f}")
```

This displays two digits after the decimal point.

## 10. Useful built-in functions

| Function | Purpose |
|---|---|
| `len(text)` | Returns the number of characters |
| `str(value)` | Converts a value to a string |
| `min(text)` | Smallest character by Unicode value |
| `max(text)` | Largest character by Unicode value |
| `sorted(text)` | Returns a sorted list of characters |

Example:

```python
word = "python"
print(len(word))
print(sorted(word))
```

## 11. Loop through a string

```python
word = "Python"

for character in word:
    print(character)
```

Loop using indexes:

```python
for index in range(len(word)):
    print(index, word[index])
```

## 12. Check whether text exists

Use the `in` and `not in` operators:

```python
message = "Python is useful"

print("Python" in message)
print("Java" not in message)
```

String checks are case-sensitive. `"python"` and `"Python"` are different strings.

## 13. Escape characters

| Escape | Meaning |
|---|---|
| `\n` | New line |
| `\t` | Tab |
| `\\` | Backslash |
| `\\'` | Single quote |
| `\\"` | Double quote |

Example:

```python
message = "Hello\nPython"
print(message)
```

## Quick Revision

| Task | Syntax |
|---|---|
| First character | `text[0]` |
| Last character | `text[-1]` |
| Slice | `text[start:stop]` |
| Reverse | `text[::-1]` |
| Length | `len(text)` |
| Lowercase | `text.lower()` |
| Uppercase | `text.upper()` |
| Remove edge spaces | `text.strip()` |
| Split into a list | `text.split()` |
| Join items | `" ".join(items)` |
| Replace text | `text.replace(old, new)` |
| Check substring | `"word" in text` |

## Remember

- Strings are ordered sequences of characters.
- Indexing starts at `0`.
- Negative indexes access characters from the end.
- The stop position in slicing is excluded.
- Strings are immutable.
- String methods return new strings rather than changing the original.
