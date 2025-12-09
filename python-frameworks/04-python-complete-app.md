# Building Complete Applications and Troubleshooting

Learn how to build production-ready applications combining Pydantic, SQLAlchemy, and FastAPI, plus solutions to common problems.

## Project Structure

### Recommended Organization

```
my_api/
├── main.py                 # FastAPI app entry point
├── requirements.txt        # Dependencies
├── .env                    # Environment variables
├── .env.example           # Example environment variables
├── database.py            # Database setup
├── models.py              # SQLAlchemy models
├── schemas.py             # Pydantic schemas
├── crud.py                # Database operations
└── routes/
    ├── __init__.py
    ├── users.py           # User endpoints
    ├── items.py           # Item endpoints
    └── posts.py           # Post endpoints
```

## Complete Application Example

### database.py

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import declarative_base, sessionmaker
import os
from dotenv import load_dotenv

load_dotenv()

DATABASE_URL = os.getenv("DATABASE_URL", "sqlite:///./test.db")

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False} if "sqlite" in DATABASE_URL else {}
)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### models.py

```python
from sqlalchemy import Column, Integer, String, Text, DateTime, Boolean, ForeignKey
from sqlalchemy.orm import relationship
from datetime import datetime
from database import Base

class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True)
    username = Column(String(50), unique=True, nullable=False)
    email = Column(String(100), unique=True, nullable=False)
    password = Column(String(255), nullable=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    posts = relationship("Post", back_populates="author")

class Post(Base):
    __tablename__ = "posts"
    
    id = Column(Integer, primary_key=True)
    title = Column(String(200), nullable=False)
    content = Column(Text, nullable=False)
    author_id = Column(Integer, ForeignKey("users.id"), nullable=False)
    published = Column(Boolean, default=False)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    author = relationship("User", back_populates="posts")
```

### schemas.py

```python
from pydantic import BaseModel, EmailStr, Field
from typing import Optional, List
from datetime import datetime

class UserCreate(BaseModel):
    username: str = Field(..., min_length=3, max_length=50)
    email: EmailStr
    password: str = Field(..., min_length=8)

class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    created_at: datetime
    
    class Config:
        from_attributes = True

class PostCreate(BaseModel):
    title: str = Field(..., min_length=1, max_length=200)
    content: str = Field(..., min_length=1)
    published: bool = False

class PostResponse(BaseModel):
    id: int
    title: str
    content: str
    author_id: int
    published: bool
    created_at: datetime
    
    class Config:
        from_attributes = True

class UserWithPosts(UserResponse):
    posts: List[PostResponse] = []
```

### crud.py

```python
from sqlalchemy.orm import Session
from models import User, Post
from schemas import UserCreate, PostCreate

# User operations
def get_user(db: Session, user_id: int):
    return db.query(User).filter(User.id == user_id).first()

def get_user_by_email(db: Session, email: str):
    return db.query(User).filter(User.email == email).first()

def create_user(db: Session, user: UserCreate):
    db_user = User(
        username=user.username,
        email=user.email,
        password=user.password  # Note: hash password in real app!
    )
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

def get_all_users(db: Session, skip: int = 0, limit: int = 10):
    return db.query(User).offset(skip).limit(limit).all()

# Post operations
def create_post(db: Session, post: PostCreate, author_id: int):
    db_post = Post(
        **post.dict(),
        author_id=author_id
    )
    db.add(db_post)
    db.commit()
    db.refresh(db_post)
    return db_post

def get_user_posts(db: Session, user_id: int):
    return db.query(Post).filter(Post.author_id == user_id).all()

def get_published_posts(db: Session):
    return db.query(Post).filter(Post.published == True).all()
```

### routes/users.py

```python
from fastapi import APIRouter, Depends, HTTPException, status
from sqlalchemy.orm import Session
from schemas import UserCreate, UserResponse, UserWithPosts
from crud import create_user, get_user, get_user_by_email, get_all_users
from database import get_db

router = APIRouter(prefix="/users", tags=["users"])

@router.post("/", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
def register_user(user: UserCreate, db: Session = Depends(get_db)):
    # Check if user exists
    if get_user_by_email(db, email=user.email):
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="Email already registered"
        )
    return create_user(db=db, user=user)

@router.get("/{user_id}", response_model=UserWithPosts)
def get_user_with_posts(user_id: int, db: Session = Depends(get_db)):
    user = get_user(db, user_id=user_id)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail="User not found"
        )
    return user

@router.get("/", response_model=list[UserResponse])
def list_users(skip: int = 0, limit: int = 10, db: Session = Depends(get_db)):
    return get_all_users(db, skip=skip, limit=limit)
```

### main.py

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from database import Base, engine
from routes import users

# Create tables
Base.metadata.create_all(bind=engine)

app = FastAPI(
    title="My API",
    description="A sample API",
    version="1.0.0"
)

# Add CORS middleware
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Include routers
app.include_router(users.router)

@app.get("/")
def read_root():
    return {"message": "Welcome to My API"}

# Run: uvicorn main:app --reload
```

## Common Issues and Solutions

### Issue 1: "ValidationError" on Model Creation

**Cause**: Invalid data type passed to Pydantic model.

**Solution**:
```python
# Wrong
user = User(name=123, email="invalid")  # name should be string

# Right
user = User(name="John", email="john@example.com")
```

---

### Issue 2: "SQLAlchemy: Cannot access closed session"

**Cause**: Trying to access database object after session closed.

**Solution**:
```python
# Wrong
session.close()
print(user.name)  # Error!

# Right - Use eager loading
user = session.query(User).options(joinedload(User.posts)).first()
session.close()
print(user.posts)  # Works fine
```

---

### Issue 3: "CORS Error" in Frontend

**Cause**: CORS not properly configured.

**Solution**:
```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],  # Your frontend URL
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

### Issue 4: "Relationship not loading"

**Cause**: Relationship accessed after session closed.

**Solution**:
```python
# Wrong - lazy loading after session closed
user = db.query(User).first()
db.close()
print(user.posts)  # Error!

# Right - eagerly load relationships
from sqlalchemy.orm import joinedload
user = db.query(User).options(joinedload(User.posts)).first()
db.close()
print(user.posts)  # Works!
```

---

### Issue 5: "Duplicate entry" in database

**Cause**: Unique constraint violated.

**Solution**:
```python
from sqlalchemy.exc import IntegrityError

try:
    user = User(username="john", email="john@example.com")
    db.add(user)
    db.commit()
except IntegrityError:
    db.rollback()
    raise HTTPException(
        status_code=400,
        detail="Username or email already exists"
    )
```

---

### Issue 6: "N+1 Query Problem"

**Cause**: Querying database in a loop, creating many queries.

**Solution**:
```python
# Wrong - N+1 problem
users = db.query(User).all()
for user in users:
    print(user.posts)  # One query per user!

# Right - Single query with join
from sqlalchemy.orm import joinedload
users = db.query(User).options(joinedload(User.posts)).all()
for user in users:
    print(user.posts)  # Already loaded!
```

---

### Issue 7: "Password stored as plaintext"

**Cause**: Storing passwords without hashing.

**Solution**:
```bash
pip install passlib[bcrypt]
```

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str):
    return pwd_context.hash(password)

def verify_password(plain: str, hashed: str):
    return pwd_context.verify(plain, hashed)

# In your create user function
user.password = hash_password(user.password)
```

---

## Environment Variables

### .env File

```
DATABASE_URL=postgresql://user:password@localhost/dbname
API_KEY=your_secret_key_here
DEBUG=True
```

### Using Environment Variables

```python
import os
from dotenv import load_dotenv

load_dotenv()

DATABASE_URL = os.getenv("DATABASE_URL")
API_KEY = os.getenv("API_KEY")
DEBUG = os.getenv("DEBUG", "False").lower() == "true"
```

## Testing Your API

### Using pytest

```bash
pip install pytest pytest-asyncio httpx
```

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_create_user():
    response = client.post(
        "/users/",
        json={
            "username": "testuser",
            "email": "test@example.com",
            "password": "testpass123"
        }
    )
    assert response.status_code == 201
    assert response.json()["username"] == "testuser"

def test_get_user():
    response = client.get("/users/1")
    assert response.status_code == 200
```

## Production Checklist

- [ ] Use environment variables for secrets
- [ ] Hash passwords with bcrypt or similar
- [ ] Implement authentication (JWT)
- [ ] Add logging
- [ ] Add rate limiting
- [ ] Use HTTPS
- [ ] Add request validation
- [ ] Error handling with proper status codes
- [ ] Database migrations with Alembic
- [ ] Unit and integration tests
- [ ] API documentation complete
