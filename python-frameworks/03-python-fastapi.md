# FastAPI: Building Modern Web APIs

FastAPI is a modern, fast web framework for building APIs with Python. It combines Pydantic for validation, SQLAlchemy for databases, and provides automatic API documentation.

## Why Use FastAPI?

- **Fast**: Very high performance (comparable to Node.js and Go)
- **Easy**: Simple to learn and use
- **Built-in validation**: Uses Pydantic models automatically
- **Auto documentation**: Automatic interactive API docs (Swagger UI, ReDoc)
- **Type hints**: Full type support for better development experience
- **Async support**: Build concurrent applications easily
- **Dependency injection**: Built-in dependency system

## Installation

```bash
pip install fastapi
pip install uvicorn  # ASGI server to run FastAPI
```

## Creating Your First API

### Basic Application

```python
from fastapi import FastAPI

# Create app instance
app = FastAPI(
    title="My API",
    description="A simple API",
    version="1.0.0"
)

# Define a route
@app.get("/")
def read_root():
    return {"message": "Hello, World!"}

# Run the app: uvicorn main:app --reload
```

### Running the API

```bash
# Run with auto-reload (development)
uvicorn main:app --reload

# Run on specific port
uvicorn main:app --port 8000

# Run on all interfaces
uvicorn main:app --host 0.0.0.0
```

Visit `http://localhost:8000/docs` for automatic API documentation.

## HTTP Methods

### GET - Retrieve Data

```python
from fastapi import FastAPI

app = FastAPI()

# Simple GET
@app.get("/items")
def get_items():
    return [{"item": "Apple"}, {"item": "Banana"}]

# GET with path parameter
@app.get("/items/{item_id}")
def get_item(item_id: int):
    return {"item_id": item_id, "item_name": "Apple"}

# GET with query parameters
@app.get("/search")
def search(q: str, skip: int = 0, limit: int = 10):
    return {
        "query": q,
        "skip": skip,
        "limit": limit
    }
```

### POST - Create Data

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float
    description: str = None

@app.post("/items")
def create_item(item: Item):
    return {
        "message": "Item created",
        "item": item
    }
```

### PUT - Update Data

```python
@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    return {
        "item_id": item_id,
        "item": item,
        "message": "Item updated"
    }
```

### DELETE - Remove Data

```python
@app.delete("/items/{item_id}")
def delete_item(item_id: int):
    return {
        "item_id": item_id,
        "message": "Item deleted"
    }
```

## Request and Response Models

### Using Pydantic Models

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Optional

app = FastAPI()

class User(BaseModel):
    name: str
    email: str
    age: Optional[int] = None

class UserResponse(BaseModel):
    id: int
    name: str
    email: str

@app.post("/users", response_model=UserResponse)
def create_user(user: User):
    # In real app, save to database
    return {
        "id": 1,
        "name": user.name,
        "email": user.email
    }
```

## Path Parameters and Query Parameters

### Path Parameters (Required)

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}

# With multiple parameters
@app.get("/users/{user_id}/posts/{post_id}")
def get_user_post(user_id: int, post_id: int):
    return {"user_id": user_id, "post_id": post_id}
```

### Query Parameters (Optional)

```python
@app.get("/items")
def list_items(
    skip: int = 0,           # Default value
    limit: int = 10,         # Default value
    q: Optional[str] = None  # Optional
):
    return {
        "skip": skip,
        "limit": limit,
        "search": q
    }
```

## Status Codes and Headers

### Custom Status Codes

```python
from fastapi import FastAPI, status

app = FastAPI()

@app.post("/items", status_code=status.HTTP_201_CREATED)
def create_item(item: dict):
    return item

@app.delete("/items/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_item(item_id: int):
    return None
```

## Error Handling

### HTTP Exceptions

```python
from fastapi import FastAPI, HTTPException, status

app = FastAPI()

@app.get("/items/{item_id}")
def get_item(item_id: int):
    if item_id == 0:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="Item not found"
        )
    return {"item_id": item_id}

@app.post("/users")
def create_user(name: str):
    if not name:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Name cannot be empty"
        )
    return {"name": name}
```

## Dependency Injection

### Using Dependencies

```python
from fastapi import FastAPI, Depends

app = FastAPI()

# Define a dependency
def get_query(q: str = None, skip: int = 0):
    return {"q": q, "skip": skip}

# Use the dependency
@app.get("/items")
def read_items(common: dict = Depends(get_query)):
    return common

# Use multiple dependencies
def verify_token(token: str):
    if not token:
        raise HTTPException(status_code=401, detail="No token")
    return token

@app.get("/protected")
def protected_route(token: str = Depends(verify_token)):
    return {"token": token, "message": "Access granted"}
```

## Database Integration with FastAPI

### Complete Example

```python
from fastapi import FastAPI, HTTPException
from sqlalchemy.orm import sessionmaker, Session
from pydantic import BaseModel
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base

# Database setup
DATABASE_URL = "sqlite:///./test.db"
engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)
Base = declarative_base()

# SQLAlchemy model
class UserDB(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String)

# Pydantic model
class User(BaseModel):
    name: str
    email: str

class UserResponse(User):
    id: int

# Create tables
Base.metadata.create_all(engine)

app = FastAPI()

# Dependency to get database session
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Routes
@app.get("/users/{user_id}", response_model=UserResponse)
def get_user(user_id: int, db: Session = Depends(get_db)):
    user = db.query(UserDB).filter(UserDB.id == user_id).first()
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user

@app.post("/users", response_model=UserResponse)
def create_user(user: User, db: Session = Depends(get_db)):
    db_user = UserDB(name=user.name, email=user.email)
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

@app.delete("/users/{user_id}")
def delete_user(user_id: int, db: Session = Depends(get_db)):
    user = db.query(UserDB).filter(UserDB.id == user_id).first()
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    db.delete(user)
    db.commit()
    return {"message": "User deleted"}
```

## CORS (Cross-Origin Resource Sharing)

### Enable CORS

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# Allow all origins (development only)
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Allow specific origins (production)
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:3000",
        "https://example.com"
    ],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["*"],
)
```

## Async Endpoints

### Using async/await

```python
from fastapi import FastAPI
import asyncio

app = FastAPI()

@app.get("/async-items")
async def read_async_items():
    await asyncio.sleep(1)  # Simulate async operation
    return [{"item": "Apple"}, {"item": "Banana"}]

# async endpoints are faster when doing I/O operations
```

## Best Practices

1. **Use Pydantic models** for request/response validation
2. **Use status codes** appropriately
3. **Handle errors** with HTTPException
4. **Use dependencies** for code reuse
5. **Document your API** with docstrings
6. **Use async** for I/O-bound operations
7. **Separate concerns** - database, routes, models

## Common Endpoints Pattern

```python
@app.get("/items", response_model=list)
def list_items(db: Session = Depends(get_db)):
    return db.query(Item).all()

@app.post("/items", response_model=ItemResponse)
def create_item(item: ItemCreate, db: Session = Depends(get_db)):
    db_item = Item(**item.dict())
    db.add(db_item)
    db.commit()
    return db_item

@app.get("/items/{item_id}", response_model=ItemResponse)
def get_item(item_id: int, db: Session = Depends(get_db)):
    item = db.query(Item).filter(Item.id == item_id).first()
    if not item:
        raise HTTPException(404, "Not found")
    return item

@app.put("/items/{item_id}", response_model=ItemResponse)
def update_item(item_id: int, item: ItemUpdate, db: Session = Depends(get_db)):
    db_item = db.query(Item).filter(Item.id == item_id).first()
    if not db_item:
        raise HTTPException(404, "Not found")
    for key, value in item.dict().items():
        setattr(db_item, key, value)
    db.commit()
    return db_item

@app.delete("/items/{item_id}")
def delete_item(item_id: int, db: Session = Depends(get_db)):
    db_item = db.query(Item).filter(Item.id == item_id).first()
    if not db_item:
        raise HTTPException(404, "Not found")
    db.delete(db_item)
    db.commit()
    return {"message": "Deleted"}
```
