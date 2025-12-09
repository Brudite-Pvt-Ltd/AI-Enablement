# Common Issues and Solutions

This section covers common Python errors and how to solve them.

## Issue 1: Modifying List While Iterating

**Cause**: Unexpected behavior when list size changes during iteration.

**Problem**:
```python
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    if num % 2 == 0:
        numbers.remove(num)  # Skips elements!
print(numbers)  # Output: [1, 3, 5]
```

**Solution**:
```python
numbers = [1, 2, 3, 4, 5]

# Iterate over a copy
for num in numbers[:]:  # Create copy with slicing
    if num % 2 == 0:
        numbers.remove(num)
print(numbers)  # Output: [1, 3, 5]

# Or use list comprehension
numbers = [1, 2, 3, 4, 5]
numbers = [num for num in numbers if num % 2 != 0]
print(numbers)  # Output: [1, 3, 5]
```

---

## Issue 2: Dictionary Key Error

**Cause**: Accessing a key that doesn't exist in dictionary.

**Problem**:
```python
person = {"name": "John", "age": 30}
print(person["email"])  # KeyError: 'email'
```

**Solution**:
```python
person = {"name": "John", "age": 30}

# Use get() method
print(person.get("email"))              # Output: None
print(person.get("email", "N/A"))       # Output: N/A

# Use in operator
if "email" in person:
    print(person["email"])
else:
    print("Email not found")
```

---

## Issue 3: Mutable Default Arguments

**Cause**: Using mutable object as default argument causes unexpected sharing.

**Problem**:
```python
def add_item(item, items=[]):
    items.append(item)
    return items

result1 = add_item("apple")
result2 = add_item("banana")
print(result2)  # Output: ['apple', 'banana'] (unexpected!)
```

**Solution**:
```python
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

result1 = add_item("apple")
result2 = add_item("banana")
print(result2)  # Output: ['banana'] (correct!)
```

---

## Issue 4: UnboundLocalError with Global Variable

**Cause**: Referencing a variable before assigning it in the same scope.

**Problem**:
```python
count = 0

def increment():
    print(count)  # Error: UnboundLocalError
    count += 1    # Python sees this, so count is local

increment()
```

**Solution**:
```python
count = 0

def increment():
    global count  # Declare global before using
    print(count)
    count += 1

increment()  # Output: 0
print(count)  # Output: 1
```

---

## Issue 5: FileNotFoundError When Reading Files

**Cause**: File path is incorrect or file doesn't exist.

**Problem**:
```python
with open("data.txt", "r") as file:  # FileNotFoundError if file doesn't exist
    content = file.read()
```

**Solution**:
```python
import os

file_path = "data.txt"

# Check if file exists first
if os.path.exists(file_path):
    with open(file_path, "r") as file:
        content = file.read()
else:
    print(f"File {file_path} not found")

# Or use try-except
try:
    with open(file_path, "r") as file:
        content = file.read()
except FileNotFoundError:
    print(f"File {file_path} not found")
```

---

## Issue 6: Comparing with None Incorrectly

**Cause**: Using `==` instead of `is` to compare with None.

**Problem**:
```python
value = None
if value == None:      # Works but not Pythonic
    print("Value is None")
```

**Solution**:
```python
value = None
if value is None:      # Correct way
    print("Value is None")

if value is not None:  # Check if not None
    print("Value exists")
```

---

## Issue 7: Shallow Copy of Nested Structures

**Cause**: Shallow copy doesn't copy nested objects.

**Problem**:
```python
original = [[1, 2], [3, 4]]
copy = original.copy()  # Shallow copy
copy[0].append(99)
print(original)  # Output: [[1, 2, 99], [3, 4]] (modified!)
```

**Solution**:
```python
import copy

original = [[1, 2], [3, 4]]
deep = copy.deepcopy(original)  # Deep copy
deep[0].append(99)
print(original)  # Output: [[1, 2], [3, 4]] (unchanged)
```

---

## Issue 8: Class Attribute vs Instance Attribute Confusion

**Cause**: Confusing class variables with instance variables.

**Problem**:
```python
class Dog:
    tricks = []  # Class variable
    
    def __init__(self, name):
        self.name = name
    
    def add_trick(self, trick):
        self.tricks.append(trick)  # Modifies class variable!

dog1 = Dog("Buddy")
dog2 = Dog("Max")
dog1.add_trick("sit")
dog2.add_trick("stay")
print(dog1.tricks)  # Output: ['sit', 'stay'] (shared!)
```

**Solution**:
```python
class Dog:
    def __init__(self, name):
        self.name = name
        self.tricks = []  # Instance variable
    
    def add_trick(self, trick):
        self.tricks.append(trick)

dog1 = Dog("Buddy")
dog2 = Dog("Max")
dog1.add_trick("sit")
dog2.add_trick("stay")
print(dog1.tricks)  # Output: ['sit'] (separate)
print(dog2.tricks)  # Output: ['stay'] (separate)
```

---

## Issue 9: String Index Out of Range

**Cause**: Accessing an index that doesn't exist in a string or list.

**Problem**:
```python
text = "hello"
print(text[10])  # IndexError: string index out of range
```

**Solution**:
```python
text = "hello"

# Check length before accessing
if len(text) > 10:
    print(text[10])
else:
    print("Index out of range")

# Use slicing (doesn't raise error)
print(text[10:])  # Output: "" (empty string)

# Use get for lists with default
my_list = [1, 2, 3]
value = my_list[10] if len(my_list) > 10 else None
```

---

## Issue 10: Division by Zero

**Cause**: Attempting to divide by zero.

**Problem**:
```python
result = 10 / 0  # ZeroDivisionError: division by zero
```

**Solution**:
```python
divisor = 0

# Check before dividing
if divisor != 0:
    result = 10 / divisor
else:
    print("Cannot divide by zero")

# Or use try-except
try:
    result = 10 / divisor
except ZeroDivisionError:
    print("Cannot divide by zero")
    result = None
```

---

## Issue 11: Type Mismatch in Operations

**Cause**: Trying to perform operations on incompatible types.

**Problem**:
```python
result = "10" + 5  # TypeError: can only concatenate str (not "int") to str
```

**Solution**:
```python
# Convert to same type
result = int("10") + 5  # Output: 15
result = "10" + str(5)  # Output: "105"

# Type checking before operation
value1 = "10"
value2 = 5

if isinstance(value1, int) and isinstance(value2, int):
    result = value1 + value2
else:
    print("Type mismatch")
```

---

## Issue 12: Infinite Loop

**Cause**: Loop condition is always true.

**Problem**:
```python
while True:
    print("This will print forever")
    # Missing break or condition update
```

**Solution**:
```python
# Use break statement
count = 0
while True:
    print(count)
    count += 1
    if count >= 5:
        break

# Or use proper condition
count = 0
while count < 5:
    print(count)
    count += 1

# For loops are safer
for i in range(5):
    print(i)
```

---

## Issue 13: Off-by-One Error with Range

**Cause**: Misunderstanding how range works (upper limit is exclusive).

**Problem**:
```python
# range(1, 5) gives [1, 2, 3, 4], not [1, 2, 3, 4, 5]
for i in range(1, 5):
    print(i)  # Output: 1 2 3 4 (not 5!)
```

**Solution**:
```python
# If you need up to and including 5, use range(1, 6)
for i in range(1, 6):
    print(i)  # Output: 1 2 3 4 5

# Remember: range(start, stop, step)
# - start is inclusive
# - stop is exclusive
# - step is the increment

# Starting from 0
for i in range(5):  # 0, 1, 2, 3, 4
    print(i)

# With step
for i in range(0, 10, 2):  # 0, 2, 4, 6, 8
    print(i)
```

---

## Issue 14: Import Errors

**Cause**: Module not found or circular imports.

**Problem**:
```python
import nonexistent_module  # ModuleNotFoundError
```

**Solution**:
```python
# Check if module exists
try:
    import nonexistent_module
except ModuleNotFoundError:
    print("Module not found. Install it first.")

# Avoid circular imports
# In module_a.py, avoid importing from module_b if module_b imports from module_a

# Use conditional imports
if some_condition:
    import optional_module
```

---

## Issue 15: Exception Handling Best Practices

**Problem** - Too Broad Exception Catching:
```python
try:
    result = 10 / 0
except:  # Catches everything!
    print("Error occurred")
```

**Solution** - Catch Specific Exceptions:
```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
except ValueError:
    print("Invalid value")
except Exception as e:
    print(f"Unexpected error: {e}")
finally:
    print("Cleanup code")
```
