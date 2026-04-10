# Python Security Standards

Python-specific security rules. These extend the common [security.md](../security.md).

## Secret Management

- Use environment variables via `os.environ` — raise on missing keys.
- Use `python-dotenv` for local development, with `.env` in `.gitignore`.
- Never hardcode secrets in source, config files, or default arguments.

```python
import os
from dotenv import load_dotenv

load_dotenv()

# GOOD — raises KeyError if missing (fail fast)
api_key = os.environ["OPENAI_API_KEY"]

# GOOD — explicit validation at startup
def validate_config() -> None:
    required = ["DATABASE_URL", "SECRET_KEY", "API_KEY"]
    missing = [k for k in required if k not in os.environ]
    if missing:
        raise EnvironmentError(f"Missing required env vars: {missing}")
```

## SQL Injection Prevention

- Always use parameterized queries — never string concatenation or f-strings.

```python
# BAD — SQL injection
cursor.execute(f"SELECT * FROM users WHERE email = '{email}'")

# GOOD — parameterized (raw SQL)
cursor.execute("SELECT * FROM users WHERE email = %s", (email,))

# GOOD — ORM (SQLAlchemy)
user = session.query(User).filter(User.email == email).first()

# GOOD — ORM (Django)
user = User.objects.filter(email=email).first()
```

## Input Validation

- Validate all input at API boundaries using schema-based validation.
- Use **Pydantic** for FastAPI, **Django Forms/Serializers** for Django.
- Never trust request data — validate types, ranges, and formats.

```python
# GOOD — Pydantic model validates automatically
from pydantic import BaseModel, EmailStr, conint

class CreateUserRequest(BaseModel):
    name: str
    email: EmailStr
    age: conint(ge=0, le=150)

# GOOD — FastAPI uses Pydantic for request validation
@app.post("/users")
async def create_user(request: CreateUserRequest) -> User:
    ...
```

## Authentication & Authorization

- Hash passwords with **bcrypt** or **argon2** — never store plaintext.
- Use constant-time comparison for tokens (`hmac.compare_digest`).
- Validate JWT tokens on every request — check expiry, issuer, audience.
- Implement rate limiting on auth endpoints.

```python
# BAD — plaintext comparison (timing attack)
if token == expected_token:
    ...

# GOOD — constant-time comparison
import hmac
if hmac.compare_digest(token, expected_token):
    ...
```

## Dependency Auditing

```bash
# Scan for known vulnerabilities
pip-audit

# Static security analysis
bandit -r src/

# Check for outdated packages
pip list --outdated
```

- Run `pip-audit` before every release.
- Run `bandit -r src/` in CI to catch common security issues.
- Pin dependency versions in `requirements.txt` or `pyproject.toml`.
- Review changelogs for security patches on updates.

## Dangerous Functions

Never use these with user-controlled input:

| Function | Risk | Alternative |
|----------|------|-------------|
| `eval()` | Arbitrary code execution | `ast.literal_eval()` for data |
| `exec()` | Arbitrary code execution | Structured dispatch/handlers |
| `pickle.loads()` | Arbitrary code execution | `json.loads()` for data |
| `subprocess.shell=True` | Command injection | `subprocess.run([...])` with list args |
| `os.system()` | Command injection | `subprocess.run([...])` |
| `yaml.load()` | Code execution | `yaml.safe_load()` |
| `__import__()` | Arbitrary module loading | Explicit imports |

## Error Scrubbing

- Never expose stack traces, file paths, or internal details to users.
- Log full details server-side, return sanitized messages to clients.

```python
# BAD — leaks internals
@app.exception_handler(Exception)
async def handler(request, exc):
    return JSONResponse({"error": str(exc)}, status_code=500)

# GOOD — scrubbed
@app.exception_handler(Exception)
async def handler(request, exc):
    logger.exception("Unhandled error")
    return JSONResponse({"error": "Internal server error"}, status_code=500)
```

## File Handling

- Validate file paths to prevent path traversal (`../../etc/passwd`).
- Use `pathlib.Path.resolve()` and check it's within the allowed directory.
- Validate file types and sizes before processing uploads.
- Never serve user-uploaded files directly — use a CDN or storage service.

```python
from pathlib import Path

UPLOAD_DIR = Path("/app/uploads").resolve()

def safe_path(filename: str) -> Path:
    target = (UPLOAD_DIR / filename).resolve()
    if not target.is_relative_to(UPLOAD_DIR):
        raise ValueError("Path traversal detected")
    return target
```
