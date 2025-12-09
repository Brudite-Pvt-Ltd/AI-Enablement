# SQLAlchemy: Database Operations and ORM

SQLAlchemy is a popular Object-Relational Mapping (ORM) library that makes database operations easier and more Pythonic.

## Why Use SQLAlchemy?

- **ORM abstraction**: Write database operations in Python instead of SQL
- **Database agnostic**: Same code works with different databases
- **Relationships**: Easy handling of database relationships
- **Type safety**: Use Python types for database columns
- **Query builder**: Write complex queries in Python

## Installation

```bash
pip install sqlalchemy

# You'll also need a database driver
pip install psycopg2-binary  # For PostgreSQL
pip install mysql-connector-python  # For MySQL
pip install sqlite3  # Built-in for SQLite
```

## Database Connection

### Setting Up the Engine

```python
from sqlalchemy import create_engine

# SQLite (local file)
engine = create_engine('sqlite:///database.db')

# PostgreSQL
engine = create_engine('postgresql+psycopg2://user:password@localhost:5432/dbname')

# MySQL
engine = create_engine('mysql+pymysql://user:password@localhost:3306/dbname')

# In-memory SQLite (for testing)
engine = create_engine('sqlite:///:memory:')
```

### Creating Tables

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import declarative_base

Base = declarative_base()
engine = create_engine('sqlite:///database.db')

# Create all tables
Base.metadata.create_all(engine)

# Drop all tables
Base.metadata.drop_all(engine)
```

## Defining Models

### Basic Model Definition

```python
from sqlalchemy import Column, Integer, String, Float, Boolean, DateTime, Text
from sqlalchemy.orm import declarative_base
from datetime import datetime

Base = declarative_base()

class Product(Base):
    __tablename__ = "products"
    
    # Primary key
    id = Column(Integer, primary_key=True)
    
    # String column
    name = Column(String(100), nullable=False)
    
    # Description with longer text
    description = Column(Text)
    
    # Price
    price = Column(Float, nullable=False)
    
    # Quantity
    quantity = Column(Integer, default=0)
    
    # Boolean flag
    is_available = Column(Boolean, default=True)
    
    # Timestamps
    created_at = Column(DateTime, default=datetime.utcnow)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
```

### Column Types and Options

```python
from sqlalchemy import Column, Integer, String, Float, Boolean, DateTime, Text, Date, Numeric
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class Article(Base):
    __tablename__ = "articles"
    
    id = Column(Integer, primary_key=True)
    title = Column(String(200), nullable=False, unique=True)  # unique = no duplicates
    content = Column(Text)  # For large text
    author = Column(String(100), nullable=False)
    views = Column(Integer, default=0)
    published = Column(Boolean, default=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    updated_at = Column(DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
```

## Relationships

### One-to-Many Relationship

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, declarative_base

Base = declarative_base()

class Author(Base):
    __tablename__ = "authors"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    email = Column(String(100), unique=True)
    
    # Relationship: Author has many Books
    books = relationship("Book", back_populates="author")

class Book(Base):
    __tablename__ = "books"
    
    id = Column(Integer, primary_key=True)
    title = Column(String(200), nullable=False)
    author_id = Column(Integer, ForeignKey("authors.id"), nullable=False)
    
    # Relationship: Book belongs to Author
    author = relationship("Author", back_populates="books")
```

### Many-to-Many Relationship

```python
from sqlalchemy import Table, Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, declarative_base

Base = declarative_base()

# Association table for many-to-many
student_course = Table(
    'student_course',
    Base.metadata,
    Column('student_id', Integer, ForeignKey('students.id'), primary_key=True),
    Column('course_id', Integer, ForeignKey('courses.id'), primary_key=True)
)

class Student(Base):
    __tablename__ = "students"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    
    # Relationship: Student has many Courses
    courses = relationship("Course", secondary=student_course, back_populates="students")

class Course(Base):
    __tablename__ = "courses"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    
    # Relationship: Course has many Students
    students = relationship("Student", secondary=student_course, back_populates="courses")
```

## CRUD Operations

### Create (Insert)

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
session = Session()

# Create new product
product = Product(
    name="Laptop",
    description="High-performance laptop",
    price=999.99,
    quantity=10
)

session.add(product)
session.commit()
print(f"Product created with ID: {product.id}")
session.close()
```

### Read (Query)

```python
Session = sessionmaker(bind=engine)
session = Session()

# Get one product by ID
product = session.query(Product).filter(Product.id == 1).first()
if product:
    print(f"Product: {product.name}, Price: {product.price}")

# Get all products
all_products = session.query(Product).all()
for product in all_products:
    print(f"{product.name}: ${product.price}")

# Filter with conditions
expensive_products = session.query(Product).filter(Product.price > 500).all()

# Filter with multiple conditions
available_expensive = session.query(Product).filter(
    Product.price > 500,
    Product.is_available == True
).all()

# Count
total_products = session.query(Product).count()
print(f"Total products: {total_products}")

session.close()
```

### Update

```python
Session = sessionmaker(bind=engine)
session = Session()

# Get product and update
product = session.query(Product).filter(Product.id == 1).first()
if product:
    product.price = 899.99
    product.quantity = 15
    session.commit()
    print("Product updated")

session.close()
```

### Delete

```python
Session = sessionmaker(bind=engine)
session = Session()

# Get and delete
product = session.query(Product).filter(Product.id == 1).first()
if product:
    session.delete(product)
    session.commit()
    print("Product deleted")

session.close()
```

## Sessions and Context Managers

### Using Context Manager (Recommended)

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)

# Manual context manager
with Session() as session:
    products = session.query(Product).all()
    for product in products:
        print(product.name)
# Session automatically closes and commits
```

### Custom Context Manager

```python
from sqlalchemy.orm import sessionmaker
from contextlib import contextmanager

Session = sessionmaker(bind=engine)

@contextmanager
def get_db():
    session = Session()
    try:
        yield session
    finally:
        session.close()

# Use it
with get_db() as session:
    products = session.query(Product).all()
    for product in products:
        print(product.name)
# Session automatically closes
```

## Advanced Queries

### Filtering with Conditions

```python
from sqlalchemy import and_, or_

# Multiple AND conditions
results = session.query(Product).filter(
    and_(
        Product.price > 100,
        Product.quantity > 0
    )
).all()

# Multiple OR conditions
results = session.query(Product).filter(
    or_(
        Product.name.like('%Laptop%'),
        Product.name.like('%Desktop%')
    )
).all()

# Combined
results = session.query(Product).filter(
    and_(
        Product.price > 100,
        or_(
            Product.is_available == True,
            Product.quantity > 10
        )
    )
).all()
```

### Sorting and Limiting

```python
# Sort ascending
products = session.query(Product).order_by(Product.price).all()

# Sort descending
products = session.query(Product).order_by(Product.price.desc()).all()

# Limit and offset
products = session.query(Product).limit(10).offset(20).all()  # Get 10 items starting from position 20

# First and Last
first = session.query(Product).order_by(Product.id).first()
last = session.query(Product).order_by(Product.id.desc()).first()
```

### Joins

```python
# Simple join
results = session.query(Author, Book).join(Book).filter(Author.id == 1).all()

# With relationship
author = session.query(Author).filter(Author.id == 1).first()
books = author.books  # Access related books directly
```

## SQLAlchemy Best Practices

1. **Use sessions properly** - Always close or use context managers
2. **Use relationships** instead of manual joins
3. **Commit frequently** - Don't hold long transactions
4. **Use transactions** for data consistency
5. **Create indexes** for frequently queried columns
6. **Use lazy loading** appropriately to avoid N+1 queries

## Common Issues

**Issue 1: Forgot to commit**
```python
product.price = 999
# Changes are not saved! Need:
session.commit()
```

**Issue 2: Using closed session**
```python
session.close()
# This will fail:
product.name  # Error: Can't access closed session
```
