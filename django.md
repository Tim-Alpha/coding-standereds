# Django Project Structure with Template Rendering and Static Serving via Nginx)

```
django-social-app/
├── manage.py                     # Django CLI tool
├── requirements/                # Environment-specific dependencies
│   ├── base.txt
│   ├── development.txt
│   ├── production.txt
│   └── testing.txt
├── config/                      # Main project configuration
│   ├── __init__.py
│   ├── settings/                # Environment settings split
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── development.py
│   │   ├── production.py
│   │   ├── testing.py
│   │   └── local.py.example
│   ├── urls.py                  # Root URL dispatcher
│   ├── wsgi.py                  # WSGI app for prod
│   ├── asgi.py                  # ASGI app for async
│   └── celery.py                # Celery app definition
├── apps/                        # All Django apps
│   ├── __init__.py
│   ├── common/                 # Shared logic and utilities
│   │   ├── models.py
│   │   ├── mixins.py
│   │   ├── utils.py
│   │   ├── pagination.py
│   │   ├── permissions.py
│   │   ├── exceptions.py
│   │   ├── validators.py
│   │   └── tests/
│   ├── users/                  # User app (auth, profiles, follow)
│   │   ├── models.py
│   │   ├── views.py
│   │   ├── urls.py
│   │   ├── serializers.py
│   │   ├── services.py
│   │   ├── tasks.py
│   │   ├── admin.py
│   │   ├── managers.py
│   │   └── tests/
│   ├── posts/                  # Posts (images, captions, likes)
│   │   ├── models.py
│   │   ├── views.py
│   │   ├── urls.py
│   │   ├── serializers.py
│   │   └── tests/
│   ├── comments/               # Comments and replies
│   ├── authentication/         # JWT login, logout, refresh
│   ├── health/                 # Health check endpoints
│   └── api_v1/                 # API versioning root app
│       ├── urls.py             # Include all v1 app routes
├── static/                     # Static files (CSS, JS, images)
│   ├── css/
│   ├── js/
│   └── images/
├── media/                      # User uploaded content (avatars, posts)
├── templates/                  # HTML templates (server-rendered pages)
│   ├── base.html
│   ├── users/
│   ├── posts/
│   └── emails/
├── docker/
│   ├── Dockerfile              # Application container
│   ├── docker-compose.yml      # Local dev setup
│   ├── docker-compose.prod.yml # Production setup
│   ├── nginx/
│   │   ├── Dockerfile
│   │   └── nginx.conf          # Serve static & media files
│   └── entrypoint.sh
├── scripts/                    # Automation scripts
│   ├── start.sh
│   ├── migrate.sh
│   ├── collectstatic.sh
│   └── backup.sh
├── docs/                       # Developer docs
│   ├── api.md
│   ├── deployment.md
│   └── development.md
├── logs/                       # Log files (rotated, gitignored)
├── .env.example                # Env variable template
├── .gitignore
├── pytest.ini                  # Pytest config
├── pyproject.toml              # Tool config (Black, isort)
├── Makefile                    # CLI shortcuts
└── README.md                   # Project overview and setup
```

# Django Project Structure - File Purpose & Usage
---

## Root-Level Structure

```
django-enterprise/
├── manage.py                     # Django CLI entry point
├── .env.example                  # Environment variables template
├── pytest.ini                   # Pytest configuration
├── pyproject.toml               # Project metadata & tool configs
├── Makefile                     # Shortcuts for CLI (migrate, test, etc.)
├── README.md                    # Project overview & setup guide
├── requirements/                # Organized Python dependencies
│   ├── base.txt                 # Common dependencies (Django, DRF, psycopg2)
│   ├── development.txt          # Dev-only tools (debug-toolbar, ipdb)
│   ├── production.txt           # Prod-only tools (gunicorn, sentry)
│   └── testing.txt              # Testing tools (pytest, coverage)
```

---

## Project Configuration (`config/`)

```
config/
├── __init__.py
├── urls.py                      # Root URL routing
├── wsgi.py                      # WSGI app for Gunicorn/Apache
├── asgi.py                      # ASGI app for WebSockets
├── celery.py                    # Celery worker setup
└── settings/
    ├── __init__.py              # Settings module package
    ├── base.py                  # Shared settings (apps, middleware, DB)
    ├── development.py           # Dev settings (DEBUG=True, local DB)
    ├── production.py            # Prod settings (secure configs)
    ├── testing.py               # Testing configs (in-memory DB)
    └── local.py.example         # Local override (personal developer configs)
```

---

## Modular Apps (`apps/`)

### `apps/common/` — Shared Utilities

```
common/
├── __init__.py
├── models.py                   # Abstract models (e.g. TimeStampedModel)
├── mixins.py                   # Reusable view/model mixins
├── permissions.py              # Custom DRF permissions
├── pagination.py               # Custom pagination classes
├── exceptions.py               # Global exception handlers
├── validators.py               # Custom field validators
└── utils.py                    # General helper methods
```

### `apps/users/` — User Management

```
users/
├── __init__.py
├── models.py                   # Custom user model
├── admin.py                    # Admin registration
├── apps.py                     # App config
├── views.py                    # DRF ViewSets for user operations
├── serializers.py              # Request/response handling
├── urls.py                     # Routes (e.g. /users/)
├── permissions.py
├── filters.py
├── services.py                 # Business logic (email verify, etc.)
├── tasks.py                    # Celery tasks
├── signals.py
├── managers.py
├── migrations/
└── tests/
    ├── __init__.py
    ├── test_models.py
    ├── test_views.py
    ├── test_services.py
    └── factories.py
```

### `apps/authentication/` — Auth (JWT)

```
authentication/
├── __init__.py
├── models.py                   # Login sessions, refresh tokens
├── views.py                    # Login, logout, token refresh
├── serializers.py              # Auth serializers
├── urls.py                     # Auth routes (/auth/login, etc.)
├── services.py
├── permissions.py
├── middleware.py               # IP logging, session checks
├── backends.py                 # Custom auth backends
└── tests/
```

### `apps/core/` — Social Media Core Logic

```
core/
├── __init__.py
├── models.py                   # Posts, comments, likes
├── views.py
├── serializers.py
├── urls.py
├── services.py
└── tests/
```

### `apps/health/` — Monitoring & Readiness

```
health/
├── __init__.py
├── views.py                    # /health, /readiness endpoints
├── urls.py
└── checks.py                   # DB, Redis, external API checks
```

---

## API Versioning: `apps/api_v1/`

```
api_v1/
├── __init__.py
├── urls.py                     # Root router for API v1 (/api/v1/)
└── views/
    ├── users.py                # User API handlers
    ├── posts.py                # Post-related APIs
    └── comments.py             # Comment APIs
```

> Add in `config/urls.py`:

```python
path("api/v1/", include("apps.api_v1.urls")),
```

---

## Templates, Static, and Media

```
├── templates/                  # Django templates (Jinja2 or HTML)
│   ├── base.html
│   └── emails/
│       └── welcome.html
├── static/                     # Static assets for dev (CSS, JS, images)
│   ├── css/
│   ├── js/
│   └── images/
├── media/                      # Uploaded media (avatars, images)
├── staticfiles/                # Collected static files (prod only)
├── mediafiles/                 # Prod media mount location
```

---

## Docker Setup

```
docker/
├── Dockerfile                  # Django container (Gunicorn)
├── docker-compose.yml          # Local stack (Django + DB + Redis)
├── docker-compose.prod.yml     # Prod stack
├── entrypoint.sh               # Entrypoint to run collectstatic, migrate
└── nginx/
    ├── Dockerfile              # Nginx container (static/media)
    └── nginx.conf              # Nginx config for serving Django + static files
```

### docker/nginx/nginx.conf

```nginx
server {
    listen 80;

    location /static/ {
        alias /app/staticfiles/;
        expires 30d;
    }

    location /media/ {
        alias /app/mediafiles/;
    }

    location / {
        proxy_pass http://web:8000;
        include /etc/nginx/proxy_params;
    }
}
```

### docker-compose.prod.yml (volumes)

```yaml
web:
  volumes:
    - ./staticfiles:/app/staticfiles
    - ./mediafiles:/app/mediafiles

nginx:
  volumes:
    - ./staticfiles:/app/staticfiles:ro
    - ./mediafiles:/app/mediafiles:ro
```

---

## Scripts (Automation)

```
scripts/
├── start.sh                    # Start Django dev server
├── test.sh                     # Run tests
├── migrate.sh                  # Apply migrations
├── collectstatic.sh            # Run collectstatic
└── backup.sh                   # Dump PostgreSQL DB
```

---

## Local vs Production: Static & Media Config

### In `settings/development.py`

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "media"
```

```python
# config/urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATICFILES_DIRS[0])
```

### In `settings/production.py`

```python
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / "staticfiles"
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / "mediafiles"
```

Run before deploy:

```bash
python manage.py collectstatic
```


