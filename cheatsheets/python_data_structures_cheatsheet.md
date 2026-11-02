# Python Data Structures and Functions

## 1. Lists

Lists are ordered and **mutable** collections.

```python
numbers = [10, 20, 30, 40]
```

### Common List Methods

- `append(x)` – Add an element at the end.
- `insert(i, x)` – Insert an element at a specific index.
- `extend(iterable)` – Add multiple elements.
- `remove(x)` – Remove the specified element.
- `pop(i)` – Remove and return an element at an index.
- `sort()` – Sort the list.
- `reverse()` – Reverse the list.
- `index(x)` – Return the index of an element.
- `count(x)` – Count occurrences of an element.
- `copy()` – Create a shallow copy.

### List Operations

```python
numbers[0] = 5       # Update element
numbers[0]           # Access element
numbers[1:3]         # Slice from index 1 to 2
numbers[:3]          # First three elements
numbers[2:]          # From index 2 onwards
numbers[::-1]        # Reverse using slicing

len(numbers)         # Get length
20 in numbers        # Check membership
```

## 2. Tuples

Tuples are ordered and **immutable** collections.

```python
coordinates = (10, 20, 30)
```

### Tuple Operations

```python
coordinates[0]          # Access element
len(coordinates)        # Get length
coordinates.count(10)   # Count occurrences
coordinates.index(20)   # Find element index
```

**Single element tuple:**

```python
single = (10,)
```

Remember to include the comma when creating a single-element tuple.

## 3. Dictionaries

Dictionaries store data in key-value pairs.

```python
student = {
    "name": "John",
    "age": 22,
    "marks": 85
}
```

### Common Dictionary Methods

- `get(key)` – Get a value safely.
- `keys()` – Return all keys.
- `values()` – Return all values.
- `items()` – Return key-value pairs.
- `update()` – Update or add items.
- `pop(key)` – Remove a specified key.
- `popitem()` – Remove the last inserted pair.
- `clear()` – Remove all items.
- `copy()` – Create a shallow copy.
- `setdefault()` – Return the value of a key or insert a default value if the key doesn't exist.

### Dictionary Operations

```python
student["name"]              # Access value
student["city"] = "Pune"     # Add item
student["age"] = 23          # Update item
student.get("marks")         # Safe access

"name" in student            # Check if key exists
len(student)                 # Number of items
```

### Loop Through a Dictionary

```python
# Loop through items
for key, value in student.items():
    print(key, value)

# Loop through keys
for key in student.keys():
    print(key)

# Loop through Values
for val in student.values():
    print(value)
```

### Create a dict from list
```python
data = [(1, "A"), (2, "B"), (3, "C")] # [(1, 'A'), (2, 'B'), (3, 'C')]

data_dict = dict(data) # {1: 'A', 2: 'B', 3: 'C'}
```

## 4. Sets

Sets store unique elements and are unordered.

```python
numbers = {1, 2, 3, 3, 4}
```

Duplicate elements are automatically removed.

### Common Set Methods

- `add(x)` – Add an element.
- `remove(x)` – Remove an element. Raises an error if it doesn't exist.
- `update(iterable)` – Add multiple elements.
- `clear()` – Remove all elements.

### Set Operations

```python
a = {1, 2, 3}
b = {3, 4, 5}

a.union(b)                  # Combine both sets
a.intersection(b)           # Get common elements
a.difference(b)             # Elements in a but not b
a.symmetric_difference(b)   # Elements in either, but not both
```

## 5. Functions

Functions are reusable blocks of code that perform a specific task.

### Defining and Calling a Function

```python
def greet(name):
    return f"Hello, {name}"

result = greet("Saad")
print(result)
```

### Default Arguments

Default arguments are used when no argument is provided.

```python
def greet(name="Guest"):
    print(f"Hello, {name}")

greet()          # Hello, Guest
greet("John")    # Hello, John
```

### Multiple Return Values

A function can return multiple values using a tuple.

```python
def calculate(a, b):
    return a + b, a - b

addition, subtraction = calculate(10, 5)
```

### Variable-Length Arguments

`*args` allows a function to accept multiple positional arguments.

```python
def add(*numbers):
    return sum(numbers)

add(10, 20, 30)
```

`**kwargs` allows a function to accept multiple keyword arguments.

```python
def details(**kwargs):
    print(kwargs)

details(name="John", age=25)
```

## 6. List Comprehension

List comprehension provides a shorter way to create lists.

### Example

```python
numbers = [1, 2, 3, 4, 5]

squares = [number ** 2 for number in numbers]

even_numbers = [number for number in numbers if number % 2 == 0]
```

### Equivalent Normal Loop

```python
squares = []

for number in numbers:
    squares.append(number ** 2)
```

## 7. Useful Built-in Functions

- `print()` – Display output.
- `input()` – Take user input.
- `len()` – Get the length.
- `type()` – Get the data type.
- `range()` – Generate a sequence of numbers.
- `sum()` – Calculate the sum.
- `min()` – Find the minimum value.
- `max()` – Find the maximum value.
- `sorted()` – Return a new sorted list.
- `reversed()` – Return a reverse iterator.
- `enumerate()` – Get index and value while iterating.
- `zip()` – Combine multiple iterables.
- `map()` – Apply a function to every element.
```python
>>> double = map(lambda i: i*2, a)
>>> print(list(double))
[2, 4, 6]
>>> 
```
- `filter()` – Filter elements based on a condition.
```python
>>> a = [1,2,3,4]
>>> evens = filter(lambda i: i % 2 == 0, a)
>>> print(list(evens))
[2, 4]
>>> 
```
- `any()` – Return `True` if at least one element is true.
- `all()` – Return `True` if all elements are true.
- `abs()` – Get the absolute value.
- `round()` – Round a number.

### Examples

```python
numbers = [10, 20, 30]

sum(numbers)
max(numbers)
min(numbers)
sorted(numbers, reverse=True)
```

### Enumerate

`enumerate()` provides both the index and value while iterating.

```python
names = ["John", "Sam", "Alex"]

for index, name in enumerate(names):
    print(index, name)

# 0 John
# 1 Sam
# 2 Alex
```

### Zip

`zip()` combines elements from multiple iterables.

```python
names = ["John", "Sam"]
ages = [20, 25]

for name, age in zip(names, ages):
    print(name, age)

# John 20
# Sam 25
```

## 13. File Handling

Python provides built-in functions to read from and write to files.

### Read a File

```python
with open("data.txt", "r") as file:
    content = file.read()
```

### Read Line by Line

```python
with open("data.txt", "r") as file:
    for line in file:
        print(line)
```

### Write to a File

```python
with open("data.txt", "w") as file:
    file.write("Hello Python")
```

Note: Write mode (`w`) overwrites existing content.

### Append to a File

```python
with open("data.txt", "a") as file:
    file.write("\nNew line")
```

Append mode (`a`) adds content without overwriting existing content.

### Common File Modes

- `r` – Read a file.
- `w` – Write to a file (overwrites existing content).
- `a` – Append content to a file.

**Important:** Always prefer `with open()` when working with files. It automatically closes the file after the operation is completed.
