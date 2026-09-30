# ⚡ FastAPI User Management REST API

A high-performance, asynchronous RESTful API built with **FastAPI** to manage user data, featuring automatic interactive documentation (Swagger UI).

---

### 🚀 Features:
* **REST Endpoints:** Supports `GET`, `POST`, and data filtering.
* **Auto-Documentation:** Built-in interactive Swagger UI (`/docs`) and ReDoc (`/redoc`).
* **Data Validation:** Utilizes Pydantic models to ensure clean and validated incoming JSON payloads.

---

### 🛠️ Tech Stack:
* **Python 3.8+**
* **FastAPI** & **Uvicorn**

---

### 💻 The Code (`main.py`):
```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI(title="User Management API", version="1.0")

# In-memory database simulation
users_db = [
    {"id": 1, "name": "Ahmed Ali", "email": "ahmed@example.com", "role": "Engineer"},
    {"id": 2, "name": "Sara Mohamed", "email": "sara@example.com", "role": "Developer"}
]

class User(BaseModel):
    name: str
    email: str
    role: Optional[str] = "User"

@app.get("/")
def home():
    return {"message": "Welcome to FastAPI User Management API!"}

@app.get("/users", response_model=List[dict])
def get_users():
    return users_db

@app.post("/users", status_code=201)
def create_user(user: User):
    new_id = len(users_db) + 1
    new_user = {"id": new_id, "name": user.name, "email": user.email, "role": user.role}
    users_db.append(new_user)
    return {"message": "User created successfully", "user": new_user}
