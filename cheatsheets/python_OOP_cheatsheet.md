# Object Oriented Programming in Python
Object-Oriented Programming (OOP) is a programming paradigm that organizes software design around data, or "objects" rather than functions and logic. Instead of writing a continuous sequence of instructions, you break your program down into small, reusable pieces modeled after real world things

## Core Concepts in OOP
* **Class**: A blueprint or template for creating an object.
```python
class Car:
    def __init__(self, manufacturer, model, color):
        self.manufacturer = manufacturer
        self.model = model
        self.color = color
```
* **Object**: An instance of class.
```python
toyota_supra = Car("Toyota", "Supra", "Black")
```
* **Attributes**: The data in a class
```python
toyota_supra = Car("Toyota", "Supra", "Black")
# (In the above example, "Toyota", "Supra" and "Black" are attributes)
```
* **Methods**: The actions of behaviors that an object can perform.
```python
class Car:
    def __init__(self, manufacturer, model, color):
        self.manufacturer = manufacturer
        self.model = model
        self.color = color

    # This is a method
    def start_engine():
        print("Engine has started")
```
* What does `self` mean? It refers to the object.
* `__init__(self, ...)` is the contructor method


## 4 Pillars of OOP
1. **Inheritence**:
Inheritence allows a child class to acquire properties of a parent class. It supports hierarchial classification and promotes code reuse.
```python
class Animal:
    def eat(self):
        print("Animal is eating")
        
class Dog(Animal):
    def bark(self):
        print("Dog is barking")
        
dog = Dog()
dog.eat()   # Inherited from Animal
dog.bark()  # Defined in Dog
```
2. **Polymorphism**:
* Polymorphism means same operation, different behaviour. It allows functions or methods of the same name work differently depending upon the type of object they are acting upon.
**"Many Forms"**.

* Two ways to achieve polymorphism
    1. Inheritance
    ```python
    drom abs import ABC, abstractmethod
    
    class Shape(ABC):
    
        @abstractmethod
        def area(self):
            pass
    
    class Circle(Shape):
        def __init__(self, radius):
            self.radius = radius
    
        def area(self):
            return 3.14 * self.radius * self.radius
    
    class Rectangle(Shape):
        def __init__(self, width, height):
            self.width = width
            self.height = height
    
        def area(self):
            return self.width * self.height
    
    class Pizza(Circle):
        def __init__(self, toppings, radius):
            self.toppings = toppings
            self.radius = radius
    
        def area(self):
            return 3.14 * self.radius * self.radius
    
    shapes = [Circle(6), Rectangle(4,5), Pizza("BBQ Chicken", 12)]
    
    for shape in shapes:
        print(shape.area())
        
    ```
    In this example, circle, rectangle and even pizza all are shapes, and all have area, so they are all shapes in many forms.
    
    2. Duck Typing
    - Another way to achieve polymorphism, besides inheritence
    ```python
    class Animal:
        alive = True
        
    def Dog(Animal):
        def speak("Woof")

    def Cat(Animal):
        def speak("Meow")

    class Car:
        alive = False

        def speak("Honk")
    ```


3. **Encapsulation**:
Encapsulation is the bundling of data (attributes) and methods (functions) within a class. Restricting access to components to control interactions.
```python
class BankAccount:
    def __init__(self, name, balance):
        self.name = name
        self.__balance = balance # Private (Name mangling)

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount

    def get_balance(self):
        return self.__balance


account = BankAccount("Alex", 5000)

account.deposit(1000)

print(account.get_balance()) # 6000
```

4. **Abstraction**:
Abstraction hides the internal implementation details while only exposing the necessary functionality.
```python
class Car:

    def start(self):
        self.__check_battery()
        self.__check_fuel()
        self.__ignite_engine()
        print("Car started!")

    def __check_battery(self):
        print("Checking battery...")

    def __check_fuel(self):
        print("Checking fuel...")

    def __ignite_engine(self):
        print("Igniting engine...")


car = Car()
car.start()
```

## Class vs Intance variables:
* Instance variable belongs to object. Each object has its own Instance variables
* Class variable belongs to class. Each object shares the class variables. These can also be accessed directly from the class.
```python
class Student:

    class_year = 2026 # Class variable
    no_of_students = 0
    
    def __init__(self, name, age):
        self.name = name # Instance variable
        self.age = age # Instance variable
        Student.no_of_students += 1

student1 = Student("Alex", 23)
student2 = Student("Bob", 22)

# Accessing instance varibles (which belong to object)
student1.name # "Alex"
student2.name # "Bob"

# Accessing class variables (either through object or through class name only)
student1.class_year # 2026
student2.class_year # 2026
Student.class_year # 2026 # It is good Practice too.
Student.no_of_students # 2
```

## Some Syntax To Remember

### Abstract class
A class which can't be instantiated on its own. It needs to be inherited by child classes and handle their won implementation

They can contain abstract methods which are declared but have no implementation

To use Abstract classes we need to import ABC and abstractmethod from abc module
```python
from abc import ABC, abstractmethod
```

Here, Vehicle class is an abstract class which is inherited from ABC
```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    pass
```

Abstract methods can be added to Abstract class. For this, we need to use a decorator @abstractmethod
```python
class Vehicle(ABC):

    @abstractmethod
    def start(self):
        pass

    @abstractmethod
    def stop(self):
        pass
```
the start() and stop() are abstract methods which are expected to be implemented by children classes

The children inherits the abstract class and then objects can be instantiated. The inherited method has to implement **ALL** abstract methods.
```python
class Car(Vehicle):
    def start(self):
        print("Car Started")

    def stop(self):
        print("Car Stopped")
```

### `super()` keyword
* It is used to reference the parent class
* Example
```python
class Shape:
    def __init__(self, color, is_filled):
        self.color = color
        self.is_filled = is_filled

class Circle:
    def __init__(color, is_filled, radius):
        super().__init__(color, is_filled)
        self.radius = radius

class Square:
    def __init__(self, color, is_filled, side):
        super().__init__(color, is_filled)
```


### Special/Dunder methods
Using these methods, we can customize the behaviour of the objects.

### 1. `__init__()` – Constructor
Called automatically when an object is created.

```python
class Student:
    def __init__(self, name):
        self.name = name

s1 = Student("John")
```

### 2. `__str__()` – Changes `print()` behaviour
Defines what gets displayed when an object is printed.

```python
class Student:
    def __str__(self):
        return "This is a student object"

s1 = Student()
print(s1)
```

### 3. `__eq__()` – Changes `==` behaviour
Defines how two objects are compared using `==`.

```python
class Student:
    def __init__(self, marks):
        self.marks = marks

    def __eq__(self, other):
        return self.marks == other.marks

s1 = Student(90)
s2 = Student(90)

print(s1 == s2)  # True
```

### 4. `__gt__()` – Changes `>` behaviour
Defines how objects are compared using `>`.

```python
class Student:
    def __init__(self, marks):
        self.marks = marks

    def __gt__(self, other):
        return self.marks > other.marks

s1 = Student(90)
s2 = Student(80)

print(s1 > s2)  # True
```

### 5. `__add__()` – Changes `+` behaviour
Defines what happens when two objects are added using `+`.

```python
class Number:
    def __init__(self, value):
        self.value = value

    def __add__(self, other):
        return self.value + other.value

n1 = Number(10)
n2 = Number(20)

print(n1 + n2)  # 30
```

### 6. `__contains__()` – Changes `in` behaviour
Defines how membership is checked using the `in` operator.

```python
class Team:
    def __init__(self, players):
        self.players = players

    def __contains__(self, player):
        return player in self.players

team = Team(["John", "Alex", "Sam"])

print("John" in team)  # True
```

### 7. `__getitem__()` – Changes indexing behaviour
Defines how elements are accessed using square brackets `[]`.

```python
class Team:
    def __init__(self, players):
        self.players = players

    def __getitem__(self, index):
        return self.players[index]

team = Team(["John", "Alex", "Sam"])

print(team[0])  # John
print(team[1])  # Alex
```

## References
* [Python Object Oriented Programming Full Course - Bro Code](https://www.youtube.com/watch?v=IbMDCwVm63M)
