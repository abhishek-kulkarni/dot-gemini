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

## Research & Reuse

Before writing new code:

1. **Search existing codebase first** — look for patterns, utilities, or modules that already solve the problem.
2. **Check package registries** — search npm, pub.dev, PyPI, Maven Central before writing utility code.
3. **Check library docs** — confirm API behavior and version-specific details before implementing.
4. **Prefer proven approaches** — adopt or port a battle-tested implementation over writing net-new code when it meets the requirement.

## Change Quality

- Keep changes self-contained, readable, and easy to revert.
- Plan before editing large features — use the planning workflow.
- Review for security before shipping — use the security checklist.

## Feature Implementation Workflow

For non-trivial changes, follow this sequence:

1. **Research** — Search codebase, docs, and registries for existing solutions.
2. **Plan** — Use `@planner` for multi-file or architectural changes.
3. **TDD** — Write tests first → implement → refactor → verify 95%+ coverage.
4. **Review** — Use `@code-reviewer` (and language-specific reviewers) immediately after writing code.
5. **Ship** — Conventional commits, pre-ship checklist, push.

## Pre-Ship Checklist

Before pushing to a shared branch:

- [ ] All automated checks (CI/CD) are passing.
- [ ] Merge conflicts are resolved.
- [ ] Branch is up to date with target branch.
- [ ] No skipped or disabled tests.
- [ ] Commit messages follow conventional format.
