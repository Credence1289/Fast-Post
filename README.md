# 📝 FastPost

A RESTful API built with **FastAPI** for user authentication and post management. Users can register, log in, and create, view, update, and delete posts — all secured with JWT authentication.

---

## 🚀 Tech Stack

| Technology | Purpose |
|---|---|
| **FastAPI** | Web framework |
| **SQLAlchemy** | ORM for database interaction |
| **PostgreSQL** | Relational database |
| **python-jose** | JWT token creation and decoding |
| **bcrypt** | Password hashing |
| **Pydantic** | Data validation and serialization |
| **python-dotenv** | Environment variable management |
| **Uvicorn** | ASGI server |

---

## 📁 Project Structure

```
FastPost/
├── backend/
│   ├── .env                  # Environment variables (never push this!)
│   ├── .env.example          # Template for environment variables
│   ├── auth.py               # JWT token creation and decoding
│   ├── create_table.py       # Script to initialize database tables
│   ├── db_models.py          # SQLAlchemy ORM models (User, Post)
│   ├── dbengine.py           # Database engine setup
│   ├── gate.py               # Auth dependency (current_user guard)
│   ├── hash.py               # Password hashing and verification
│   ├── main.py               # FastAPI app and route definitions
│   ├── models.py             # Pydantic schemas (request/response)
│   └── session.py            # DB session management
├── frontend/
│   └── frontend.html         # Simple browser-based UI
├── .gitignore
└── requirements.txt
```

---

## ⚙️ Setup & Installation

### 1. Clone the repository

```bash
git clone https://github.com/your-username/FastPost.git
cd FastPost
```

### 2. Create and activate a virtual environment

```bash
python -m venv myenv

# Windows
myenv\Scripts\activate

# Mac/Linux
source myenv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `backend/.env` file based on the provided example:

```bash
cp backend/.env.example backend/.env
```

Then fill in your values:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/yourdbname
SECRET_KEY=your_super_secret_key
```

### 5. Initialize the database

```bash
cd backend
python create_table.py
```

### 6. Run the server

```bash
uvicorn main:app --reload
```

The API will be available at `http://127.0.0.1:8000`

> 📖 Interactive API docs available at `http://127.0.0.1:8000/docs`

---

## 🔐 Authentication

This API uses **JWT Bearer tokens**. After logging in, include the token in the `Authorization` header for all protected routes:

```
Authorization: Bearer <your_access_token>
```

> Tokens expire after **30 minutes**.

---

## 📡 API Endpoints

### Auth

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/Register` | Register a new user | ❌ |
| POST | `/login` | Login and receive JWT token | ❌ |

### Posts

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/post` | Create a new post | ✅ |
| GET | `/post` | Get current user's posts | ✅ |
| GET | `/posts` | Get all posts (filterable) | ✅ |
| PUT | `/post/{post_id}` | Update a specific post | ✅ |
| DELETE | `/post/{post_id}` | Delete a specific post | ✅ |
| DELETE | `/post` | Delete all of current user's posts | ✅ |

> **Filter posts by type:** `GET /posts?content_type=blog`  
> Pass `all` to return all posts regardless of type.

---

## 📦 Request & Response Examples

### Register — `POST /Register`

```json
{
  "name": "John Doe",
  "username": "johndoe",
  "email": "john@example.com",
  "password": "securepassword"
}
```

### Login — `POST /login`

```json
{
  "username": "johndoe",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "bearer"
}
```

### Create Post — `POST /post`

```json
{
  "content_type": "blog",
  "title": "My First Post",
  "post": "Hello, world!"
}
```

---

## 🗄️ Database Models

### User
| Field | Type | Description |
|---|---|---|
| `user_id` | Integer (PK) | Auto-incremented ID |
| `name` | String | Full name |
| `username` | String | Unique username |
| `email` | String (unique) | Email address |
| `hashed_password` | String | Bcrypt hashed password |

### Post
| Field | Type | Description |
|---|---|---|
| `post_id` | Integer (PK) | Auto-incremented ID |
| `user_id` | FK → users | Owner of the post |
| `content_type` | String | Type of post (e.g. blog, news) |
| `title` | String | Post title |
| `post` | String | Post content |
| `created_at` | DateTime | Auto-set on creation |

---

## 🖥️ Frontend

A basic HTML frontend (`frontend/frontend.html`) is included for interacting with the API directly in the browser — no Postman needed.

---

## 🔒 Security Notes

- Passwords are hashed with **bcrypt** — never stored in plain text
- JWT tokens expire after **30 minutes**
- Users can only edit/delete **their own posts**
- Sensitive config lives in **`.env`** — never commit this file

---


