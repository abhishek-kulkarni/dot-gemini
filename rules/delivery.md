# Delivery Standards

These rules govern how code is committed, verified, and shipped.

## Conventional Commits

All commits must follow the format: `<type>: <description>`

| Type | When to use |
|------|-------------|
| `feat` | New user-visible functionality |
| `fix` | Bug fix |
| `refactor` | Code restructuring, no behavior change |
| `docs` | Documentation only |
| `test` | Adding or updating tests only |
| `chore` | Build config, CI, deps, tooling |
| `perf` | Performance improvement, no behavior change |
| `ci` | CI/CD pipeline changes |

## Verification

- Run targeted verification for all touched areas before shipping.
- All tests must pass with **95%+ coverage** on new/modified code.
- No skipped or disabled tests in committed code.

## Test-First

- Prefer test-first changes for bug fixes and new functionality.
- Every production code change should have corresponding tests.

## Change Quality

- Keep changes self-contained, readable, and easy to revert.
- Plan before editing large features — use the planning workflow.
- Review for security before shipping — use the security checklist.
