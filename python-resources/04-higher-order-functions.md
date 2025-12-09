# Higher-Order Functions

Higher-order functions are functions that take other functions as arguments or return functions.

## Map Function

The `map()` function applies a function to every item of an iterable.

### Basic Map

```python
# Apply function to each element
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)             # Output: [1, 4, 9, 16, 25]

# Using regular function
def double(x):
    return x * 2

doubled = list(map(double, numbers))
print(doubled)             # Output: [2, 4, 6, 8, 10]
```

### Map with Multiple Iterables

```python
def add(x, y):
    return x + y

list1 = [1, 2, 3]
list2 = [10, 20, 30]

result = list(map(add, list1, list2))
print(result)              # Output: [11, 22, 33]

# With lambda
result = list(map(lambda x, y: x * y, list1, list2))
print(result)              # Output: [10, 40, 90]
```

## Filter Function

The `filter()` function filters items from an iterable based on a condition.

### Basic Filter

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filter even numbers
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)               # Output: [2, 4, 6, 8, 10]

# Using regular function
def is_positive(x):
    return x > 0

numbers_with_negative = [-5, -2, 0, 3, 7, -1, 9]
positive = list(filter(is_positive, numbers_with_negative))
print(positive)            # Output: [3, 7, 9]
```

### Filter with None

```python
# Filter None removes all falsy values
values = [0, 1, 2, 3, False, True, None, "", "hello"]
truthy = list(filter(None, values))
print(truthy)              # Output: [1, 2, 3, True, 'hello']
```

## Reduce Function

The `reduce()` function cumulatively applies a function to items of an iterable.

### Basic Reduce

```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]

# Sum all numbers
total = reduce(lambda x, y: x + y, numbers)
print(total)               # Output: 15

# Multiply all numbers
product = reduce(lambda x, y: x * y, numbers)
print(product)             # Output: 120

# With initial value
total = reduce(lambda x, y: x + y, numbers, 100)
print(total)               # Output: 115 (100 + 1 + 2 + 3 + 4 + 5)
```

### Complex Reduce Operations

```python
from functools import reduce

# Build a string
words = ["Python", "is", "awesome"]
sentence = reduce(lambda x, y: x + " " + y, words)
print(sentence)            # Output: Python is awesome

# Find maximum using reduce
numbers = [3, 7, 2, 9, 1]
maximum = reduce(lambda x, y: x if x > y else y, numbers)
print(maximum)             # Output: 9
```

## Sorted with Key Function

The `sorted()` function can take a key function for custom sorting.

### Basic Sorting

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
sorted_numbers = sorted(numbers)
print(sorted_numbers)      # Output: [1, 1, 2, 3, 4, 5, 6, 9]

# Reverse sort
reverse_sorted = sorted(numbers, reverse=True)
print(reverse_sorted)      # Output: [9, 6, 5, 4, 3, 2, 1, 1]
```

### Sorting with Key Function

```python
# Sort strings by length
words = ["python", "is", "awesome", "really"]
sorted_by_length = sorted(words, key=len)
print(sorted_by_length)    # Output: ['is', 'python', 'really', 'awesome']

# Sort by second element
students = [("Alice", 85), ("Bob", 75), ("Charlie", 90)]
sorted_by_grade = sorted(students, key=lambda x: x[1])
print(sorted_by_grade)     # Output: [('Bob', 75), ('Alice', 85), ('Charlie', 90)]

# Sort dictionary by value
scores = {"Alice": 85, "Bob": 75, "Charlie": 90}
sorted_scores = sorted(scores.items(), key=lambda x: x[1])
print(sorted_scores)       # Output: [('Bob', 75), ('Alice', 85), ('Charlie', 90)]
```

## Function Composition

Combining multiple functions together.

### Manual Composition

```python
def add(x, y):
    return x + y

def multiply(x, y):
    return x * y

def subtract(x, y):
    return x - y

# Compose functions manually
result = multiply(add(5, 3), subtract(10, 2))  # multiply(8, 8)
print(result)              # Output: 64
```

### Composition with Higher-Order Functions

```python
# Compose functions
def compose(f, g):
    return lambda x: f(g(x))

def add_five(x):
    return x + 5

def multiply_two(x):
    return x * 2

# Create composed function
add_then_multiply = compose(multiply_two, add_five)
print(add_then_multiply(3))  # Output: 16 (multiply_two(add_five(3)))

# Create reverse order
multiply_then_add = compose(add_five, multiply_two)
print(multiply_then_add(3))  # Output: 11 (add_five(multiply_two(3)))
```

## Partial Functions

Creating a new function by fixing some arguments.

```python
from functools import partial

def add(x, y):
    return x + y

# Create a new function that always adds 10
add_10 = partial(add, 10)
print(add_10(5))           # Output: 15

# Practical example
def power(base, exponent):
    return base ** exponent

square = partial(power, exponent=2)
cube = partial(power, exponent=3)

print(square(5))           # Output: 25
print(cube(5))             # Output: 125
```

## Practical Examples

### Processing Data

```python
from functools import reduce

# Data processing pipeline
data = [1, 2, 3, 4, 5]

# Filter, map, and reduce
result = reduce(
    lambda x, y: x + y,
    map(lambda x: x ** 2, filter(lambda x: x % 2 == 0, data))
)
print(result)              # Output: 20 (4 + 16)

# More readable with intermediate variables
evens = filter(lambda x: x % 2 == 0, data)      # [2, 4]
squared = map(lambda x: x ** 2, evens)          # [4, 16]
total = reduce(lambda x, y: x + y, squared)     # 20
```

### Custom Sorting

```python
# Sort complex objects
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def __repr__(self):
        return f"Person({self.name}, {self.age})"

people = [
    Person("Alice", 30),
    Person("Bob", 25),
    Person("Charlie", 35)
]

# Sort by age
sorted_by_age = sorted(people, key=lambda p: p.age)
print(sorted_by_age)       # [Person(Bob, 25), Person(Alice, 30), Person(Charlie, 35)]

# Sort by name
sorted_by_name = sorted(people, key=lambda p: p.name)
print(sorted_by_name)      # [Person(Alice, 30), Person(Bob, 25), Person(Charlie, 35)]
```

### Function Decorators with Higher-Order Functions

```python
# Decorator that caches results
def memoize(func):
    cache = {}
    
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))       # Fast because results are cached
```
