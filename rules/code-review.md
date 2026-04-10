# Code Review Standards

These rules define when and how to conduct code reviews. For the detailed review process, use the `@code-reviewer`, `@flutter-dart-code-review`, or `@python-reviewer` agents.

## When to Review

**Mandatory review triggers:**

- After writing or modifying production code
- Before any commit to shared branches
- When security-sensitive code is changed (auth, payments, user data)
- When architectural changes are made
- Before merging pull requests

## Pre-Review Requirements

Before requesting review, ensure:

- [ ] All automated checks (CI/CD) are passing
- [ ] Merge conflicts are resolved
- [ ] Branch is up to date with target branch
- [ ] No `console.log` / `print()` / debug statements in committed code

## Security Review Triggers

**STOP and use `@security-reviewer` when changes touch:**

- Authentication or authorization code
- User input handling or form processing
- Database queries or ORM models
- File system operations
- External API calls or webhooks
- Cryptographic operations
- Payment or financial code
- Dependency updates

## Review Severity Levels

| Level | Meaning | Action |
|-------|---------|--------|
| CRITICAL | Security vulnerability or data loss risk | **BLOCK** — Must fix before merge |
| HIGH | Bug, missing tests (<95%), or significant quality issue | **BLOCK** — Must fix before merge |
| MEDIUM | Maintainability concern | **WARNING** — Consider fixing |
| LOW | Style or minor suggestion | **NOTE** — Optional |

## Common Issues to Catch

### Security
- Hardcoded credentials (API keys, passwords, tokens)
- SQL injection (string concatenation in queries)
- XSS vulnerabilities (unescaped user input)
- Path traversal (unsanitized file paths)
- CSRF protection missing
- Authentication bypasses
- Internal error details leaked in responses

### Code Quality
- Functions >50 lines — split into focused helpers
- Files >800 lines — extract modules by responsibility
- Deep nesting >4 levels — use early returns
- Missing error handling — handle explicitly
- Mutation patterns — prefer immutable operations
- Missing tests — coverage must be 95%+

### Performance
- N+1 queries — use JOINs or batching
- Missing pagination — add LIMIT to queries
- Unbounded queries — add constraints
- Missing caching — cache expensive operations

## Approval Criteria

- **Approve**: No CRITICAL or HIGH issues.
- **Warning**: Only MEDIUM issues (merge with caution).
- **Block**: Any CRITICAL or HIGH issues found.
- **Block**: Test coverage below 95%.

## Agent Routing

| Language | Agent |
|----------|-------|
| Any / General | `@code-reviewer` |
| Flutter / Dart | `@flutter-dart-code-review` |
| Python | `@python-reviewer` |
| Security-sensitive | `@security-reviewer` |
| Coverage gaps | `@tdd-guide` |
