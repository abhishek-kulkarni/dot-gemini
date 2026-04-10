# Python Build Resolution

Python specific static-checking fixes. These extend the common [build-resolution.md](file:///Users/abhishekkulkarni/.gemini/rules/build-resolution.md).

## Diagnostic Tool
- Always use `mypy src/` for typing issues. 
- Use `ruff check .` for static analysis and linting failures.

## Missing Types
- **Error:** `Missing type parameters for generic type`
- **Error:** `Function is missing a return type annotation`
  - **Minimal Fix:** Add the explicit type generics. For modern Python (3.10+), prefer built-in generics (`list[int]`, `dict[str, Any]`) over `typing.List`. Add `Any` only if deeply dynamic.

## Incompatible Types
- **Error:** `Argument 1 to "xyz" has incompatible type "A"; expected "B"`
  - **Minimal Fix:** Cast the type via `cast(B, var)` or dynamically convert it (e.g. `str(var)`). 
  - **Fallback:** Use `# type: ignore[error-code]` ONLY if the types are dynamically sound but technically impossible for mypy to trace.

## Imports
- **Error:** `ModuleNotFoundError: No module named 'X'`
  - **Minimal Fix:** Add the missing package to `requirements.txt` or your build system (e.g. `pip install X`). If the module is internal, ensure `__init__.py` markers exist and absolute imports are used.

## Syntax & Ruff Errors
- **Error:** Deprecation warnings or Ruff format errors.
  - **Minimal Fix:** Run `ruff format .` and `ruff check --fix .` on the specific broken files. 
