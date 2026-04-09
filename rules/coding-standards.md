# Coding Standards

These rules apply to all code changes regardless of language or framework.

## Immutability

- Prefer immutable updates over in-place mutation.
- Create new objects/copies instead of modifying existing ones.
- Use language-appropriate immutable patterns:
  - JS/TS: spread operator, `map`/`filter` — not `push`/`splice`.
  - Dart: `copyWith()`, `const` constructors, `freezed` — not field reassignment.
  - Python: `tuple`, `frozenset`, `@dataclass(frozen=True)` — not in-place mutation.
  - Java: `List.of()`, `Map.of()`, `Collections.unmodifiable*()` — not mutable setters.

## Functions & Files

- Keep functions under 50 lines.
- Keep files under 800 lines.
- Split large files by responsibility into focused modules.
- Use early returns to avoid deep nesting (max 4 levels).

## Constants

- Never hardcode strings — extract to a constants file or use `const` at module level.
- Never hardcode magic numbers — use named constants.

## Input Validation

- Validate all user input at boundaries (API routes, form handlers, CLI args).
- Fail loudly with clear error messages instead of silently swallowing problems.

## Formatting

- Apply the code formatter **ONLY** to specific files modified during a task.
- Never run a global formatting reset unless explicitly requested.
- This preserves localized Git histories and avoids noisy diffs.

## Dependencies

- Prefer contained local implementations over adding new third-party runtime dependencies.
- Evaluate new dependencies for maintenance health before adding.
