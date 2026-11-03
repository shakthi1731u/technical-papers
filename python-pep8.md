# Python PEP 8

## 1. What is PEP 8?

PEP 8 is the official Python style guide. It gives recommendations for writing clean and readable Python code.

It covers naming, indentation, spacing, imports, comments, and code layout. Following these rules makes code easier for other developers to understand and maintain.

## 2. Indentation

- Use **4 spaces** for each indentation level.
- Do not mix tabs and spaces.
- Keep the same indentation level inside a code block.

Example:

```python
if total > 100:
    print("High total")
```

## 3. Naming conventions

Use clear names that explain the purpose of a variable, function, or class.

| Type | Style | Example |
|---|---|---|
| Variable | `lowercase_with_underscores` | `total_runs` |
| Function | `lowercase_with_underscores` | `calculate_total()` |
| Class | `CapitalizedWords` | `DeliveryReport` |
| Constant | `ALL_CAPS` | `MAX_RETRIES` |
| Module | Short lowercase name | `data_reader.py` |

Avoid unclear names such as `x1`, `abc`, or `temp` when a descriptive name is possible.

## 4. Spaces and blank lines

Use spaces around operators:

```python
total = price + tax
```

Avoid unnecessary spaces inside brackets:

```python
numbers = [1, 2, 3]
```

Use:
- Two blank lines between top-level functions and classes.
- One blank line between methods inside a class.
- Blank lines to separate logical sections of code.

## 5. Line length

PEP 8 recommends keeping normal code lines within **79 characters**.

If a line becomes too long, split it into readable parts.

Example:

```python
total_amount = (
    product_price
    + delivery_charge
    + tax_amount
)
```

## 6. Imports

Place imports at the top of the file.

A common order is:

1. Standard library imports.
2. Third-party library imports.
3. Project-specific imports.

Example:

```python
import csv
import os

import pandas as pd

from src.data_reader import read_data
```

Avoid wildcard imports:

```python
from module import *
```

They make it harder to identify where names come from.

## 7. Comments and docstrings

Use comments to explain why a particular part of the code is needed. Avoid comments that only repeat what the code already says.

Use docstrings to describe functions, classes, and modules.

Example:

```python
def calculate_total(numbers):
    """Return the sum of all numbers."""
    return sum(numbers)
```

## 8. Useful tools

**pycodestyle** checks Python code against many PEP 8 style rules.

Install it:

```bash
python -m pip install pycodestyle
```

Check a Python file:

```bash
pycodestyle main.py
```

**Black** is a code formatter that automatically applies a consistent style.

Install and run it:

```bash
python -m pip install black
black main.py
```

## Quick revision

- Indentation: 4 spaces.
- Variables and functions: `snake_case`.
- Classes: `PascalCase`.
- Constants: `UPPER_CASE`.
- Imports: keep them at the top.
- Spacing: use spaces around operators.
- Comments: explain the reason when needed.
- Line length: aim for 79 characters.

## Remember

PEP 8 is a style guide, not a rule that changes how Python runs. Code can run without following PEP 8, but a consistent style makes it easier to read, review, and maintain.
