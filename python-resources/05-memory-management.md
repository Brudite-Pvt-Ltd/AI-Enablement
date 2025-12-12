# Memory Management

Understanding how Python manages memory is important for writing efficient code.

## Variables and References

### How Variables Work

```python
# Variables are references to objects
x = 10
y = x         # y references the same integer object

print(id(x))  # Memory address
print(id(y))  # Same address as x

x = 20        # x now references a different integer
print(id(x))  # Different address
print(id(y))  # Still the original address (y is still 10)
```

### Mutable vs Immutable Objects

```python
# Immutable objects (int, float, str, tuple)
x = "hello"
y = x
x = "world"
print(y)      # Output: hello (y unchanged)

# Mutable objects (list, dict, set)
list1 = [1, 2, 3]
list2 = list1
list1.append(4)
print(list2)  # Output: [1, 2, 3, 4] (list2 also changed!)
```

## Garbage Collection

Python automatically manages memory through garbage collection.

### Reference Counting

```python
import sys

# Create an object
x = []
print(sys.getrefcount(x))  # Reference count

# Create another reference
y = x
print(sys.getrefcount(x))  # Reference count increased

# Delete reference
del y
print(sys.getrefcount(x))  # Reference count decreased

# Object is deleted when reference count reaches 0
del x
# x no longer exists
```

### Circular References

```python
import gc

class Node:
    def __init__(self, value):
        self.value = value
        self.next = None

# Create circular reference
node1 = Node(1)
node2 = Node(2)
node1.next = node2
node2.next = node1  # Circular reference!

# Objects still exist even after del
del node1
del node2

# Manual garbage collection
gc.collect()  # Cleans up circular references
```

## Shallow Copy vs Deep Copy

### Shallow Copy

```python
original = [[1, 2], [3, 4]]
shallow = original.copy()  # or shallow = original[:]

# Modify nested list
shallow[0].append(99)
print(original)  # [[1, 2, 99], [3, 4]] (modified!)
```

### Deep Copy

```python
import copy

original = [[1, 2], [3, 4]]
deep = copy.deepcopy(original)

# Modify nested list
deep[0].append(99)
print(original)  # [[1, 2], [3, 4]] (unchanged)
```

## Memory Efficiency Tips

### Using Generators Instead of Lists

```python
# Uses more memory (creates entire list)
squares_list = [x**2 for x in range(1000000)]

# Uses less memory (generates values on demand)
squares_gen = (x**2 for x in range(1000000))

# Process one at a time
for square in squares_gen:
    print(square)
```

### Using Slots

```python
# Without slots - uses __dict__ dictionary
class PersonNormal:
    def __init__(self, name, age):
        self.name = name
        self.age = age

# With slots - fixed attributes, less memory
class PersonSlots:
    __slots__ = ['name', 'age']
    
    def __init__(self, name, age):
        self.name = name
        self.age = age

import sys
person_normal = PersonNormal("John", 30)
person_slots = PersonSlots("John", 30)

print(sys.getsizeof(person_normal.__dict__))  # Dict memory overhead
# PersonSlots uses less memory
```

### String Interning

```python
# Python interns small strings and integers
a = "hello"
b = "hello"
print(a is b)  # Output: True (same object)

# But not all strings
a = "hello world"
b = "hello world"
print(a is b)  # Output: False (different objects)

# Manual interning
import sys
a = sys.intern("hello world")
b = sys.intern("hello world")
print(a is b)  # Output: True
```

## Memory Profiling

### Using Memory Profiler

```python
# Install: pip install memory-profiler

from memory_profiler import profile

@profile
def my_function():
    a = [i for i in range(100000)]
    b = [i**2 for i in range(100000)]
    return sum(b)

# Run with: python -m memory_profiler script.py
```

### Using Tracemalloc

```python
import tracemalloc

tracemalloc.start()

# Your code here
x = [i**2 for i in range(100000)]

current, peak = tracemalloc.get_traced_memory()
print(f"Current memory: {current / 1024 / 1024:.2f} MB")
print(f"Peak memory: {peak / 1024 / 1024:.2f} MB")

tracemalloc.stop()
```

## Performance Optimization

### List vs Tuple

```python
# Tuples are faster and use less memory
import time

# List creation
start = time.time()
my_list = [i for i in range(1000000)]
end = time.time()
print(f"List creation: {end - start}")

# Tuple creation
start = time.time()
my_tuple = tuple(i for i in range(1000000))
end = time.time()
print(f"Tuple creation: {end - start}")
```

### Using __slots__ for Classes

```python
import sys

# Class without slots
class RegularClass:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Class with slots
class SlottedClass:
    __slots__ = ['x', 'y']
    
    def __init__(self, x, y):
        self.x = x
        self.y = y

regular = RegularClass(1, 2)
slotted = SlottedClass(1, 2)

print(f"Regular size: {sys.getsizeof(regular)}")
print(f"Slotted size: {sys.getsizeof(slotted)}")
```

### Avoiding Global Variables

```python
# Slow - global lookup
global_list = []

def slow_append():
    global_list.append(1)

# Fast - local reference
def fast_append():
    local_list = []
    local_list.append(1)
    return local_list

# Even faster - pass as parameter
def fastest_append(lst):
    lst.append(1)
    return lst
```

## Context Managers and Resource Cleanup

### Using With Statement

```python
# Properly handles resource cleanup
with open("file.txt", "r") as file:
    content = file.read()
# File is automatically closed

# Without with statement (not recommended)
file = open("file.txt", "r")
try:
    content = file.read()
finally:
    file.close()
```

### Creating Custom Context Managers

```python
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.time()
    try:
        yield
    finally:
        end = time.time()
        print(f"Elapsed: {end - start} seconds")

# Usage
with timer():
    sum(range(1000000))
```

### Context Manager Class

```python
class MyResourceManager:
    def __init__(self, name):
        self.name = name
    
    def __enter__(self):
        print(f"Acquiring {self.name}")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"Releasing {self.name}")
        return False

# Usage
with MyResourceManager("database") as resource:
    print(f"Using {resource.name}")
```
