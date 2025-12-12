# Object-Oriented Programming (OOP)

Object-oriented programming is a paradigm that uses objects and classes to structure code.

## Classes and Objects

A class is a blueprint for creating objects. An object is an instance of a class.

### Basic Class Definition

```python
class Dog:
    """A class representing a dog."""
    
    # Constructor
    def __init__(self, name, age):
        # Instance variables
        self.name = name
        self.age = age
    
    # Method
    def bark(self):
        return f"{self.name} says Woof!"
    
    def get_age(self):
        return f"{self.name} is {self.age} years old"

# Creating an object (instance)
dog1 = Dog("Buddy", 5)
dog2 = Dog("Max", 3)

# Accessing attributes
print(dog1.name)           # Output: Buddy
print(dog1.age)            # Output: 5

# Calling methods
print(dog1.bark())         # Output: Buddy says Woof!
print(dog1.get_age())      # Output: Buddy is 5 years old
```

## Attributes and Methods

### Instance vs Class Variables

```python
class Counter:
    class_count = 0  # Class variable (shared)
    
    def __init__(self, value):
        self.value = value  # Instance variable
        Counter.class_count += 1
    
    def increment(self):
        self.value += 1

c1 = Counter(10)
c2 = Counter(20)

print(c1.value)            # Output: 10
print(c2.value)            # Output: 20
print(Counter.class_count) # Output: 2 (shared by all instances)
```

### Methods

```python
class Calculator:
    def __init__(self, value=0):
        self.value = value
    
    # Instance method
    def add(self, num):
        self.value += num
        return self.value
    
    # Class method
    @classmethod
    def create_zero(cls):
        return cls(0)
    
    # Static method (doesn't need self)
    @staticmethod
    def is_positive(num):
        return num > 0
    
    # String representation
    def __str__(self):
        return f"Calculator with value: {self.value}"

calc = Calculator(5)
print(calc.add(3))         # Output: 8
print(Calculator.is_positive(5))  # Output: True
print(calc)                # Output: Calculator with value: 8
```

## Inheritance

Inheritance allows a class to inherit attributes and methods from another class.

### Basic Inheritance

```python
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return f"{self.name} makes a sound"

# Child class
class Dog(Animal):
    def speak(self):  # Override parent method
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

dog = Dog("Buddy")
cat = Cat("Whiskers")

print(dog.speak())         # Output: Buddy says Woof!
print(cat.speak())         # Output: Whiskers says Meow!
```

### Using super()

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand
    
    def info(self):
        return f"Brand: {self.brand}"

class Car(Vehicle):
    def __init__(self, brand, model):
        super().__init__(brand)  # Call parent constructor
        self.model = model
    
    def info(self):
        parent_info = super().info()  # Call parent method
        return f"{parent_info}, Model: {self.model}"

car = Car("Toyota", "Corolla")
print(car.info())          # Output: Brand: Toyota, Model: Corolla
```

### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "Flying..."

class Swimmable:
    def swim(self):
        return "Swimming..."

class Duck(Flyable, Swimmable):
    pass

duck = Duck()
print(duck.fly())          # Output: Flying...
print(duck.swim())         # Output: Swimming...
```

## Polymorphism

Polymorphism allows objects of different types to be used interchangeably.

### Method Overriding

```python
class Shape:
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

# Polymorphic behavior
shapes = [Circle(5), Rectangle(4, 6)]

for shape in shapes:
    print(shape.area())    # Different output for each shape
```

## Encapsulation

Encapsulation restricts direct access to some of an object's attributes and methods.

### Private and Protected Attributes

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner
        self.__balance = balance  # Private attribute (name mangling)
        self._pin = "1234"        # Protected attribute (convention)
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            return f"Deposited: ${amount}"
        return "Invalid amount"
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return f"Withdrawn: ${amount}"
        return "Insufficient funds"
    
    def get_balance(self):
        return self.__balance

account = BankAccount("John", 1000)
print(account.deposit(500))        # Output: Deposited: $500
print(account.withdraw(200))       # Output: Withdrawn: $200
print(account.get_balance())       # Output: 1300

# Accessing private attribute (name mangling)
print(account._BankAccount__balance)  # Output: 1300 (not recommended)
```

### Properties

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature cannot be below -273.15°C")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        return self._celsius * 9/5 + 32

temp = Temperature(25)
print(temp.celsius)        # Output: 25
print(temp.fahrenheit)     # Output: 77.0

temp.celsius = 30          # Uses setter
print(temp.celsius)        # Output: 30
```

## Abstraction

Abstraction hides complex implementation details and shows only essential features.

### Abstract Base Classes

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass
    
    @abstractmethod
    def stop(self):
        pass

class Car(Vehicle):
    def start(self):
        return "Car engine started"
    
    def stop(self):
        return "Car engine stopped"

class Motorcycle(Vehicle):
    def start(self):
        return "Motorcycle engine started"
    
    def stop(self):
        return "Motorcycle engine stopped"

# Cannot instantiate abstract class
# vehicle = Vehicle()  # TypeError

car = Car()
print(car.start())         # Output: Car engine started
```

## Special Methods

Special methods (magic methods) have special meanings in Python.

```python
class Book:
    def __init__(self, title, author, pages):
        self.title = title
        self.author = author
        self.pages = pages
    
    # String representation
    def __str__(self):
        return f"{self.title} by {self.author}"
    
    # Official representation
    def __repr__(self):
        return f"Book('{self.title}', '{self.author}', {self.pages})"
    
    # Length
    def __len__(self):
        return self.pages
    
    # Comparison
    def __eq__(self, other):
        return self.pages == other.pages
    
    def __lt__(self, other):
        return self.pages < other.pages
    
    # Addition
    def __add__(self, other):
        return self.pages + other.pages

book1 = Book("Python Basics", "John Doe", 300)
book2 = Book("Advanced Python", "Jane Smith", 400)

print(str(book1))          # Output: Python Basics by John Doe
print(len(book1))          # Output: 300
print(book1 == book2)      # Output: False
print(book1 < book2)       # Output: True
print(book1 + book2)       # Output: 700
```
