# AMA 10th Nov, 2026

## 1. What is Abstraction?

**Abstraction** is an OOP concept that hides implementation details and exposes only the essential functionality to the user.

It focuses on **what an object does rather than how it does it.**

### Example

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):

    @abstractmethod
    def start(self):
        pass


class Car(Vehicle):

    def start(self):
        print("Car started")


car = Car()
car.start()
```

**Explanation:**
- `Vehicle` is an abstract class.
- `start()` is an abstract method.
- Child classes must implement abstract methods.
- We cannot create objects directly from an abstract class containing unimplemented abstract methods.

**Real-world example:** When driving a car, you use the steering wheel, accelerator and brakes without needing to understand the internal engine mechanism.

---

## 2. Why Do We Use Property Decorators?

The `@property` decorator allows us to access a method like an attribute.

It is commonly used for:
- Controlling access to class attributes.
- Implementing getters and setters.
- Validating values before assigning them.
- Hiding internal implementation details.

### Example

```python
class Employee:

    def __init__(self, salary):
        self._salary = salary

    @property
    def salary(self):
        return self._salary

    @salary.setter
    def salary(self, value):
        if value < 0:
            raise ValueError("Salary cannot be negative")

        self._salary = value


emp = Employee(30000)

print(emp.salary)

emp.salary = 40000

print(emp.salary)
```

**Explanation:**
- `@property` allows us to read `salary` like an attribute.
- `@salary.setter` allows us to control how salary is modified.
- We can validate values before assigning them.

Without property decorators, we would typically use separate getter and setter methods.

---

## 3. What Is the Purpose of `&&` in CLI?

In Linux, `&&` is a logical AND operator used to execute multiple commands sequentially.

The second command executes only if the first command succeeds (returns exit status 0).

### Example

```bash
mkdir project && cd project
```

**Explanation:**
1. First, the `project` directory is created.
2. If directory creation succeeds, the terminal moves into that directory.
3. If directory creation fails, the second command is not executed.

### Difference Between `&&` and `;`

```bash
mkdir project && cd project
```

The second command executes only if the first succeeds.

```bash
mkdir project; cd project
```

The second command executes regardless of whether the first succeeds.

---

## 4. How Do We Make a Variable Private in Python?

Python does not provide strictly private variables like some other programming languages.

Instead, it uses naming conventions and name mangling.

### Single Underscore

```python
class Employee:

    def __init__(self):
        self._salary = 30000
```

`_salary` indicates that the variable is intended for internal use.

It is still accessible from outside the class.

### Double Underscore

```python
class Employee:

    def __init__(self):
        self.__salary = 30000
```

Double underscores trigger **name mangling**.

Python internally changes `__salary` to `_Employee__salary`.

This makes accidental access and overriding more difficult.

---

## 5. How Can We Access a Private Variable Outside a Class?

Python uses name mangling for double-underscore attributes.

### Example

```python
class Employee:

    def __init__(self):
        self.__salary = 30000


emp = Employee()

print(emp._Employee__salary)
```

Output:

```text
30000
```

We can also expose the variable through a property:

```python
class Employee:

    def __init__(self):
        self.__salary = 30000

    @property
    def salary(self):
        return self.__salary


emp = Employee()

print(emp.salary)
```

**Important:** Double underscores do not provide absolute privacy. They mainly prevent accidental access and name collisions.

---

## 6. Difference Between Encapsulation and Abstraction

Both are important OOP concepts, but they solve different problems.

| Encapsulation | Abstraction |
|---|---|
| Combines data and methods inside a class. | Hides implementation details. |
| Controls access to internal data. | Exposes essential functionality. |
| Focuses on data protection and management. | Focuses on simplicity and usability. |
| Uses access conventions, properties and methods. | Commonly implemented using abstract classes and interfaces. |

### Encapsulation Example

```python
class BankAccount:

    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance
```

Here, the balance is managed inside the class.

### Abstraction Example

```python
from abc import ABC, abstractmethod

class Payment(ABC):

    @abstractmethod
    def pay(self, amount):
        pass


class CreditCard(Payment):

    def pay(self, amount):
        print(f"Paid {amount} using credit card")
```

Here, the user only needs to know that the payment object supports `pay()`. Its internal implementation is hidden.

---

## 7. What Is Hybrid Inheritance?

Hybrid inheritance is a combination of two or more types of inheritance.

Python supports:
1. Single inheritance
2. Multiple inheritance
3. Multilevel inheritance
4. Hierarchical inheritance
5. Hybrid inheritance

### Example

```python
class Person:
    def introduce(self):
        print("I am a person")


class Employee(Person):
    def work(self):
        print("I work")


class Student(Person):
    def study(self):
        print("I study")


class Intern(Employee, Student):
    def internship(self):
        print("I am an intern")


intern = Intern()

intern.introduce()
intern.work()
intern.study()
intern.internship()
```

**Explanation:**

- `Employee` inherits from `Person`.
- `Student` inherits from `Person`.
- `Intern` inherits from both `Employee` and `Student`.

This combines hierarchical, multilevel and multiple inheritance.

---

## 8. How to Delete a Specific Key From a Dictionary?

Python provides two common approaches.

### Using `del`

```python
student = {
    "name": "John",
    "age": 22,
    "grade": "A"
}

del student["age"]

print(student)
```

Output:

```python
{'name': 'John', 'grade': 'A'}
```

### Using `pop()`

```python
student = {
    "name": "John",
    "age": 22,
    "grade": "A"
}

student.pop("age")

print(student)
```

**Difference:**
- `del` removes a key directly.
- `pop()` removes the key and returns its value.
- `pop()` can also accept a default value to avoid a `KeyError`.

Example:

```python
student.pop("marks", None)
```

---

## 9. How to Remove an Element From a Set?

Python provides three methods.

### Using `remove()`

```python
numbers = {1, 2, 3, 4}

numbers.remove(3)

print(numbers)
```

Raises `KeyError` if the element does not exist.

### Using `discard()`

```python
numbers = {1, 2, 3, 4}

numbers.discard(5)

print(numbers)
```

Does not raise an error if the element does not exist.

### Using `pop()`

```python
numbers = {1, 2, 3, 4}

numbers.pop()

print(numbers)
```

Removes and returns an arbitrary element from the set.

**Remember:** Sets are unordered, so `pop()` does not guarantee which element will be removed.

---

## 10. How to Find the Length of a String?

Python provides the built-in `len()` function.

```python
name = "Python"

print(len(name))
```

Output:

```text
6
```

`len()` returns the number of characters in a string.

It also works with lists, tuples, dictionaries and sets.

---

## 11. Difference Between Compile-Time and Runtime Polymorphism

### Compile-Time Polymorphism

The method to be executed is determined before runtime based on the method signature or arguments.

- Also called static polymorphism.
- Commonly achieved through method overloading.
- Python does not natively support traditional method overloading.

### Runtime Polymorphism

The method to be executed is determined at runtime based on the actual object.

- Also called dynamic polymorphism.
- Achieved through method overriding.
- Supported in Python.

### Example

```python
class Animal:
    def sound(self):
        print("Animal sound")


class Dog(Animal):
    def sound(self):
        print("Bark")


animal = Dog()
animal.sound()  # Bark
```

### Key Differences

| Compile-Time | Runtime |
|---|---|
| Static polymorphism | Dynamic polymorphism |
| Method overloading | Method overriding |
| Resolved before execution | Resolved during execution |
| Example: Java overloading | Example: Python overriding |

**Important:** Python supports runtime polymorphism through method overriding but does not natively support traditional method overloading.

---

## 12. Difference Between `*args` and `**kwargs`

Both allow functions to accept a variable number of arguments.

### `*args`

Used to accept multiple positional arguments.

They are collected into a tuple.

```python
def add(*args):
    print(args)

    return sum(args)


print(add(10, 20, 30))
```

Output:

```text
(10, 20, 30)
60
```

### `**kwargs`

Used to accept multiple keyword arguments.

They are collected into a dictionary.

```python
def student(**kwargs):
    print(kwargs)


student(name="John", age=22, grade="A")
```

Output:

```text
{'name': 'John', 'age': 22, 'grade': 'A'}
```

### Difference

| `*args` | `**kwargs` |
|---|---|
| Accepts positional arguments. | Accepts keyword arguments. |
| Collects arguments into a tuple. | Collects arguments into a dictionary. |
| Uses one asterisk. | Uses two asterisks. |

They can also be used together:

```python
def example(*args, **kwargs):
    print(args)
    print(kwargs)


example(10, 20, name="John", age=22)
```

---

## 13. What Is the Command for Force Remove in Linux?

The `rm` command removes files and directories.

### Remove a File

```bash
rm file.txt
```

### Force Remove a File

```bash
rm -f file.txt
```

The `-f` flag forces removal without prompting and ignores nonexistent files.

### Remove a Directory and Its Contents

```bash
rm -r folder
```

### Force Remove a Directory and Its Contents

```bash
rm -rf folder
```

**Important flags:**

- `-r`: Recursively removes directories and their contents.
- `-f`: Forces removal without prompting.

**Warning:** `rm -rf` is destructive. Always verify the path before executing it.

---

## 14. What Are SOLID Principles in OOP?

SOLID is a set of five design principles that help us write maintainable, flexible and reusable object-oriented code.

### S – Single Responsibility Principle

A class should have only one reason to change.

Each class should have one primary responsibility.

**Example:** An `Invoice` class should handle invoice-related data, while a separate `InvoicePrinter` class should handle printing.

### O – Open/Closed Principle

Software entities should be open for extension but closed for modification.

We should be able to introduce new functionality without constantly modifying existing, tested code.

**Example:**

```python
class Payment:
    def pay(self):
        pass


class CreditCardPayment(Payment):
    def pay(self):
        print("Credit card payment")


class UpiPayment(Payment):
    def pay(self):
        print("UPI payment")
```

New payment methods can be added by extending the existing design.

### L – Liskov Substitution Principle

Objects of a child class should be replaceable with objects of their parent class without breaking the expected behaviour of the program.

**Example:** If a function accepts a `Bird`, every subclass passed to it should behave according to the expectations of that function.

### I – Interface Segregation Principle

A class should not be forced to implement methods it does not need.

Instead of creating one large interface, create smaller, specific interfaces.

**Example:** A `Printer` interface should not force a basic printer to implement scanning and faxing methods.

### D – Dependency Inversion Principle

High-level modules should not depend directly on low-level modules. Both should depend on abstractions.

**Example:** An order service should depend on a payment interface rather than directly depending on a specific payment provider.

**Summary:**

| Principle | Meaning |
|---|---|
| S | One class, one responsibility. |
| O | Extend functionality without modifying existing code unnecessarily. |
| L | Child classes should be substitutable for parent classes. |
| I | Prefer smaller, specific interfaces. |
| D | Depend on abstractions, not concrete implementations. |

---

## 15. What Is the Real Need for Classes and Objects?

Classes and objects help us organize code by grouping related data and functionality together.

### Class

A class is a blueprint or template used to create objects.

### Object

An object is an instance of a class.

### Example

```python
class Car:

    def __init__(self, brand, color):
        self.brand = brand
        self.color = color

    def drive(self):
        print(f"{self.brand} is driving")


car1 = Car("BMW", "Black")
car2 = Car("Audi", "White")

car1.drive()
car2.drive()
```

### Why Do We Need Them?

1. **Code Reusability:** We can create multiple objects using one class.
2. **Encapsulation:** We can group related data and methods together.
3. **Maintainability:** Code becomes easier to organize and maintain.
4. **Scalability:** We can extend existing classes through inheritance and composition.
5. **Real-world Modelling:** We can represent entities such as employees, cars, accounts and products.

Without classes, we might end up writing repetitive code for every individual entity.

---

## 16. Useful Python String Methods

Python provides several built-in string methods for manipulating strings.

Remember that strings are immutable. These methods return new strings rather than modifying the original string.

### Case Conversion

```python
text = "hello python"

print(text.upper())       # HELLO PYTHON
print(text.lower())       # hello python
print(text.capitalize())  # Hello python
print(text.title())       # Hello Python
print(text.swapcase())    # HELLO PYTHON
```

### Removing Whitespace

```python
text = "  Hello Python  "

print(text.strip())   # Removes whitespace from both ends
print(text.lstrip())  # Removes whitespace from the left
print(text.rstrip())  # Removes whitespace from the right
```

To remove all whitespace, including spaces between words:

```python
text = "Hello Python World"

result = "".join(text.split())

print(result)
```

Output:

```text
HelloPythonWorld
```

### Searching and Checking

```python
text = "Hello Python"

print(text.find("Python"))       # 6
print(text.index("Python"))      # 6
print(text.startswith("Hello"))  # True
print(text.endswith("Python"))   # True
print(text.count("o"))           # 2
```

### Replacing and Splitting

```python
text = "Hello Python"

print(text.replace("Python", "World"))

print(text.split())
```

Output:

```text
Hello World
['Hello', 'Python']
```

### Joining

```python
words = ["Hello", "Python", "World"]

print(" ".join(words))
```

Output:

```text
Hello Python World
```

### Checking String Content

```python
print("123".isdigit())       # True
print("hello".isalpha())     # True
print("hello123".isalnum())  # True
print("HELLO".isupper())     # True
print("hello".islower())     # True
print("   ".isspace())       # True
```

### Formatting

```python
name = "John"
age = 22

print("My name is {} and I am {} years old".format(name, age))
```

You can also use f-strings:

```python
print(f"My name is {name} and I am {age} years old")
```

---

# Quick Revision

| Topic | Important Concept / Method |
|---|---|
| Abstraction | `ABC`, `@abstractmethod` |
| Property decorators | `@property`, `@setter` |
| CLI `&&` | Execute second command only on success |
| Private variables | `_variable`, `__variable` |
| Encapsulation | Bundling data and methods |
| Hybrid inheritance | Combination of inheritance types |
| Delete dictionary key | `del`, `pop()` |
| Remove set element | `remove()`, `discard()`, `pop()` |
| String length | `len()` |
| Compile time vs runtime | Compilation vs execution |
| Variable arguments | `*args`, `**kwargs` |
| Force remove | `rm -rf` |
| Access private variable | Name mangling |
| SOLID | Five OOP design principles |
| Classes and objects | Blueprint and instance |
| String methods | `strip()`, `split()`, `join()`, `replace()` |

**End of Notes**
