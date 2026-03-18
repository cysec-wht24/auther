# Auther - Authentication System Sandbox

A complete authentication system built entirely in Python for learning and practicing security attacks on authentication mechanisms. Includes a Flask REST API backend, SQLite database, and command-line client. It consists of user registration, secure login, session management, and login history tracking

## Tech Stack

- **Language:** Python 3
- **Backend:** Flask
- **Database:** SQLite
- **Password Hashing:** Bcrypt
- **HTTP Client:** Requests library
- **Authentication:** UUID-based session tokens

## Components

**Backend (Flask Server)**
- REST API with 4 endpoints: `/signup`, `/login`, `/history`, `/health`
- In-memory session token management
- SQLite database integration
- Credential validation and password verification
- Login history tracking with IP addresses

**Frontend (Python CLI)**
- Interactive command-line interface
- User registration workflow
- Login and session token handling
- View login history
- Server health verification

**Database (SQLite)**
- `user` - username and user ID storage
- `password` - bcrypt password hashes with timestamps
- `history` - login attempts with IP and timestamp data
- Foreign key constraints enabled

## Dependencies

```
flask                    # Web framework
PyJWT                    # JWT utilities
mysql-connector-python   # Database connectors
bcrypt                   # Password hashing
requests                 # HTTP requests
flask_sqlalchemy         # Database ORM
```

## Setup & Installation

### 1. Clone Repository
```bash
git clone https://github.com/cysec-wht24/auther.git
cd auther
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Start Backend Server
```bash
cd backend/server
python app.py
```

Server runs on `http://127.0.0.1:5000`

### 4. Start Frontend Client (new terminal)
```bash
cd frontend/client
python cmd.py
```

## Usage

### Register New Account
```
Select: 1
Username: testuser
Password: (hidden input)
Retype Password: (hidden input)
Successfully created your account
```

### Login & View History
```
Select: 2
Username: testuser
Password: (hidden input)
History:
- {'ip_addr': '127.0.0.1', 'login_time': '2026-03-18 10:30:45.123456', 'logout_time': None}
```

## API Endpoints

### `POST /signup`
Create new user account

**Request:**
```json
{
  "username": "testuser",
  "password": "password123"
}
```

**Response:**
```json
{"message": "User Created"}
```

---

### `POST /login`
Authenticate and get session token

**Request:**
```json
{
  "username": "testuser",
  "password": "password123"
}
```

**Response:**
```json
{"session_token": "abc123-uuid-here"}
```

---

### `POST /history`
Get login history (requires valid session token)

**Request:**
```json
{
  "session_token": "abc123-uuid-here"
}
```

**Response:**
```json
{
  "history": [
    {
      "ip_addr": "127.0.0.1",
      "login_time": "2026-03-18 10:30:45.123456",
      "logout_time": null
    }
  ]
}
```

---

### `GET /health`
Server status check

**Response:**
```json
{"status": "ok"}
```
---

## Security Implementation

- **Password Hashing:** Bcrypt with 8 salt rounds
- **Session Tokens:** UUID-based with 60-second TTL
- **Query Safety:** Parameterized SQL queries (prevents SQL injection)
- **Input Validation:** JSON type checking
- **Database Integrity:** Foreign key constraints enabled
- **IP Logging:** Captures source IP on each login

## How It Works

### Authentication Flow

1. User registers with username and password
2. Password is hashed with bcrypt and stored in database
3. On login, password is verified against stored hash
4. UUID session token generated and stored in memory
5. Token expires after 60 seconds
6. Protected endpoints require valid token in request

### Database Schema

```sql
user (user_id, user_name)
password (pass_id, user_id, pass_hash, mfg_time)
history (history_id, user_id, ip_addr, login_time, logout_time)
```

## Future Enhancements

- Implement Kerberos authentication
- Add reverse proxy (nginx)
- Implement custom JWT tokens
- Add multi-factor authentication
- Implement proper logout with session cleanup
- Add rate limiting for brute-force protection
