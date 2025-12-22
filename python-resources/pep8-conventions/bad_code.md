# BAD CODE EXAMPLES
# This file demonstrates what NOT to do in Python

# Poor variable naming
x = 10
y = 20
z = x + y
print(z)

# No spacing around operators
result=x+5*3

# Improper indentation (will raise error / confusing)
if x>5:
print("x is greater than 5")

# Function name not following snake_case
def AddNumbers(a,b):
    print(a+b)

# Function prints instead of returning
def calc(a,b):
    c=a+b
    print("Result is",c)

# Magic number (no explanation what 40 means)
salary = hours * 40

# Very long line (hard to read and violates PEP8)
total = first_value + second_value + third_value + fourth_value + fifth_value + sixth_value
