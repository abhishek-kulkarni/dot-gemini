# Rules

Common, language-agnostic rules that apply to all projects. These are referenced by `GEMINI.md` and enforced across all agents and workflows.

## Rule Files

| Rule | File | Scope |
|------|------|-------|
| [Coding Standards](coding-standards.md) | `coding-standards.md` | Immutability, function/file size limits, constants, formatting, dependencies |
| [Security Checklist](security.md) | `security.md` | Pre-commit security verification — secrets, injection, auth, error scrubbing |
| [Delivery Standards](delivery.md) | `delivery.md` | Conventional commits, 95%+ test coverage, test-first approach, change quality |
| [Code Review Standards](code-review.md) | `code-review.md` | When to review, severity levels, agent routing |
| [Testing Standards](testing.md) | `testing.md` | 95%+ coverage, TDD workflow, AAA patterns, edge cases |
| [Build Resolution](build-resolution.md) | `build-resolution.md` | Speed to green, minimal diffs, explicit typing |

## How Rules Are Loaded

1. `GEMINI.md` (auto-loaded every conversation) references these rule files.
2. Agents and workflows read relevant rules when performing their tasks.
3. Per-project `GEMINI.md` files add language-specific rules on top.

## Adding New Rules

1. Create a new `.md` file in this directory.
2. Add a reference in `~/.gemini/GEMINI.md` under the `## Rules` section.
3. Keep rules general and language-agnostic — language-specific rules belong in per-project `GEMINI.md` files.
