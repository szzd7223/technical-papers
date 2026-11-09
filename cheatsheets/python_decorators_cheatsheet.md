# Decorators in Python

## Introduction
* Decorators extend the behaviour of another function without modifying the base function.
* Decorators are nothing but functions, which take another function (base function) as an argument.

## Simple Code Sample
```python
def greet(func):
    def modified_func():
        print("Good Morning")
        func()
        print("Thanks for using the program")

    return modified_func


@greet
def hello():
    print("Hello")


hello()

```
- In this sample, the `greet` function is the decorator which takes `hello` function and modifies it to print a sentence before hello and after hello.
- The `@greet` syntax is used to apply decorator to the function.

## `*args` and `**kwargs`
- `*args` allows a function to accept any number of positional arguments. They are stored in a **Tuple**.
- `**kwargs` allows a function to accept any number of keyword arguments. They are stored in a **Dictionary**.
- Example:
```python
def order_pizza(size, *args, **kwargs):
    print(f"Ordered a {size} inch pizza with the following toppings:")

    for topping in args:
        print(f"- {topping}")

    print("\nDetails of the order:")
    for key, value in kwargs.items():
        print(f"- {key}:{value}")


order_pizza(
    12, "BBQ Chicken", "Olives", "Jalepenos", Delivery=True, total_amount=280, gst=18
)

```
```text
Ordered a 12 inch pizza with the following toppings:
- BBQ Chicken
- Olives
- Jalepenos

Details of the order:
- Delivery:True
- total_amount:280
- gst:18
```

## What if Base Function Has Arguments
- We use *args and **kwargs to support that.
```python
def add_sprinkles(func):
    def wrapper(*args, **kwargs):
        print("*You add sprinkles*")
        func(*args, **kwargs)

    return wrapper


@add_sprinkles
def get_ice_cream(flavor):
    print(f"Here is your {flavor} ice cream.")


get_ice_cream("vanilla")

```

## Commonly used decorators in OOP
- @abstractmethod
- @staticmethod
