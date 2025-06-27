# FastAPI

```bash
fastapi-enterprise/
├── app/
│   ├── __init__.py
│   ├── main.py                    # Application entry point
│   ├── core/
│   │   ├── __init__.py
│   │   ├── config.py              # Configuration management
│   │   ├── security.py            # JWT, password hashing
│   │   ├── database.py            # Database connection
│   │   ├── exceptions.py          # Custom exceptions
│   │   ├── middleware.py          # Custom middleware
│   │   └── deps.py                # Dependencies
│   ├── api/
│   │   ├── __init__.py
│   │   ├── deps.py                # API dependencies
│   │   └── v1/
│   │       ├── __init__.py
│   │       ├── api.py             # Main API router
│   │       └── endpoints/
│   │           ├── __init__.py
│   │           ├── auth.py
│   │           ├── users.py
│   │           └── health.py
│   ├── models/
│   │   ├── __init__.py
│   │   ├── base.py                # Base model class
│   │   ├── user.py
│   │   └── auth.py
│   ├── schemas/
│   │   ├── __init__.py
│   │   ├── base.py                # Base schema classes
│   │   ├── user.py
│   │   ├── auth.py
│   │   └── response.py            # Standard response schemas
│   ├── services/
│   │   ├── __init__.py
│   │   ├── base.py                # Base service class
│   │   ├── user_service.py
│   │   └── auth_service.py
│   ├── utils/
│   │   ├── __init__.py
│   │   ├── logger.py              # Logging configuration
│   │   ├── validators.py
│   │   └── helpers.py
│   └── tests/
│       ├── __init__.py
│       ├── conftest.py
│       ├── test_auth.py
│       └── test_users.py
├── alembic/                       # Database migrations
│   ├── versions/
│   ├── env.py
│   ├── script.py.mako
│   └── alembic.ini
├── docker/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   ├── docker-compose.prod.yml
│   └── entrypoint.sh
├── scripts/
│   ├── start.sh
│   ├── test.sh
│   └── migrate.sh
├── .env.example
├── .gitignore
├── requirements.txt
├── requirements-dev.txt
├── pyproject.toml
└── README.md
```
# FastAPI Project Structure - File Purpose & Usage

## Root Level Files
```
├── .env.example              # Template for environment variables (copy to .env)
├── .gitignore               # Files/folders to ignore in git (node_modules, .env, __pycache__)
├── requirements.txt         # Production dependencies (FastAPI, SQLAlchemy, etc.)
├── requirements-dev.txt     # Development dependencies (pytest, black, flake8)
├── pyproject.toml          # Project metadata and tool configurations (poetry, black, isort)
└── README.md               # Project documentation and setup instructions
```

## App Directory Structure

### Core Application
```
├── app/
│   ├── __init__.py         # Makes app a Python package (usually empty)
│   ├── main.py            # FastAPI app creation, middleware setup, error handlers
```

### Core Infrastructure (`app/core/`)
```
│   ├── core/
│   │   ├── __init__.py    # Makes core a package
│   │   ├── config.py      # Environment settings (DB_URL, SECRET_KEY, CORS origins)
│   │   ├── security.py    # JWT token creation/validation, password hashing
│   │   ├── database.py    # SQLAlchemy engine, session maker, Base model
│   │   ├── exceptions.py  # Custom exception classes (ValidationError, NotFoundError)
│   │   ├── middleware.py  # Custom middleware (request logging, rate limiting)
│   │   └── deps.py        # Core dependencies (database session, settings)
```

### API Layer (`app/api/`)
```
│   ├── api/
│   │   ├── __init__.py    # Makes api a package
│   │   ├── deps.py        # API-specific dependencies (current user, permissions)
│   │   └── v1/
│   │       ├── __init__.py        # Makes v1 a package
│   │       ├── api.py             # Main router that includes all endpoint routers
│   │       └── endpoints/
│   │           ├── __init__.py    # Makes endpoints a package
│   │           ├── auth.py        # Login, logout, refresh token endpoints
│   │           ├── users.py       # User CRUD endpoints (GET, POST, PUT, DELETE)
│   │           └── health.py      # Health check endpoint for monitoring
```

### Data Layer (`app/models/`)
```
│   ├── models/
│   │   ├── __init__.py    # Makes models a package, imports all models
│   │   ├── base.py        # Base SQLAlchemy model with common fields (id, created_at)
│   │   ├── user.py        # User database model (SQLAlchemy ORM)
│   │   └── auth.py        # Auth-related models (refresh tokens, sessions)
```

### Validation Layer (`app/schemas/`)
```
│   ├── schemas/
│   │   ├── __init__.py    # Makes schemas a package
│   │   ├── base.py        # Base Pydantic schemas with common patterns
│   │   ├── user.py        # User request/response schemas (UserCreate, UserResponse)
│   │   ├── auth.py        # Auth schemas (LoginRequest, TokenResponse)
│   │   └── response.py    # Standard API response formats (success, error, pagination)
```

### Business Logic (`app/services/`)
```
│   ├── services/
│   │   ├── __init__.py        # Makes services a package
│   │   ├── base.py            # Base service class with common CRUD operations
│   │   ├── user_service.py    # User business logic (create user, validate email)
│   │   └── auth_service.py    # Authentication logic (login, password reset)
```

### Utilities (`app/utils/`)
```
│   ├── utils/
│   │   ├── __init__.py    # Makes utils a package
│   │   ├── logger.py      # Logging configuration and setup
│   │   ├── validators.py  # Custom validation functions (email, phone)
│   │   └── helpers.py     # General helper functions (date formatting, etc.)
```

### Testing (`app/tests/`)
```
│   └── tests/
│       ├── __init__.py    # Makes tests a package
│       ├── conftest.py    # Pytest configuration and shared fixtures
│       ├── test_auth.py   # Authentication endpoint tests
│       └── test_users.py  # User endpoint tests
```

## Database Migrations (`alembic/`)
```
├── alembic/
│   ├── versions/          # Directory containing migration files
│   ├── env.py            # Alembic environment configuration
│   ├── script.py.mako    # Template for generating migration files
│   └── alembic.ini       # Alembic configuration file
```

## Docker Configuration (`docker/`)
```
├── docker/
│   ├── Dockerfile             # Instructions to build app container
│   ├── docker-compose.yml     # Development environment (app + postgres + redis)
│   ├── docker-compose.prod.yml  # Production environment configuration
│   └── entrypoint.sh          # Container startup script (run migrations, start app)
```

## Scripts (`scripts/`)
```
├── scripts/
│   ├── start.sh          # Start the application (uvicorn with reload)
│   ├── test.sh           # Run all tests with coverage
│   └── migrate.sh        # Run database migrations
```

## Quick File Purpose Summary

| File Type | Purpose | Example |
|-----------|---------|---------|
| `main.py` | App entry point | Create FastAPI app, add middleware |
| `config.py` | Settings management | DB_URL, SECRET_KEY from environment |
| `models/*.py` | Database tables | SQLAlchemy ORM classes |
| `schemas/*.py` | Request/Response validation | Pydantic models for API |
| `services/*.py` | Business logic | User creation, email sending |
| `endpoints/*.py` | API routes | HTTP endpoints with decorators |
| `deps.py` | Dependency injection | Get current user, database session |
| `exceptions.py` | Custom errors | NotFoundError, ValidationError |

## Common File Patterns

### Empty `__init__.py` files
- Make directories Python packages
- Usually empty or contain imports

### `deps.py` files
- Contain FastAPI dependencies
- Used with `Depends()` in endpoints

### `base.py` files
- Contain base classes
- Shared functionality for models/services/schemas
