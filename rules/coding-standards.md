# Coding Standards

These rules apply to all code changes regardless of language or framework.

## Core Principles

- **KISS** — Prefer the simplest solution that works. Optimize for clarity over cleverness.
- **DRY** — Extract repeated logic into shared utilities. Introduce abstractions when repetition is real, not speculative.
- **YAGNI** — Do not build features or abstractions before they are needed. Start simple, refactor when the pressure is real.

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
- Keep files under 800 lines (200–400 typical).
- Split large files by responsibility into focused modules.
- Organize by feature/domain, not by file type.
- Use early returns to avoid deep nesting (max 4 levels).

## Constants

- Never hardcode strings — extract to a constants file or use `const` at module level.
- Never hardcode magic numbers — use named constants.

## Error Handling

- Handle errors explicitly at every level — never silently swallow them.
- Provide user-friendly error messages in UI-facing code.
- Log detailed error context on the server side.
- Fail fast with clear messages instead of continuing in a bad state.

## Input Validation

- Validate all user input at boundaries (API routes, form handlers, CLI args).
- Use schema-based validation where available (Zod, Pydantic, `@Valid`, etc.).
- Never trust external data — API responses, user input, file content.

## Formatting

- Apply the code formatter **ONLY** to specific files modified during a task.
- Never run a global formatting reset unless explicitly requested.
- This preserves localized Git histories and avoids noisy diffs.

## Dependencies

- Prefer contained local implementations over adding new third-party runtime dependencies.
- Evaluate new dependencies for maintenance health before adding.

## Naming Conventions

- Variables and functions: `camelCase` with descriptive names.
- Booleans: prefer `is`, `has`, `should`, or `can` prefixes.
- Interfaces, types, and components: `PascalCase`.
- Constants: `UPPER_SNAKE_CASE`.
- Follow language-specific conventions where they differ (e.g., Python `snake_case` for functions).

## Quality Checklist

Before marking work complete:

- [ ] Code is readable and well-named
- [ ] Functions are small (<50 lines)
- [ ] Files are focused (<800 lines)
- [ ] No deep nesting (>4 levels)
- [ ] Proper error handling at every level
- [ ] No hardcoded values (use constants or config)
- [ ] No mutation (immutable patterns used)
