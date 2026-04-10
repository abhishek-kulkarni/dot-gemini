# Python Coding Standards

Python-specific rules. These extend the common [coding-standards.md](file:///Users/abhishekkulkarni/.gemini/rules/coding-standards.md).

## Standards

- Follow **PEP 8** conventions.
- Use **type annotations** on all function signatures and return types.
- Target Python 3.10+ for pattern matching and modern syntax.

## Formatting & Linting

- **black** — code formatting (or **ruff format**).
- **isort** — import sorting (or **ruff** with isort rules).
- **ruff** — linting (replaces flake8, pylint, isort, pyupgrade in one tool).
- **mypy** — static type checking with `--strict` where possible.

Apply the formatter **only** to files modified during the task — not globally.

## Naming

Follow PEP 8 conventions:

- `snake_case` — variables, functions, methods, modules.
- `PascalCase` — classes, exceptions, type aliases.
- `SCREAMING_SNAKE_CASE` — module-level constants.
- `_prefix` — private/internal names (single underscore).
- `__dunder__` — reserved for Python special methods only.

## Immutability

Prefer immutable data structures:

```python
# GOOD — frozen dataclass
from dataclasses import dataclass

@dataclass(frozen=True)
class User:
    name: str
    email: str

# GOOD — NamedTuple
from typing import NamedTuple

class Point(NamedTuple):
    x: float
    y: float

# GOOD — Pydantic with frozen
from pydantic import BaseModel

class Config(BaseModel, frozen=True):
    host: str
    port: int
```

- Use `tuple` over `list` for immutable sequences.
- Use `frozenset` over `set` when mutation is not needed.
- Use `MappingProxyType` for read-only dicts in public APIs.

## Type Annotations

```python
# BAD — no type hints
def get_user(id):
    ...

# GOOD — fully annotated
def get_user(user_id: int) -> User | None:
    ...

# GOOD — complex types
from collections.abc import Sequence

def process_items(items: Sequence[Item]) -> list[Result]:
    ...
```

- Use `X | None` instead of `Optional[X]` (Python 3.10+).
- Use `collections.abc` types (`Sequence`, `Mapping`) for function parameters over concrete types.
- Use `TypeAlias` or `type` statement for complex type expressions.

## Error Handling

```python
# BAD — bare except
try:
    fetch_data()
except:
    pass

# BAD — broad catch
try:
    fetch_data()
except Exception:
    log("error")

# GOOD — specific exceptions
try:
    fetch_data()
except ConnectionError as e:
    logger.error("Network failure: %s", e)
    raise ServiceUnavailableError from e
except TimeoutError:
    logger.warning("Request timed out, retrying")
    retry()
```

- Never use bare `except:` — always specify exception types.
- Use `raise ... from e` to chain exceptions and preserve tracebacks.
- Use custom exception classes for domain errors.
- Never use exceptions for flow control.

## Imports

Order (enforced by isort/ruff):
1. Standard library (`os`, `sys`, `json`)
2. Third-party packages (`fastapi`, `pydantic`, `sqlalchemy`)
3. Local modules (`from app.models import User`)

```python
# BAD
from os import *
import app.models, app.services

# GOOD
import os
from collections.abc import Sequence

from fastapi import APIRouter, Depends

from app.models import User
from app.services import UserService
```

- No wildcard imports (`from x import *`).
- No multi-import statements (`import a, b`).
- Prefer absolute imports over relative imports.

## Comprehensions & Generators

```python
# BAD — manual loop to build list
result = []
for item in items:
    if item.active:
        result.append(item.name)

# GOOD — list comprehension
result = [item.name for item in items if item.active]

# GOOD — generator for large datasets
result = (item.name for item in items if item.active)
```

- Use comprehensions for simple transformations.
- Use generators for large datasets to avoid memory issues.
- Don't nest comprehensions more than 2 levels deep — extract to a function.

## Context Managers

```python
# BAD — manual resource management
f = open("data.txt")
data = f.read()
f.close()

# GOOD — context manager
with open("data.txt") as f:
    data = f.read()

# GOOD — custom context manager
from contextlib import contextmanager

@contextmanager
def database_transaction(db):
    try:
        yield db
        db.commit()
    except Exception:
        db.rollback()
        raise
```

## Async

- Use `async`/`await` consistently — don't mix sync and async code paths.
- Use `asyncio.gather()` for concurrent operations.
- Always set timeouts on async operations (`asyncio.wait_for`).
- Use `httpx` (async-native) over `requests` in async code.

## Quality Checklist

Before marking Python work complete:

- [ ] All functions have type annotations
- [ ] `ruff check` passes with no errors
- [ ] `mypy` passes (or type errors are explicitly documented)
- [ ] No bare `except` or `except Exception`
- [ ] No wildcard imports
- [ ] Immutable data structures used where possible
- [ ] Docstrings on all public functions and classes
