# Functions

Functions are reusable blocks of code that perform specific tasks.

## Function Basics

### Defining and Calling Functions

```python
# Simple function
def greet():
    print("Hello!")

greet()                    # Output: Hello!

# Function with parameters
def add(a, b):
    return a + b

result = add(5, 3)
print(result)              # Output: 8
```

### Parameters and Arguments

```python
# Positional parameters
def introduce(name, age):
    print(f"Name: {name}, Age: {age}")

introduce("John", 30)      # Output: Name: John, Age: 30

# Default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Alice"))              # Output: Hello, Alice!
print(greet("Bob", "Hi"))          # Output: Hi, Bob!

# Keyword arguments
print(greet(name="Charlie", greeting="Hey"))  # Output: Hey, Charlie!
```

### Variable-Length Arguments

```python
# *args - variable number of positional arguments
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3))          # Output: 6
print(sum_all(1, 2, 3, 4, 5))    # Output: 15

# **kwargs - variable number of keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="John", age=30, city="NYC")
# Output:
# name: John
# age: 30
# city: NYC

# Combined
def full_example(name, *args, **kwargs):
    print(f"Name: {name}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

full_example("John", 1, 2, 3, age=30, city="NYC")
```

## Function Return Values

### Return Types

```python
# Single return value
def get_age():
    return 30

print(get_age())           # Output: 30

# Multiple return values
def get_coordinates():
    return (10, 20, 30)

x, y, z = get_coordinates()
print(x, y, z)             # Output: 10 20 30

# No return value (returns None)
def print_message():
    print("Hello")
    # No return statement

result = print_message()
print(result)              # Output: None
```

### Early Returns

```python
def check_age(age):
    if age < 0:
        return "Invalid age"
    
    if age < 18:
        return "Minor"
    
    return "Adult"

print(check_age(-5))       # Output: Invalid age
print(check_age(15))       # Output: Minor
print(check_age(25))       # Output: Adult
```

## Docstrings and Type Hints

### Docstrings

```python
def calculate_area(radius):
    """
    Calculate the area of a circle.
    
    Args:
        radius (float): The radius of the circle
    
    Returns:
        float: The area of the circle
    
    Example:
        >>> calculate_area(5)
        78.5
    """
    return 3.14 * radius ** 2

# Access docstring
print(calculate_area.__doc__)
help(calculate_area)
```

### Type Hints

```python
def add(a: int, b: int) -> int:
    """Add two integers and return the result."""
    return a + b

def greet(name: str) -> str:
    """Return a greeting message."""
    return f"Hello, {name}!"

# Type hints don't enforce types but help with documentation
print(add(5, 3))           # Output: 8
print(greet("Alice"))      # Output: Hello, Alice!
```

## Scope and Lifetime

### Local and Global Scope

```python
global_var = 10

def example():
    local_var = 20
    print(f"Global: {global_var}")   # Accessible
    print(f"Local: {local_var}")     # Accessible

example()

# print(local_var)         # NameError: local_var not defined
# Accessible outside function
print(global_var)          # Output: 10
```

### Global Keyword

```python
count = 0

def increment():
    global count
    count += 1

print(count)               # Output: 0
increment()
print(count)               # Output: 1
```

### Nonlocal Keyword

```python
def outer():
    x = 10
    
    def inner():
        nonlocal x
        x = 20
    
    print(f"Before: {x}")   # Output: Before: 10
    inner()
    print(f"After: {x}")    # Output: After: 20

outer()
```

## Function Decorators

### Basic Decorators

```python
def my_decorator(func):
    def wrapper():
        print("Something before the function")
        func()
        print("Something after the function")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Something before the function
# Hello!
# Something after the function
```

### Decorators with Arguments

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        return func(*args, **kwargs)
    return wrapper

@my_decorator
def add(a, b):
    return a + b

result = add(5, 3)         # Output: Calling add with args=(5, 3), kwargs={}
print(result)              # Output: 8
```

### Common Decorators

```python
from functools import wraps

# Timing decorator
import time

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(2)
    return "Done"

slow_function()
```

## Lambda Functions

Lambda functions are small, anonymous functions.

### Basic Lambda

```python
# Lambda function
add = lambda x, y: x + y
print(add(5, 3))           # Output: 8

# Lambda with single argument
square = lambda x: x ** 2
print(square(5))           # Output: 25

# Lambda with default argument
greet = lambda name="Guest": f"Hello, {name}!"
print(greet())             # Output: Hello, Guest!
print(greet("Alice"))      # Output: Hello, Alice!
```

### Lambda with Built-in Functions

```python
# With map
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)             # Output: [1, 4, 9, 16, 25]

# With filter
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)               # Output: [2, 4, 6, 8, 10]

# With sorted
students = [("Alice", 85), ("Bob", 75), ("Charlie", 90)]
sorted_by_grade = sorted(students, key=lambda x: x[1])
print(sorted_by_grade)     # Output: [('Bob', 75), ('Alice', 85), ('Charlie', 90)]
```

## Closures

A closure is a function that has access to variables from its enclosing scope.

```python
def outer(x):
    def inner(y):
        return x + y
    return inner

add_5 = outer(5)
print(add_5(3))             # Output: 8
print(add_5(10))            # Output: 15

# Practical example - counter
def make_counter():
    count = 0
    
    def counter():
        nonlocal count
        count += 1
        return count
    
    return counter

counter1 = make_counter()
counter2 = make_counter()

print(counter1())          # Output: 1
print(counter1())          # Output: 2
print(counter2())          # Output: 1 (separate counter)
```

## Generators and Yield

Generators are functions that return values one at a time using yield.

```python
# Basic generator
def countdown(n):
    while n > 0:
        yield n
        n -= 1

for num in countdown(5):
    print(num)             # Output: 5 4 3 2 1

# Generator expression
squares = (x ** 2 for x in range(1, 6))
print(next(squares))       # Output: 1
print(next(squares))       # Output: 4
```
