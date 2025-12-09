# Python Frameworks Guide - File Index

This is the index for the Python Frameworks Guide. The original document has been broken down into 4 smaller, focused sections for easier reading and reference.

## Files Overview

### 1. **01-python-pydantic.md**
Data validation and parsing with Pydantic:
- What is Pydantic and why use it
- Installation and setup
- **Basic models**:
  - Creating Pydantic models
  - Type validation
  - Supported type hints
  
- **Field validation**:
  - Constraints (min/max, patterns, regex)
  - Custom validators with @field_validator
  - Common field constraints

- **Model configuration**:
  - Config classes
  - Useful configuration options
  
- **Serialization**:
  - Converting to dictionary and JSON
  - Partial serialization (exclude/include fields)
  
- **Nested models**: Models within models
- **Best practices** and common issues

### 2. **02-python-sqlalchemy.md**
Database operations with SQLAlchemy ORM:
- Why use SQLAlchemy (ORM abstraction, database agnostic, etc.)
- Installation and database drivers
- **Database connection**:
  - Setting up the engine (SQLite, PostgreSQL, MySQL)
  - Creating tables

- **Defining models**:
  - Basic model definition with columns
  - Column types and options (String, Integer, Float, DateTime, etc.)
  
- **Relationships**:
  - One-to-Many relationships
  - Many-to-Many relationships
  
- **CRUD Operations**:
  - Create (Insert)
  - Read (Query) - filtering, conditions, counting
  - Update
  - Delete
  
- **Sessions**:
  - Using context managers
  - Custom context manager patterns
  
- **Advanced queries**:
  - Complex filtering with and_/or_
  - Sorting and limiting
  - Joins and eager loading
  
- **Best practices** and common issues

### 3. **03-python-fastapi.md**
Building modern web APIs with FastAPI:
- Why use FastAPI (speed, ease, validation, docs, async, etc.)
- Installation and setup
- **Creating your first API**:
  - Basic application structure
  - Running the API with Uvicorn
  - Automatic documentation

- **HTTP methods**:
  - GET - retrieving data (path and query parameters)
  - POST - creating data
  - PUT - updating data
  - DELETE - removing data

- **Request and response models**:
  - Using Pydantic models
  - Response models for validation
  
- **Path and query parameters**:
  - Required path parameters
  - Optional query parameters with defaults
  
- **Status codes and headers**:
  - Custom status codes
  - Standard HTTP status codes
  
- **Error handling**:
  - HTTP exceptions
  - Custom error responses
  
- **Dependency injection**:
  - Using dependencies
  - Multiple dependencies
  - Real-world examples
  
- **Database integration**:
  - Complete CRUD example with database
  - Session dependency
  
- **CORS configuration** for frontend integration
- **Async endpoints** with async/await
- **Best practices** and patterns

### 4. **04-python-complete-app.md**
Building production-ready applications and troubleshooting:
- **Project structure**:
  - Recommended file organization
  - Separation of concerns (models, schemas, crud, routes)

- **Complete application example**:
  - database.py setup with engine and sessions
  - models.py with SQLAlchemy models and relationships
  - schemas.py with Pydantic request/response models
  - crud.py with database operations
  - routes/ package with modular endpoints
  - main.py integrating everything with CORS and middleware

- **Common issues and solutions** (8 issues):
  1. ValidationError on model creation
  2. SQLAlchemy: Cannot access closed session
  3. CORS Error in frontend
  4. Relationship not loading
  5. Duplicate entry in database
  6. N+1 Query Problem
  7. Password stored as plaintext
  8. Solutions with code examples

- **Environment variables**:
  - Using .env files with python-dotenv
  - Configuration management
  
- **Testing your API**:
  - Using pytest and TestClient
  - Example test cases
  
- **Production checklist** for deployment

## How to Use This Guide

### For Complete Beginners

1. Start with **01-python-pydantic.md** - Learn data validation
2. Move to **02-python-sqlalchemy.md** - Understand databases
3. Learn **03-python-fastapi.md** - Build APIs
4. Reference **04-python-complete-app.md** - See it all together

### For Learning Specific Topics

| Topic | File |
|-------|------|
| Data validation | 01-python-pydantic.md |
| Database setup | 02-python-sqlalchemy.md |
| API endpoints | 03-python-fastapi.md |
| Complete app structure | 04-python-complete-app.md |
| Fixing errors | 04-python-complete-app.md |
| Database queries | 02-python-sqlalchemy.md |
| Field constraints | 01-python-pydantic.md |
| Authentication | 04-python-complete-app.md |

### For Quick Reference

- **Installation**: Top of each file
- **Basic example**: Early sections
- **Advanced patterns**: Later sections in each file
- **Troubleshooting**: 04-python-complete-app.md

## Key Concepts Overview

### Pydantic
```python
from pydantic import BaseModel, Field

class User(BaseModel):
    name: str = Field(..., min_length=1)
    email: str
    age: int = Field(..., ge=0, le=150)
```

### SQLAlchemy
```python
from sqlalchemy import Column, String, create_engine
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String, nullable=False)
```

### FastAPI
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

@app.post("/users")
def create_user(user: UserSchema):
    return user
```

## Technology Stack Integration

```
┌─────────────┐
│   FastAPI   │  (Web Framework)
└──────┬──────┘
       │
   ┌───┴────┐
   │        │
┌──▼──┐  ┌──▼─────────┐
│HTTP │  │ Pydantic   │  (Request validation)
└─────┘  └─────┬──────┘
              │
      ┌───────┴────────┐
      │                │
   ┌──▼────┐    ┌──────▼──┐
   │ Models│    │ Schemas │
   └───────┘    └─────────┘
         │           │
         └───────┬───┘
              ┌──▼──────────┐
              │ SQLAlchemy  │  (Database)
              └──────┬──────┘
                     │
              ┌──────▼──────┐
              │   Database  │
              │(SQL/NoSQL)  │
              └─────────────┘
```

## Development Workflow

### Step 1: Define Data Models (Pydantic)
```python
class User(BaseModel):
    name: str
    email: str
```

### Step 2: Create Database Models (SQLAlchemy)
```python
class User(Base):
    __tablename__ = "users"
    # ... columns
```

### Step 3: Add API Endpoints (FastAPI)
```python
@app.post("/users")
def create_user(user: UserSchema, db: Session):
    # Save to database
    return user
```

### Step 4: Test and Deploy
- Use pytest for testing
- Deploy with uvicorn or gunicorn

## Complete Request Flow

```
1. Client sends HTTP request
    ↓
2. FastAPI receives request
    ↓
3. Pydantic validates data
    ↓
4. Endpoint function receives validated data
    ↓
5. SQLAlchemy performs database operation
    ↓
6. Data returned from database
    ↓
7. FastAPI serializes response (Pydantic)
    ↓
8. HTTP response sent to client
```

## Installation (All Frameworks)

```bash
pip install fastapi
pip install sqlalchemy
pip install pydantic
pip install uvicorn  # To run FastAPI
pip install python-dotenv  # For environment variables
```

## Running Your First API

```bash
# Create main.py with FastAPI app
# Run with:
uvicorn main:app --reload
```

Then visit:
- `http://localhost:8000/docs` - Swagger UI documentation
- `http://localhost:8000/redoc` - ReDoc documentation
- `http://localhost:8000/openapi.json` - OpenAPI schema

## Best Practices Across All Three

1. **Type hints everywhere** - Pydantic, SQLAlchemy, FastAPI all support them
2. **Validation at entry** - Pydantic models for all inputs
3. **Relationships matter** - Define proper SQLAlchemy relationships
4. **Separate concerns** - Models, schemas, CRUD, routes in different files
5. **Error handling** - Use HTTPException appropriately
6. **Documentation** - Use docstrings and FastAPI descriptions
7. **Testing** - Write tests for all endpoints
8. **Environment variables** - Never hardcode secrets

## Common Patterns

### Pattern 1: Create Operation
1. Define Pydantic schema (Input)
2. Define SQLAlchemy model
3. FastAPI endpoint receives Pydantic model
4. CRUD function creates database entry
5. Return response model

### Pattern 2: Read Operation
1. FastAPI endpoint accepts ID
2. CRUD function queries database
3. Return SQLAlchemy model
4. FastAPI converts to response model
5. Return JSON response

### Pattern 3: Update Operation
1. FastAPI endpoint accepts ID and update data
2. CRUD function retrieves and updates
3. Commit changes
4. Return updated model

### Pattern 4: Delete Operation
1. FastAPI endpoint accepts ID
2. CRUD function finds and deletes
3. Return success message

## Quick Start (15 Minutes)

1. **Install** (2 min):
   ```bash
   pip install fastapi sqlalchemy pydantic uvicorn python-dotenv
   ```

2. **Create models** (3 min) - See 02-python-sqlalchemy.md

3. **Create schemas** (3 min) - See 01-python-pydantic.md

4. **Create API** (4 min) - See 03-python-fastapi.md

5. **Test** (3 min):
   ```bash
   uvicorn main:app --reload
   ```

---

**Last Updated**: 2025
**Python Version**: 3.8+
**Frameworks**: FastAPI, SQLAlchemy, Pydantic
**Topics Covered**: API Development, Data Validation, Database Operations
