# Data Structures

Data structures are ways to organize and store data. Python provides several built-in data structures.

## Lists

A list is an ordered, mutable (changeable) collection of items.

### Creating Lists

```python
# Empty list
empty_list = []

# List with integers
numbers = [1, 2, 3, 4, 5]

# List with mixed types
mixed = [1, "hello", 3.14, True]

# List with nested lists
nested = [[1, 2], [3, 4], [5, 6]]
```

### Accessing Elements

```python
numbers = [10, 20, 30, 40, 50]

# Index from the beginning (0-indexed)
print(numbers[0])      # Output: 10
print(numbers[2])      # Output: 30

# Index from the end
print(numbers[-1])     # Output: 50 (last element)
print(numbers[-2])     # Output: 40 (second from last)

# Slicing
print(numbers[1:4])    # Output: [20, 30, 40] (elements at index 1, 2, 3)
print(numbers[:3])     # Output: [10, 20, 30] (first 3 elements)
print(numbers[2:])     # Output: [30, 40, 50] (from index 2 to end)
print(numbers[::2])    # Output: [10, 30, 50] (every 2nd element)
```

### Modifying Lists

```python
numbers = [1, 2, 3, 4, 5]

# Add element at end
numbers.append(6)
print(numbers)         # Output: [1, 2, 3, 4, 5, 6]

# Add element at specific position
numbers.insert(2, 99)
print(numbers)         # Output: [1, 2, 99, 3, 4, 5, 6]

# Remove element by value
numbers.remove(99)
print(numbers)         # Output: [1, 2, 3, 4, 5, 6]

# Remove element by index
removed = numbers.pop(3)
print(removed)         # Output: 4
print(numbers)         # Output: [1, 2, 3, 5, 6]

# Clear entire list
numbers.clear()
print(numbers)         # Output: []
```

### List Methods

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# Sort list
numbers.sort()
print(numbers)         # Output: [1, 1, 2, 3, 4, 5, 6, 9]

# Reverse list
numbers.reverse()
print(numbers)         # Output: [9, 6, 5, 4, 3, 2, 1, 1]

# Find index of element
index = numbers.index(5)
print(index)           # Output: 2

# Count occurrences
count = numbers.count(1)
print(count)           # Output: 2

# Get length
length = len(numbers)
print(length)          # Output: 8
```

### List Comprehension

A concise way to create lists:

```python
# Create list of squares
squares = [x**2 for x in range(1, 6)]
print(squares)         # Output: [1, 4, 9, 16, 25]

# Create list with condition
even_numbers = [x for x in range(1, 11) if x % 2 == 0]
print(even_numbers)    # Output: [2, 4, 6, 8, 10]

# Nested list comprehension
matrix = [[i*j for j in range(1, 4)] for i in range(1, 4)]
# Output: [[1, 2, 3], [2, 4, 6], [3, 6, 9]]
```

## Tuples

A tuple is an ordered, immutable (unchangeable) collection of items.

### Creating Tuples

```python
# Empty tuple
empty_tuple = ()

# Tuple with elements
colors = ("red", "green", "blue")

# Tuple with single element (note the comma!)
single = (42,)

# Tuple with mixed types
mixed = (1, "hello", 3.14, True)

# Tuple unpacking
x, y, z = (10, 20, 30)
print(x, y, z)         # Output: 10 20 30
```

### Accessing and Slicing

```python
colors = ("red", "green", "blue", "yellow")

# Indexing (same as lists)
print(colors[0])       # Output: red
print(colors[-1])      # Output: yellow

# Slicing (same as lists)
print(colors[1:3])     # Output: ('green', 'blue')
```

### Tuple Methods

```python
colors = ("red", "green", "blue", "red")

# Count occurrences
count = colors.count("red")
print(count)           # Output: 2

# Find index
index = colors.index("blue")
print(index)           # Output: 2

# Get length
length = len(colors)
print(length)          # Output: 4
```

### Why Use Tuples?

- **Faster than lists** for large collections
- **Hashable**: Can be used as dictionary keys
- **Protection**: Prevents accidental modification
- **Function returns**: Can return multiple values

```python
# Using tuple as dictionary key
location_map = {
    (0, 0): "origin",
    (1, 1): "diagonal",
    (5, 3): "point"
}
print(location_map[(0, 0)])  # Output: origin

# Multiple return values
def get_user_info():
    return ("John", 25, "john@example.com")

name, age, email = get_user_info()
```

## Dictionaries

A dictionary is an unordered collection of key-value pairs.

### Creating Dictionaries

```python
# Empty dictionary
empty_dict = {}

# Dictionary with data
person = {
    "name": "John",
    "age": 30,
    "city": "New York",
    "email": "john@example.com"
}

# Using dict() constructor
person2 = dict(name="Alice", age=25, city="Los Angeles")
```

### Accessing and Modifying

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Access value
print(person["name"])              # Output: John

# Safe access (returns None if key doesn't exist)
print(person.get("email"))         # Output: None
print(person.get("email", "N/A"))  # Output: N/A

# Add/modify key-value pair
person["email"] = "john@example.com"
person["age"] = 31

# Delete key-value pair
del person["city"]
# or
removed = person.pop("city", None)

# Check if key exists
if "name" in person:
    print("Name exists")
```

### Dictionary Methods

```python
person = {"name": "John", "age": 30, "city": "New York"}

# Get all keys
keys = person.keys()
print(keys)            # Output: dict_keys(['name', 'age', 'city'])

# Get all values
values = person.values()
print(values)          # Output: dict_values(['John', 30, 'New York'])

# Get all key-value pairs
items = person.items()
print(items)           # Output: dict_items([('name', 'John'), ('age', 30), ('city', 'New York')])

# Iterate through dictionary
for key, value in person.items():
    print(f"{key}: {value}")

# Update dictionary
person.update({"age": 31, "email": "john@example.com"})

# Clear dictionary
person.clear()

# Get and remove last item
last_key, last_value = person.popitem()
```

### Dictionary Comprehension

```python
# Create dictionary from range
squares = {x: x**2 for x in range(1, 6)}
print(squares)         # Output: {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```

## Sets

A set is an unordered collection of unique items.

### Creating Sets

```python
# Empty set (use set(), not {})
empty_set = set()

# Set with elements
colors = {"red", "green", "blue"}

# Set from list (removes duplicates)
numbers = set([1, 2, 2, 3, 3, 3, 4])
print(numbers)         # Output: {1, 2, 3, 4}

# Set comprehension
squares = {x**2 for x in range(1, 6)}
print(squares)         # Output: {1, 4, 9, 16, 25}
```

### Set Operations

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

# Union
union = set1 | set2
print(union)           # Output: {1, 2, 3, 4, 5, 6}

# Intersection
intersection = set1 & set2
print(intersection)    # Output: {3, 4}

# Difference
difference = set1 - set2
print(difference)      # Output: {1, 2}

# Symmetric difference
sym_diff = set1 ^ set2
print(sym_diff)        # Output: {1, 2, 5, 6}
```

### Set Methods

```python
colors = {"red", "green", "blue"}

# Add element
colors.add("yellow")
print(colors)          # Output: {'red', 'green', 'blue', 'yellow'}

# Remove element
colors.remove("blue")  # Raises KeyError if not found
colors.discard("blue") # No error if not found

# Pop arbitrary element
color = colors.pop()

# Clear all elements
colors.clear()

# Check membership
if "red" in colors:
    print("Red exists")
```

## String Data Type

Strings are immutable sequences of characters.

### Creating Strings

```python
# Single quotes
str1 = 'Hello'

# Double quotes
str2 = "World"

# Triple quotes (multiline)
str3 = """This is a
multiline
string"""

# String concatenation
message = str1 + " " + str2
print(message)         # Output: Hello World

# String repetition
repeated = "Ha" * 3
print(repeated)        # Output: HaHaHa
```

### String Methods

```python
text = "Hello World"

# Change case
print(text.lower())                # Output: hello world
print(text.upper())                # Output: HELLO WORLD
print(text.capitalize())           # Output: Hello world

# Find substring
index = text.find("World")
print(index)                       # Output: 6

# Replace substring
new_text = text.replace("World", "Python")
print(new_text)                    # Output: Hello Python

# Split string
words = text.split()
print(words)                       # Output: ['Hello', 'World']

# Join list into string
joined = "-".join(words)
print(joined)                      # Output: Hello-World

# Strip whitespace
text = "  Hello  "
print(text.strip())                # Output: Hello
```

### String Formatting

```python
name = "John"
age = 30

# f-string (Python 3.6+) - preferred
message = f"My name is {name} and I'm {age} years old"
print(message)

# .format() method
message = "My name is {} and I'm {} years old".format(name, age)
print(message)

# % formatting (older style)
message = "My name is %s and I'm %d years old" % (name, age)
print(message)

# f-string with expressions
result = f"Sum: {10 + 20}"
print(result)          # Output: Sum: 30
```
