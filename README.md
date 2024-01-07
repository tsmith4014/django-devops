# Pokedex Project with Django, Docker Compose, and Frontend Integration

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Docker Compose Setup](#docker-compose-setup)
4. [Setting Up Django Environment](#setting-up-django-environment)
5. [Creating Django Project and Application](#creating-django-project-and-application)
6. [Configuring PostgreSQL with Django](#configuring-postgresql-with-django)
7. [Creating and Managing Django Models](#creating-and-managing-django-models)
8. [Setting Up the Frontend](#setting-up-the-frontend)
9. [Running the Django Application](#running-the-django-application)
10. [Interacting with the Django Application](#interacting-with-the-django-application)
11. [External Resources](#external-resources)
12. [Assignments](#assignments)

## Introduction

This comprehensive README guides you through setting up a Django-based application (`pokemon_app`) with PostgreSQL database management via Docker Compose, and integrating frontend components for displaying Pokémon information.

## Prerequisites

- Python 3.x
- Django
- Docker and Docker Compose
- Basic understanding of Python virtual environments and pip

## Docker Compose Setup

Use Docker Compose for setting up the PostgreSQL database as defined in `docker-compose.yml`:

```yaml
version: "3"
services:
  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=pokedex_db
    ports:
      - "5454:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
volumes:
  postgres_data:
```

Run `docker-compose up -d` to start and `docker-compose down` to stop the service.

## Setting Up Django Environment

### Virtual Environments

Create a virtual environment to isolate dependencies:

```bash
# Create virtual environment
python -m venv <envname>

# Activate virtual environment
source <envname>/bin/activate  # Mac/Linux
<envname>/Scripts/activate  # Windows

# Deactivate the virtual environment
deactivate
```

### Installing Django

Install Django in the virtual environment:

```bash
python -m pip install --upgrade pip
pip install django
pip install "psycopg[binary]"

```

Record dependencies in `requirements.txt`:

```bash
pip freeze > requirements.txt
```

## Creating Django Project and Application

Start a Django project and application:

```bash
python -m django startproject pokedex_proj .
python manage.py startapp pokemon_app
```

Include `pokemon_app` in `INSTALLED_APPS` in `settings.py`.

## Configuring PostgreSQL with Django

Configure Django to use PostgreSQL in `settings.py`:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "pokedex_db",
        "USER": "postgres",
        "PASSWORD": "postgres",
        "HOST": "localhost",
        "PORT": 5454,
    }
}
```

## Creating and Managing Django Models

Define models in `pokemon_app/models.py` and apply migrations:

```python
class Pokemon(models.Model):
    name = models.CharField(max_length=255)
    level = models.IntegerField(default=1)

# Apply migrations
python manage.py makemigrations
python manage.py migrate
```

## Setting Up the Frontend

Create Django views and templates to display Pokémon data:

- Define views in `pokemon_app/views.py`.
- Set up URL patterns in `pokemon_app/urls.py`.
- Create HTML templates in the `templates` directory, using Django's template language for dynamic content.
- Configure static files and templates in `settings.py`:

  ```python
  STATIC_URL = '/static/'
  STATICFILES_DIRS = [ BASE_DIR / 'pokemon_app/static', ]
  STATIC_ROOT = BASE_DIR / 'static'
  TEMPLATES = [{... "DIRS": [os.path.join(BASE_DIR, 'templates')], ...}]
  ```

- Use HTML and CSS to design the frontend, ensuring it loads static files correctly.

## Running the Django Application

Start the Django server:

```bash
python manage.py runserver
```

## Interacting with the Django Application

Access the application at `http://localhost:8000/pokemon_app/` and manage it through the Django admin panel at `http://localhost:8000/admin/`. Add new Pokémon entries through the admin panel or Django shell.

#### Django Console

> While we can interact with our data using Postgres, more often we want to interact with our data using Python. We're going to use a console for our project that will pull in all our Python classes and allow us to query the database directly using Django's ORM.

```bash
python manage.py shell

from pokemon_app.models import Pokemon
```

> The shell will allow us to load in our models from Django. Once in the shell, we can create a new student.

```python
pikachu = Pokemon(name = 'Pikachu', level = 12)
pikachu.save()
```

## External Resources

- [Django Docs](https://docs.djangoproject.com/en/2.2/)
- [Django Models Intro Docs](https://docs.djangoproject.com/en/2.2/topics/db/models/)
- [Django Queries Cheat Sheet](https://github.com/chrisdl/Django-QuerySet-Cheatsheet)
- [Django Validators Resource](https://docs.djangoproject.com/en/2.2/ref/validators/)
- [Database Diagramer](https://www.quickdatabasediagrams.com/)
- [Django Template Language Docs](https://docs.djangoproject.com/en/2.2/ref/templates/language/)
- [Django Template Language Intro Docs](https://docs.djangoproject.com/en/2.2/topics/templates/)
- [Django Template Language Built-in Tags and Filters](https://docs.djangoproject.com/en/2.2/ref/templates/builtins/)

---
