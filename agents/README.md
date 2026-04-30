# Agents

Specialized AI agents with deep expertise in specific domains. Each agent is a markdown file with frontmatter that makes it discoverable via `@agent-name` in chat.

## Agent Catalog

### Code Reviewers

| Agent | File | Specialization |
|-------|------|---------------|
| `@code-reviewer` | [code-reviewer.md](code-reviewer.md) | General quality across all languages — immutability, coverage, React/Node/Flutter/Python/Java patterns |
| `@flutter-dart-code-review` | [flutter-dart-code-reviewer.md](flutter-dart-code-reviewer.md) | Deep Flutter/Dart — widgets, state management (BLoC, Riverpod, Provider, GetX, MobX, Signals), Dart 3 idioms, accessibility, l10n |
| `@python-reviewer` | [py-code-reviewer.md](py-code-reviewer.md) | Deep Python — PEP 8, type hints, Pythonic idioms, Django/FastAPI/Flask patterns |
| `@security-reviewer` | [security-reviewer.md](security-reviewer.md) | OWASP Top 10, secrets detection, dependency audits, auto-remediation across all languages |

### Development Guides

| Agent | File | Specialization |
|-------|------|---------------|
| `@tdd-guide` | [tdd-guide.md](tdd-guide.md) | Test-driven development with 95%+ coverage, security-first testing, multi-language |
| `@planner` | [planner.md](planner.md) | Implementation planning for complex features — phasing, TDD cycles, security audit |
| `@architect` | [architect.md](architect.md) | System design, scalability, technical trade-offs, patterns, and ADR documentation |
| `@refactor-cleaner` | [refactor-cleaner.md](refactor-cleaner.md) | Technical debt cleanup — dead code, complexity reduction, immutability enforcement |
| `@doc-updater` | [doc-updater.md](doc-updater.md) | Documentation & codemap generation for Node.js, Flutter, Python, and Java projects |
| `@build-error-resolver` | [build-error-resolver.md](build-error-resolver.md) | Resolves build/type errors rapidly with minimal diffs and no refactoring |

## Review Flow

For a full code review, the recommended flow is:

1. **Language-specific reviewer** — `@flutter-dart-code-review` or `@python-reviewer` (deep analysis).
2. **General reviewer** — `@code-reviewer` (cross-cutting quality).
3. **Security** — `@security-reviewer` (vulnerability scan).
4. **Coverage gaps** — `@tdd-guide` (write missing tests).

This flow is automated by the `/wf-review-code` workflow.

## Frontmatter Format

Each agent file uses this frontmatter to be discoverable:

```yaml
---
name: agent-name
description: What the agent does and when to use it.
tools: Use any tools necessary for [task]
---
```

## Shared Standards

All agents enforce these project-wide standards (defined in `AGENTS.md`):

- **95%+ test coverage** on new/modified code
- **Security-first** — OWASP checks, secrets detection, auth verification
- **Immutability** — No in-place mutation
- **Conventional commits** — `feat:`, `fix:`, `refactor:`, `test:`, `docs:`
- **Confidence-based filtering** — Report only >80% confidence issues
