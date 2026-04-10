# Testing Standards

These rules govern how tests are written, structured, and maintained across all projects.

## Coverage Requirement: 95%+

All new and modified code must achieve **95%+ coverage** across branches, functions, lines, and statements. Coverage below this threshold is a **blocking** issue.

## Test Types (All Required)

| Type | What to Test | When |
|------|-------------|------|
| **Unit** | Individual functions, utilities, components in isolation | Always |
| **Integration** | API endpoints, database operations, service interactions | Always |
| **E2E** | Critical user flows end-to-end | Critical paths |

## Test-Driven Development

Mandatory workflow for all production code changes:

1. Write test first **(RED)** — test must fail for the right reason.
2. Write minimal implementation **(GREEN)** — only enough code to pass.
3. Refactor **(IMPROVE)** — clean up while keeping tests green.
4. Verify coverage **(95%+)** — write additional tests for missed branches.

Use `@tdd-guide` for detailed guidance.

## Test Structure (AAA Pattern)

All tests must follow Arrange-Act-Assert:

```
// Arrange — set up test data and dependencies
// Act    — execute the code under test
// Assert — verify the expected outcome
```

## Test Naming

Use descriptive names that explain the behavior under test:

```
✅ "returns empty list when no items match query"
✅ "throws error when API key is missing"
✅ "rejects unauthorized users with 403"
✅ "falls back to cache when network is unavailable"

❌ "test1"
❌ "it works"
❌ "handles error"
```

## Edge Cases You MUST Test

1. **Auth/Permissions** — unauthorized and forbidden access
2. **Null/undefined/empty** — null input, empty arrays, blank strings
3. **Boundary values** — min, max, zero, negative
4. **Error paths** — network failures, DB errors, invalid input
5. **Race conditions** — concurrent operations
6. **Large data** — performance with 1k+ items
7. **Special characters** — Unicode, emojis, SQL injection chars

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Bad | Do This Instead |
|-------------|-------------|-----------------|
| Testing implementation details | Breaks on every refactor | Test user-visible behavior |
| Shared state between tests | Tests affect each other | Each test sets up its own data |
| Mocking everything | Tests pass but code fails | Mock only external dependencies |
| No error path tests | Bugs hide in catch blocks | Test every error state explicitly |
| Skipping the RED step | No proof the test catches bugs | Always verify failure first |
| Brittle selectors | Break on styling changes | Use `data-testid` or semantic selectors |
| `expect(true).toBe(true)` | Tests nothing useful | Assert specific values and behavior |

## Troubleshooting Test Failures

1. Use `@tdd-guide` for guidance.
2. Check test isolation — does the test depend on another test's state?
3. Verify mocks are correct — is the mock returning what the real service would?
4. Fix the implementation, not the tests — unless the test expectation is wrong.
5. Check for flakiness — use explicit waits, not timing-based assertions.

## Test Isolation

- Each test must set up its own data and clean up after itself.
- No shared mutable state between test cases.
- External dependencies (APIs, databases, file system) must be mocked or faked.
- Tests must be runnable in any order.

## Test Commands

```bash
# Node.js / TypeScript
npm test
npm run test:coverage

# Flutter / Dart
flutter test
flutter test --coverage

# Python
pytest
pytest --cov=. --cov-report=term-missing

# Java
./gradlew test
./gradlew jacocoTestReport
```
