Django

```
django-enterprise/
├── manage.py                      # Django management commands entry point
├── requirements/
│   ├── base.txt                   # Common dependencies (Django, DRF, etc.)
│   ├── development.txt            # Dev dependencies (debug-toolbar, pytest)
│   ├── production.txt             # Production dependencies (gunicorn, sentry)
│   └── testing.txt                # Testing dependencies (factory-boy, coverage)
├── config/                        # Project configuration (renamed from project name)
│   ├── __init__.py               # Makes config a package
│   ├── settings/
│   │   ├── __init__.py           # Makes settings a package
│   │   ├── base.py               # Common settings for all environments
│   │   ├── development.py        # Development-specific settings (DEBUG=True)
│   │   ├── production.py         # Production settings (DEBUG=False, security)
│   │   ├── testing.py            # Test settings (in-memory DB, fast tests)
│   │   └── local.py.example      # Local overrides template (gitignored)
│   ├── urls.py                   # Root URL configuration
│   ├── wsgi.py                   # WSGI application for deployment
│   ├── asgi.py                   # ASGI application for async/websockets
│   └── celery.py                 # Celery configuration for background tasks
├── apps/                         # All Django apps go here
│   ├── __init__.py              # Makes apps a package
│   ├── common/                  # Shared utilities across apps
│   │   ├── __init__.py          # Makes common a package
│   │   ├── models.py            # Abstract base models (TimeStampedModel)
│   │   ├── mixins.py            # Reusable model/view mixins
│   │   ├── permissions.py       # Custom DRF permissions
│   │   ├── pagination.py        # Custom pagination classes
│   │   ├── exceptions.py        # Custom exception classes
│   │   ├── validators.py        # Custom field validators
│   │   └── utils.py             # Helper functions
│   ├── users/                   # User management app
│   │   ├── __init__.py          # Makes users a package
│   │   ├── models.py            # User model and related models
│   │   ├── admin.py             # Django admin configuration
│   │   ├── apps.py              # App configuration
│   │   ├── views.py             # API views (DRF ViewSets)
│   │   ├── serializers.py       # DRF serializers for validation
│   │   ├── urls.py              # URL routing for this app
│   │   ├── permissions.py       # App-specific permissions
│   │   ├── filters.py           # Django-filter configurations
│   │   ├── services.py          # Business logic layer
│   │   ├── tasks.py             # Celery background tasks
│   │   ├── signals.py           # Django signals handlers
│   │   ├── managers.py          # Custom model managers
│   │   ├── migrations/          # Database migration files
│   │   └── tests/
│   │       ├── __init__.py      # Makes tests a package
│   │       ├── test_models.py   # Model tests
│   │       ├── test_views.py    # View/API tests
│   │       ├── test_services.py # Service layer tests
│   │       └── factories.py     # Factory Boy test data factories
│   ├── authentication/          # Auth and JWT handling
│   │   ├── __init__.py
│   │   ├── models.py            # Custom auth models (refresh tokens)
│   │   ├── views.py             # Login, logout, refresh endpoints
│   │   ├── serializers.py       # Auth request/response serializers
│   │   ├── urls.py              # Auth URLs
│   │   ├── services.py          # Auth business logic
│   │   ├── permissions.py       # Auth-related permissions
│   │   ├── middleware.py        # Custom auth middleware
│   │   ├── backends.py          # Custom authentication backends
│   │   └── tests/
│   ├── core/                    # Core business logic app
│   │   ├── __init__.py
│   │   ├── models.py            # Core business models
│   │   ├── views.py             # Core API endpoints
│   │   ├── serializers.py       # Core serializers
│   │   ├── urls.py              # Core URLs
│   │   ├── services.py          # Core business services
│   │   └── tests/
│   └── health/                  # Health check and monitoring
│       ├── __init__.py
│       ├── views.py             # Health check endpoints
│       ├── urls.py              # Health URLs
│       └── checks.py            # Custom health checks
├── static/                      # Static files (CSS, JS, images)
│   ├── css/
│   ├── js/
│   └── images/
├── media/                       # User uploaded files
├── templates/                   # Django templates (if using template rendering)
│   ├── base.html
│   └── emails/                  # Email templates
├── locale/                      # Internationalization files
│   ├── en/
│   └── es/
├── docker/
│   ├── Dockerfile               # Main application container
│   ├── docker-compose.yml       # Development environment
│   ├── docker-compose.prod.yml  # Production environment
│   ├── nginx/
│   │   ├── Dockerfile           # Nginx container for static files
│   │   └── nginx.conf           # Nginx configuration
│   └── entrypoint.sh            # Container startup script
├── scripts/
│   ├── start.sh                 # Start development server
│   ├── test.sh                  # Run tests with coverage
│   ├── migrate.sh               # Run migrations
│   ├── collectstatic.sh         # Collect static files
│   └── backup.sh                # Database backup script
├── docs/                        # Project documentation
│   ├── api.md                   # API documentation
│   ├── deployment.md            # Deployment guide
│   └── development.md           # Development setup
├── logs/                        # Application logs (gitignored)
├── .env.example                 # Environment variables template
├── .gitignore                   # Git ignore file
├── pytest.ini                  # Pytest configuration
├── pyproject.toml              # Project metadata and tool configs
├── Makefile                    # Common commands shortcuts
└── README.md                   # Project overview and setup
```

# Django Project Structure - File Purpose & Usage

## Root Level Files
```
├── manage.py                     # Django CLI tool (runserver, migrate, shell, etc.)
├── requirements/
│   ├── base.txt                  # Core dependencies (Django, DRF, psycopg2)
│   ├── development.txt           # Dev tools (django-debug-toolbar, ipdb)
│   ├── production.txt            # Production deps (gunicorn, sentry-sdk)
│   └── testing.txt               # Test tools (pytest-django, factory-boy)
├── .env.example                  # Environment variables template
├── pytest.ini                   # Pytest configuration (Django settings, markers)
├── pyproject.toml               # Project metadata, black/isort configuration
├── Makefile                     # Shortcuts for common commands
└── README.md                    # Setup instructions and project overview
```

## Config Directory (Project Settings)
```
├── config/                       # Main project configuration (replaces 'myproject')
│   ├── __init__.py              # Makes config a package
│   ├── settings/
│   │   ├── __init__.py          # Makes settings a package
│   │   ├── base.py              # Shared settings (INSTALLED_APPS, MIDDLEWARE)
│   │   ├── development.py       # Dev settings (DEBUG=True, dev database)
│   │   ├── production.py        # Prod settings (security, performance)
│   │   ├── testing.py           # Test settings (in-memory DB, no emails)
│   │   └── local.py.example     # Local overrides template (personal settings)
│   ├── urls.py                  # Root URL patterns (admin, API routes)
│   ├── wsgi.py                  # WSGI application for web servers (Apache, Nginx)
│   ├── asgi.py                  # ASGI application for async (WebSockets, channels)
│   └── celery.py                # Celery configuration for background tasks
```

## Apps Directory Structure

### Common App (`apps/common/`)
```
├── apps/
│   ├── __init__.py              # Makes apps a package
│   ├── common/                  # Shared utilities across all apps
│   │   ├── __init__.py          # Makes common a package
│   │   ├── models.py            # Abstract base models (TimeStampedModel)
│   │   ├── mixins.py            # Reusable model/view mixins
│   │   ├── permissions.py       # Custom DRF permissions (IsOwnerOrAdmin)
│   │   ├── pagination.py        # Custom pagination with metadata
│   │   ├── exceptions.py        # Custom exception classes and handlers
│   │   ├── validators.py        # Custom field validators (phone, email)
│   │   └── utils.py             # Helper functions (date formatting, etc.)
```

### User Management App (`apps/users/`)
```
│   ├── users/                   # User management functionality
│   │   ├── __init__.py          # Makes users a package
│   │   ├── models.py            # User model extending AbstractUser
│   │   ├── admin.py             # Django admin interface configuration
│   │   ├── apps.py              # App configuration (signals, ready method)
│   │   ├── views.py             # DRF ViewSets for user CRUD operations
│   │   ├── serializers.py       # Request/response validation (UserSerializer)
│   │   ├── urls.py              # URL patterns for user endpoints
│   │   ├── permissions.py       # User-specific permissions
│   │   ├── filters.py           # Django-filter configurations
│   │   ├── services.py          # Business logic (email verification, etc.)
│   │   ├── tasks.py             # Celery background tasks (send emails)
│   │   ├── signals.py           # Django signals (post_save, pre_delete)
│   │   ├── managers.py          # Custom model managers (active users)
│   │   ├── migrations/          # Database schema changes
│   │   └── tests/
│   │       ├── __init__.py      # Makes tests a package
│   │       ├── test_models.py   # Model validation and methods tests
│   │       ├── test_views.py    # API endpoint tests
│   │       ├── test_services.py # Business logic tests
│   │       └── factories.py     # Factory Boy for test data generation
```

### Authentication App (`apps/authentication/`)
```
│   ├── authentication/          # JWT auth, login/logout functionality
│   │   ├── __init__.py
│   │   ├── models.py            # RefreshToken, LoginHistory models
│   │   ├── views.py             # Login, logout, refresh token endpoints
│   │   ├── serializers.py       # LoginSerializer, TokenSerializer
│   │   ├── urls.py              # Auth URL patterns (/login, /refresh)
│   │   ├── services.py          # Auth business logic (token generation)
│   │   ├── permissions.py       # Auth-related permissions
│   │   ├── middleware.py        # Custom auth middleware (IP tracking)
│   │   ├── backends.py          # Custom authentication backends
│   │   └── tests/
```

### Core Business App (`apps/core/`)
```
│   ├── core/                    # Main business logic
│   │   ├── __init__.py
│   │   ├── models.py            # Core business models (Product, Order, etc.)
│   │   ├── views.py             # Core API endpoints
│   │   ├── serializers.py       # Core serializers
│   │   ├── urls.py              # Core URL patterns
│   │   ├── services.py          # Core business services
│   │   └── tests/
```

### Health Check App (`apps/health/`)
```
│   └── health/                  # System monitoring and health checks
│       ├── __init__.py
│       ├── views.py             # Health endpoints (/health, /readiness)
│       ├── urls.py              # Health URL patterns
│       └── checks.py            # Custom health checks (DB, Redis, external APIs)
```

## Static and Media Files
```
├── static/                      # Static assets (CSS, JS, images)
│   ├── css/                     # Stylesheet files
│   ├── js/                      # JavaScript files
│   └── images/                  # Static images
├── media/                       # User uploaded files (profile pics, documents)
├── templates/                   # Django templates (if using template rendering)
│   ├── base.html               # Base template with common layout
│   └── emails/                 # Email templates (welcome, password reset)
```

## Internationalization & Documentation
```
├── locale/                      # Translation files for i18n
│   ├── en/                     # English translations
│   └── es/                     # Spanish translations
├── docs/                       # Project documentation
│   ├── api.md                  # API endpoint documentation
│   ├── deployment.md           # Deployment instructions
│   └── development.md          # Development setup guide
```

## Docker and Scripts
```
├── docker/
│   ├── Dockerfile              # Main application container build instructions
│   ├── docker-compose.yml      # Development environment (Django + Postgres + Redis)
│   ├── docker-compose.prod.yml # Production environment configuration
│   ├── nginx/
│   │   ├── Dockerfile          # Nginx container for serving static files
│   │   └── nginx.conf          
