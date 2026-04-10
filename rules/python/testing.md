# Python Testing Standards

Python-specific testing rules. These extend the common [testing.md](../testing.md).

## Coverage Requirement: 95%+

```bash
pytest --cov=src --cov-report=term-missing
```

Coverage below 95% on business logic is a **blocking** issue.

## Framework

Use **pytest** as the primary testing framework. Avoid `unittest` style classes unless integrating with legacy code.

## Test Organization

```
tests/
├── unit/
│   ├── test_models.py
│   ├── test_services.py
│   └── test_utils.py
├── integration/
│   ├── test_api.py
│   └── test_database.py
├── e2e/
│   └── test_workflows.py
├── conftest.py           # shared fixtures
└── factories.py          # test data factories
```

## Test Markers

Use `pytest.mark` for categorization:

```python
import pytest

@pytest.mark.unit
def test_calculate_total():
    assert calculate_total([10, 20, 30]) == 60

@pytest.mark.integration
def test_create_user_in_database(db_session):
    user = create_user(db_session, name="Jane")
    assert db_session.query(User).count() == 1

@pytest.mark.slow
def test_bulk_import_performance(large_dataset):
    result = bulk_import(large_dataset)
    assert len(result.errors) == 0
```

Run selectively: `pytest -m unit`, `pytest -m "not slow"`.

## Fixtures

```python
import pytest
from app.models import User

@pytest.fixture
def user() -> User:
    return User(name="Jane", email="jane@example.com")

@pytest.fixture
def db_session():
    session = create_test_session()
    yield session
    session.rollback()
    session.close()

# Shared fixtures go in conftest.py
# Scope appropriately: function (default), class, module, session
@pytest.fixture(scope="session")
def database():
    db = setup_test_database()
    yield db
    teardown_test_database(db)
```

- Use `yield` fixtures for setup/teardown.
- Keep fixture scope as narrow as possible (prefer `function`).
- Put shared fixtures in `conftest.py` at the appropriate directory level.

## Mocking

```python
from unittest.mock import patch, MagicMock, AsyncMock

# Patch at the usage site, not the definition site
@patch("app.services.user_service.send_email")
def test_signup_sends_welcome_email(mock_send):
    create_user(name="Jane", email="jane@example.com")
    mock_send.assert_called_once_with("jane@example.com", subject="Welcome")

# Async mocking
@patch("app.services.api_client.fetch", new_callable=AsyncMock)
async def test_fetch_data(mock_fetch):
    mock_fetch.return_value = {"status": "ok"}
    result = await get_data()
    assert result["status"] == "ok"

# Context manager style
def test_with_mock():
    with patch("app.config.get_setting", return_value="test_value"):
        assert get_setting("key") == "test_value"
```

**Rules:**
- Patch at the **import site** (`app.services.user_service.send_email`), not the module where it's defined.
- Use `AsyncMock` for async functions.
- Prefer dependency injection over mocking where possible.

## Parametrize

```python
import pytest

@pytest.mark.parametrize("input_val,expected", [
    ("hello", "HELLO"),
    ("", ""),
    ("Hello World", "HELLO WORLD"),
    ("123", "123"),
])
def test_to_upper(input_val: str, expected: str):
    assert to_upper(input_val) == expected

# Multiple parameters
@pytest.mark.parametrize("status,should_retry", [
    (429, True),
    (500, True),
    (503, True),
    (400, False),
    (401, False),
    (200, False),
])
def test_should_retry(status: int, should_retry: bool):
    assert is_retryable(status) == should_retry
```

## FastAPI Testing

```python
from fastapi.testclient import TestClient
from app.main import app

client = TestClient(app)

def test_get_users():
    response = client.get("/users")
    assert response.status_code == 200
    assert isinstance(response.json(), list)

def test_create_user_validates_email():
    response = client.post("/users", json={"name": "Jane", "email": "invalid"})
    assert response.status_code == 422

# Async client (httpx)
import httpx
import pytest

@pytest.mark.anyio
async def test_async_endpoint():
    async with httpx.AsyncClient(app=app, base_url="http://test") as client:
        response = await client.get("/health")
        assert response.status_code == 200
```

## Django Testing

```python
from django.test import TestCase, Client
from app.models import User

class UserAPITest(TestCase):
    def setUp(self):
        self.client = Client()
        self.user = User.objects.create(name="Jane", email="jane@example.com")

    def test_get_user(self):
        response = self.client.get(f"/api/users/{self.user.id}/")
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.json()["name"], "Jane")

    def test_create_user_requires_email(self):
        response = self.client.post("/api/users/", {"name": "No Email"})
        self.assertEqual(response.status_code, 400)
```

## Factory Pattern

Use `factory_boy` or plain factories for test data:

```python
from dataclasses import dataclass
from typing import Any

@dataclass(frozen=True)
class UserFactory:
    @staticmethod
    def build(**overrides: Any) -> User:
        defaults = {
            "name": "Jane Doe",
            "email": "jane@example.com",
            "is_active": True,
        }
        return User(**(defaults | overrides))

# Usage
user = UserFactory.build(name="Custom Name")
inactive = UserFactory.build(is_active=False)
```

## Async Testing

```python
import pytest

@pytest.mark.anyio
async def test_async_service():
    result = await fetch_data()
    assert result is not None

# Or with pytest-asyncio
@pytest.mark.asyncio
async def test_concurrent_requests():
    results = await asyncio.gather(
        fetch_user(1),
        fetch_user(2),
        fetch_user(3),
    )
    assert len(results) == 3
```

## Anti-Patterns

| Anti-Pattern | Do This Instead |
|-------------|-----------------|
| `unittest.TestCase` subclass for simple tests | Use plain `def test_*` functions with pytest |
| `self.assertEqual(a, b)` | `assert a == b` (pytest gives better diffs) |
| Mocking everything | Use dependency injection, mock only external I/O |
| No fixture cleanup | Use `yield` fixtures with teardown |
| Hardcoded test data everywhere | Create shared factories in `tests/factories.py` |
| Testing private methods directly | Test through public API |
| `time.sleep()` in tests | Use `fake_async` or explicit event waiting |
