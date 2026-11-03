# Python PEP 8 Cheatsheet

PEP 8 is the official Python style guide. It helps keep Python code clean and readable.

## 1. Indentation

- Use **4 spaces** for indentation.
- Do not use tabs.

```python
if age >= 18:
    print("Adult")
```

## 2. Variable Names

- Use `snake_case` for variables.
- Use meaningful names.

```python
user_name = "John"
total_marks = 450
```

## 3. Function Names

- Use `snake_case` for function names.
- Use descriptive names.

```python
def calculate_total():
    return 100
```

## 4. Class Names

- Use `PascalCase` for class names.

```python
class StudentDetails:
    pass
```

## 5. Constants

- Use uppercase letters.
- Separate words with underscores.

```python
MAX_SIZE = 100
PI_VALUE = 3.14
```

## 6. Line Length

- Keep each line within **79 characters**.
- Break long lines when necessary.

```python
total = calculate_total(
    first_number,
    second_number
)
```

## 7. Blank Lines

- Use 2 blank lines between functions and classes.
- Use 1 blank line between methods inside a class.
- Use blank lines to separate logical sections.

## 8. Imports

- Put imports at the top of the file.
- Use one import per line.
- Avoid unnecessary imports.

```python
import csv
import os

from datetime import date
```

## 9. Spaces

- Use spaces around operators.
- Do not add unnecessary spaces inside brackets.

```python
total = 10 + 20

numbers = [10, 20, 30]
```

## 10. Comments

- Write clear and meaningful comments.
- Keep comments updated with the code.

```python
# Calculate the total marks
total_marks = 450
```

## 11. String Quotes

- Use single or double quotes consistently.

```python
name = "John"
city = 'London'
```

## 12. Avoid Unnecessary Code

- Avoid unused variables.
- Avoid unused imports.
- Use meaningful variable names.
- Keep functions simple and focused.

---

## Quick Reference

| Rule | Convention |
|---|---|
| Indentation | 4 spaces |
| Variable names | `snake_case` |
| Function names | `snake_case` |
| Class names | `PascalCase` |
| Constants | `UPPER_CASE` |
| Maximum line length | 79 characters |
| Blank lines | 2 between functions/classes |
| Imports | At the top |
| Operators | Spaces around operators |
| Comments | Clear and meaningful |
