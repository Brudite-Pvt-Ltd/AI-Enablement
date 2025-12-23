# GOOD CODE EXAMPLES

# Descriptive variable names
first_number = 10
second_number = 20
total_sum = first_number + second_number
print(total_sum)

# Proper spacing around operators
result = x + 5 * 3

# Correct indentation (4 spaces)
if x > 5:
    print("x is greater than 5")

# Snake_case function naming
# Function returns value instead of printing
def add_numbers(num1, num2):
    return num1 + num2

# Single-responsibility function
def calculate_sum(num1, num2):
    return num1 + num2

# Avoid magic numbers using constants
HOURLY_RATE = 40
salary = hours * HOURLY_RATE

# Line wrapping for readability (max 79 chars)
total = (
    first_value + second_value + third_value
    + fourth_value + fifth_value + sixth_value
)

# Meaningful comment (explains WHY, not WHAT)
# Increment retry count after a failed request
retry_count += 1

# One import per line
import os
import sys
import math

# Clear and readable conditional logic
if a == b and b == c:
    print("All values are equal")
elif a == c:
    print("First and third values are equal")
