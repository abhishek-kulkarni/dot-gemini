# Python Project Rules

This file should be copied to the root of your Python project. It is auto-loaded by Gemini and Antigravity when working in that project.

## Language-Specific Rules

Follow the Python-specific rules defined in these files:

- [Python Coding Standards](file:///Users/abhishekkulkarni/.gemini/rules/python/coding-standards.md) — PEP 8, type hints, comprehensions, async patterns.
- [Python Security](file:///Users/abhishekkulkarni/.gemini/rules/python/security.md) — SQL injection, dangerous functions, auth, dependency auditing.
- [Python Testing](file:///Users/abhishekkulkarni/.gemini/rules/python/testing.md) — pytest, fixtures, mocking, FastAPI & Django testing.

## Project Conventions

<!-- Customize these for your specific project -->

### Framework
<!-- Specify which framework this project uses -->
- Framework: `FastAPI` / `Django` / `Flask` / `None (library/CLI)`

### Database
<!-- Specify the ORM/database layer -->
- ORM: `SQLAlchemy` / `Django ORM` / `Tortoise` / `None`
- Database: `PostgreSQL` / `SQLite` / `MySQL`

### Dependency Management
<!-- Specify the package manager -->
- Manager: `poetry` / `pip` + `requirements.txt` / `pipenv` / `uv`

### Python Version
- Target: `3.10+`

### Testing
```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=term-missing

# Run specific markers
pytest -m unit
pytest -m "not slow"
```

### Formatting & Linting
```bash
# Format
ruff format .
# or: black .

# Lint
ruff check .
# or: flake8

# Type check
mypy src/
```

## Project-Specific Notes

<!-- Add any project-specific rules, patterns, or decisions here -->
<!-- Examples: -->
<!-- - All API routes must use Pydantic models for request/response -->
<!-- - Database migrations via Alembic, never raw SQL -->
<!-- - All services must accept dependencies via constructor injection -->
