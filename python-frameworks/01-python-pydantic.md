# Pydantic: Data Validation and Parsing

Pydantic is a data validation library that uses Python type hints to validate data automatically.

## Why Use Pydantic?

- **Type validation**: Ensures data types are correct
- **Data parsing**: Converts input to correct types automatically
- **Error messages**: Provides clear validation error messages
- **Serialization**: Converts Python objects to JSON
- **Integration**: Works seamlessly with FastAPI

## Installation

```bash
pip install pydantic
```

## Basic Models

### Creating a Pydantic Model

```python
from pydantic import BaseModel
from typing import Optional

# Define a data model
class User(BaseModel):
    name: str
    email: str
    age: int
    city: Optional[str] = None  # Optional field with default

# Create an instance
user_data = {
    "name": "John",
    "email": "john@example.com",
    "age": 30,
    "city": "New York"
}

user = User(**user_data)
print(user.name)              # Output: John
print(user.age)               # Output: 30
```

### Type Validation

```python
from pydantic import BaseModel, ValidationError

class Product(BaseModel):
    name: str
    price: float
    quantity: int

# Valid data
try:
    product = Product(name="Laptop", price=999.99, quantity=5)
    print(product)
except ValidationError as e:
    print(e)

# Invalid data - will raise ValidationError
try:
    product = Product(name="Laptop", price="expensive", quantity=5)
except ValidationError as e:
    print("Validation error:", e)
    # Output: price must be a valid number
```

### Type Hints Supported

```python
from pydantic import BaseModel
from datetime import datetime
from typing import List, Optional, Dict

class Article(BaseModel):
    title: str
    content: str
    author: str
    tags: List[str]  # List of strings
    views: int = 0   # Default value
    published: bool = False
    created_at: datetime
    metadata: Optional[Dict[str, str]] = None  # Optional dictionary

# Create instance
article = Article(
    title="Python Tips",
    content="Some content here",
    author="John",
    tags=["python", "tips", "programming"],
    created_at="2025-12-09T10:30:00"
)

print(article.tags)  # Output: ['python', 'tips', 'programming']
```

## Field Validation

### Custom Validation with Constraints

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)  # ... means required
    email: str = Field(..., description="User's email address")
    age: int = Field(..., ge=0, le=150)  # ge = greater or equal, le = less or equal
    phone: str = Field(default=None, pattern=r'^\d{10}$')  # Regex pattern

# Valid
try:
    user = User(name="John", email="john@example.com", age=30, phone="1234567890")
    print("User created successfully")
except ValidationError as e:
    print(e)
```

### Common Field Constraints

```python
from pydantic import BaseModel, Field, EmailStr
from typing import Optional

class UserProfile(BaseModel):
    # String constraints
    username: str = Field(..., min_length=3, max_length=20)
    
    # Email validation
    email: str = Field(..., description="Valid email address")
    
    # Numeric constraints
    age: int = Field(..., ge=18, le=120)
    salary: float = Field(..., gt=0)  # gt = greater than
    
    # String with pattern
    phone: str = Field(..., pattern=r'^\d{10}$')
    
    # List constraints
    tags: list = Field(default_factory=list, max_items=5)  # max 5 items
    
    # Optional with default
    bio: Optional[str] = Field(default=None, max_length=500)
```

## Custom Validators

### Using @field_validator

```python
from pydantic import BaseModel, field_validator

class User(BaseModel):
    name: str
    email: str
    password: str
    
    @field_validator('name')
    def name_not_empty(cls, v):
        if not v.strip():
            raise ValueError('Name cannot be empty')
        return v
    
    @field_validator('email')
    def email_valid(cls, v):
        if '@' not in v:
            raise ValueError('Invalid email format')
        return v
    
    @field_validator('password')
    def password_strong(cls, v):
        if len(v) < 8:
            raise ValueError('Password must be at least 8 characters')
        return v

# Test
try:
    user = User(name="John", email="john@example.com", password="short")
except ValidationError as e:
    print(e)  # Shows password error
```

## Model Configuration

### Config Class

```python
from pydantic import BaseModel, ConfigDict

class User(BaseModel):
    model_config = ConfigDict(str_strip_whitespace=True)
    
    name: str
    email: str

# Whitespace is automatically stripped
user = User(name="  John  ", email="john@example.com  ")
print(user.name)  # Output: "John" (spaces removed)
```

### Useful Config Options

```python
from pydantic import BaseModel, ConfigDict

class Product(BaseModel):
    model_config = ConfigDict(
        str_strip_whitespace=True,        # Strip whitespace from strings
        validate_assignment=True,         # Validate on assignment
        use_attribute_docstrings=True,    # Use docstrings for descriptions
        from_attributes=True,             # Allow ORM objects
        json_schema_extra={"example": "..."}  # Extra schema info
    )
    
    name: str
    price: float
```

## Serialization and JSON

### Convert to Dictionary

```python
from pydantic import BaseModel

class User(BaseModel):
    name: str
    email: str
    age: int

user = User(name="John", email="john@example.com", age=30)

# To dictionary
user_dict = user.model_dump()
print(user_dict)  # {'name': 'John', 'email': 'john@example.com', 'age': 30}

# To JSON string
user_json = user.model_dump_json()
print(user_json)  # '{"name":"John","email":"john@example.com","age":30}'
```

### Partial Serialization

```python
class User(BaseModel):
    name: str
    email: str
    password: str  # Don't expose this

# Exclude sensitive fields
user_dict = user.model_dump(exclude={'password'})

# Include only specific fields
public_data = user.model_dump(include={'name', 'email'})
```

## Nested Models

### Models Within Models

```python
from pydantic import BaseModel
from typing import List

class Address(BaseModel):
    street: str
    city: str
    zip_code: str

class Company(BaseModel):
    name: str
    address: Address

class Employee(BaseModel):
    name: str
    email: str
    company: Company
    addresses: List[Address]  # List of nested models

# Create instance
employee = Employee(
    name="John",
    email="john@example.com",
    company={
        "name": "Tech Corp",
        "address": {
            "street": "123 Main St",
            "city": "New York",
            "zip_code": "10001"
        }
    },
    addresses=[
        {
            "street": "456 Oak Ave",
            "city": "Boston",
            "zip_code": "02101"
        }
    ]
)

print(employee.company.name)  # Tech Corp
```

## Pydantic Best Practices

1. **Use type hints** for all fields
2. **Provide defaults** for optional fields
3. **Use Field()** for complex validation
4. **Custom validators** for business logic
5. **Meaningful error messages** with descriptions
6. **Serialize carefully** - exclude sensitive data

## Common Issues

**Issue 1: Type mismatch**
```python
class User(BaseModel):
    age: int

# This will automatically convert
user = User(age="30")  # String "30" converts to int 30
```

**Issue 2: Required vs Optional**
```python
class User(BaseModel):
    name: str              # Required
    email: Optional[str]   # Optional
    city: str = "Unknown"  # Optional with default
```
