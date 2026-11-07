# Python Strings
Strings are sequence of unique characters written inside quotes. It can include letters, numbers, symbols and spaces.

## Create Strings
```python
word = "Hello"

# Multi line Strings
sentence = """The quick brown fox,
jumped over the wall"""
```

## Accessing characters in a string
```python
s = "Hello"

s[0] # first charater "H"
s[-1] # last charater "o"
s[1] # second character "e"
s[-2] # second last character "l"
```

## String slicing
```python
s = "ABCD"

print(s[1:3]) # BC
print(s[:]) # ABCD
print(s[1:]) # BCD
print(s[:3]) # ABC
```

## Looping Through Strings
```python
s = "abcde"

for char in s:
    print(char)

for i in range(len(s)):
    print(s[i])
```

##  String Immutability
* Strings are immutable, their values cannot be changed after creation.

```python
s ="Hello"
s[2] = "o"
print(s) # You might expect "Hollo" but it would throw an error because strings are immutable.
```
```bash
ERROR!
Traceback (most recent call last):
  File "<main.py>", line 2, in <module>
TypeError: 'str' object does not support item assignment
```
* Any modification to the string creates a new string instead of altering the original one
```python
s = "aBCD"
print(s) # aBCD
s = "A" + s[1:] # This creates a new string and assigns it to variable s overwriting the previous value
print(s) # ABCD
```

## Common String Methods
Some common methods for string manipulation

1. **len():**
    - Returns total number of characters in a string
      ```python
      s = "Hello"
      len(s) # Returns 5
      ```
2. **upper() and lower():**
    - upper() converts all characters to uppercase
    - lower() converts all charaters to lowercase
      ```python
      s = "heLlO"
      s.upper() # HELLO
      s.lower() # hello
      ```
3. **strip() and replace():**
    - strip() removes whitespaces
    - replace() replaces all occurances of a specified substring with another
      ```python
      s = "   Abc                  "
      s.strip() # Returns "Abc"

      s = "Java is nice"
      s.replace("Java", "Python") # Returns "Python is nice"
      ```
4. **split():**
    - Returns a list of words in the string seperated by " " by default
    - Can be used to split on basis of other charaters also
      ```python
      s = "This sentence has some words"
      s.split() # Returns ['This', 'sentence', 'has', 'some', 'words']
      
      vitamins = "Vitamin A, Vitamin C, Vitamin D"
      vitamins.split(", ") # Returns ["Vitamin A", "Vitamin C", "Vitamin D"]
      ```
5. **Some addtional methods:**

| Method         | Description                                                                  | Example                     | Output          |
| -------------- | ---------------------------------------------------------------------------- | --------------------------- | --------------- |
| `capitalize()` | Capitalizes the first character                                              | `"hello".capitalize()`      | `"Hello"`       |
| `title()`      | Capitalizes the first character of each word                                 | `"hello world".title()`     | `"Hello World"` |
| `startswith()` | Checks whether a string starts with a given substring                        | `"Python".startswith("Py")` | `True`          |
| `endswith()`   | Checks whether a string ends with a given substring                          | `"Python".endswith("on")`   | `True`          |
| `find()`       | Returns the index of the first occurrence of a substring (`-1` if not found) | `"hello".find("l")`         | `2`             |
| `count()`      | Returns the number of occurrences of a substring                             | `"hello".count("l")`        | `2`             |
| `join()`       | Joins elements of an iterable into a string                                  | `"-".join(["a", "b", "c"])` | `"a-b-c"`       |
| `isdigit()`    | Checks whether all characters are digits                                     | `"123".isdigit()`           | `True`          |
| `isalpha()`    | Checks whether all characters are alphabetic                                 | `"Hello".isalpha()`         | `True`          |
| `isalnum()`    | Checks whether all characters are letters or digits                          | `"abc123".isalnum()`        | `True`          |

## Concatenating and repeating strings
* Concatenation - Strings can be combined using "+" operator.
```python
s1 = "Hello"
s2 = "World"
print(s1 + " " + s2) # Prints "Hello World"
```
* Repetition - A string can be repeated multiple times using "*".
```python
s = "Hello "
print(s * 3) # Print "Hello Hello Hello"
```

## Formatting strings
* Using f-strings:
```python
name = "Jake"
age = 22
print(f"Name: {name}, Age: {age}") # Prints "Name: Jake, Age: 22"
```

* Using format():
```python
s = "My name is {} and I am {} years old.".format( "Emily", 22)
print(s) # Prints "My name is Emily and I am 22 years old."
```

## String Membership
* `in` keyword is used to check whether a substring exists inside a string. It returns True if string is found, otherwise it returns False.
```python
s = "Hi, nice to meet you."
print("nice" in s) # Prints True
print("Hello" in s) # Prints False
```

## Tricks
* Reverse a string
```python
s = "Hello"
rev = s[::-1]
print(rev) # Prints "olleH"
```

## References:
[Python String - GeeksForGeeks](https://www.geeksforgeeks.org/python/python-string/)
