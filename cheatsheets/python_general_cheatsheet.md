# Python General Cheatsheet

## 1. Variables and Data Types

```python
name = "A"          # String
age = 22               # Integer
price = 99.5            # Float
is_active = True        # Boolean
nothing = None          # NoneType

type(age)               # Check data type
```

### Type Conversion

```python
int("10")
float("10.5")
str(100)
list("hello")
```
## 2. Operators

### Arithmetic Operators

* `+` : Addition (5 + 2)
* `-` : Subtraction (5 - 2)
* `*` : Multiplication (5 * 2)
* `/` : Division (5 / 2)
* `//` : Floor division (5 // 2)
* `%` : Modulus (5 % 2)
* `**` : Exponent (5 ** 2)

### Comparison Operators

* `==` : Equal to (a == b)
* `!=` : Not equal to (a != b)
* `>` : Greater than (a > b)
* `<` : Less than (a < b)
* `>=` : Greater than or equal to (a >= b)
* `<=` : Less than or equal to (a <= b)

### Logical Operators

* `and` : Logical AND (a and b)
* `or` : Logical OR (a or b)
* `not` : Logical NOT (not a)

### Membership Operators

* `in` : Checks whether an element exists in a sequence (a in numbers)
* `not in` : Checks whether an element does not exist in a sequence (a not in numbers)

## 3. Conditional Statements

```python
age = 20

if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teenager")
else:
    print("Child")
```

### Ternary Operator

```python
result = "Adult" if age >= 18 else "Minor"
```

## 4. Loops

### For Loop

```python
for number in range(5):
    print(number)
```

### Range

```python
range(5)          # 0 to 4
range(1, 5)       # 1 to 4
range(1, 10, 2)   # 1, 3, 5, 7, 9
```

### While Loop

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

### Loop Control

```python
break       # Exit loop
continue    # Skip current iteration
pass        # Do nothing
```

## 5. Strings

```python
name = "Python Programming"

name[0]                  # P
name[-1]                 # g
name[0:6]                # Python

name.lower()             # Lowercase
name.upper()             # Uppercase
name.title()             # Title case
name.strip()             # Remove surrounding whitespace
name.replace("Python", "Java")
name.split()             # Split into a list
name.startswith("Python")
name.endswith("ing")
name.find("Pro")
len(name)                # Length
```

### String Formatting

```python
name = "John Doe"
age = 30

print(f"My name is {name} and I am {age} years old.")
```
