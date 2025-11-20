# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

This is a Django 5.2.8 project for a reminder/subscription management system called "kula_reminder". The project is in early development stages with the basic Django structure in place but minimal implementation in the apps.

## Environment Setup

### Virtual Environment
The project uses a Python 3.12 virtual environment located at `/home/buggzy/kod3/remind_Me/venv/` (one directory up from the Django project root).

**Activate the virtual environment before running any commands:**
```bash
source ../venv/bin/activate
```

### Project Structure
```
kula_reminder/          # Django project root
├── manage.py           # Django management script
├── kula_reminder/      # Main project configuration
│   ├── settings.py     # Project settings
│   ├── urls.py         # Root URL configuration
│   ├── wsgi.py         # WSGI application
│   └── asgi.py         # ASGI application
├── reminders/          # Reminders app
├── users/              # User management app
├── subscriptions/      # Subscriptions app
├── payments/           # Payment processing app
└── importJob/          # Import job handling app
```

## Common Commands

### Running the Development Server
```bash
python3 manage.py runserver
```

### Database Operations
```bash
# Create migrations after model changes
python3 manage.py makemigrations

# Apply migrations to database
python3 manage.py migrate

# Create a superuser for admin access
python3 manage.py createsuperuser
```

### Testing
```bash
# Run all tests
python3 manage.py test

# Run tests for a specific app
python3 manage.py test reminders
python3 manage.py test users
python3 manage.py test subscriptions
python3 manage.py test payments
python3 manage.py test importJob

# Run a specific test
python3 manage.py test reminders.tests.TestClassName.test_method_name
```

### Django Shell
```bash
# Interactive Python shell with Django environment
python3 manage.py shell
```

### Static Files
```bash
# Collect static files (for production)
python3 manage.py collectstatic
```

### App Management
```bash
# Create a new Django app
python3 manage.py startapp app_name
```

## Architecture

### Application Structure
The project follows Django's standard app-based architecture with five distinct applications:

1. **reminders**: Core functionality for managing reminders
2. **users**: User account management and authentication
3. **subscriptions**: Subscription management for recurring reminders
4. **payments**: Payment processing for premium features
5. **importJob**: Batch import operations for reminder data

### Key Notes
- **Database**: Currently using SQLite3 (`db.sqlite3`) - suitable for development but should be changed to PostgreSQL/MySQL for production
- **Apps Not Registered**: The custom apps (reminders, users, subscriptions, payments, importJob) are created but NOT yet added to `INSTALLED_APPS` in `settings.py`. They need to be registered before use.
- **Models Empty**: All app models are currently empty templates - the data schema is not yet defined
- **No URL Routing**: Only the admin URL is configured; app-specific URLs need to be added to `kula_reminder/urls.py`
- **Settings**: Using development settings with `DEBUG=True` and exposed `SECRET_KEY` - these must be secured for production

### Configuration Location
- Main settings: `kula_reminder/settings.py`
- URL routing: `kula_reminder/urls.py`
- Database: SQLite file will be at project root as `db.sqlite3` after first migration

## Development Workflow

### Adding a New App to the Project
1. Ensure the app is created (already done for the five apps)
2. Add the app's AppConfig to `INSTALLED_APPS` in `settings.py`:
   ```python
   INSTALLED_APPS = [
       # ... existing apps ...
       'reminders.apps.RemindersConfig',
       'users.apps.UsersConfig',
       'subscriptions.apps.SubscriptionsConfig',
       'payments.apps.PaymentsConfig',
       'importJob.apps.ImportjobConfig',
   ]
   ```
3. Create models in the app's `models.py`
4. Run `python3 manage.py makemigrations` and `python3 manage.py migrate`
5. Register models in `admin.py` for admin interface access
6. Create URL patterns in app's `urls.py` and include in main `urls.py`

### Before Implementing Features
- Register all apps in `INSTALLED_APPS` before creating models
- Define the relationships between apps (e.g., users → reminders, subscriptions → payments)
- Consider the data model carefully since this affects reminders, subscriptions, and payments
- Use Django's built-in User model or extend it in the users app

### Python Usage
- Use `python3` command (not `python`) for all management commands
- Virtual environment is required for Django to be available
