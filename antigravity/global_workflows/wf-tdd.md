---
description: Step-by-step TDD workflow enforcing write-tests-first methodology. Use when writing new features, fixing bugs, or refactoring code. Ensures 95%+ test coverage with git checkpoint commits at each stage.
---

// turbo-all

# TDD Workflow

This workflow enforces strict Test-Driven Development with git checkpoint commits, multi-language support, and 95%+ coverage.

## When to Use

- Writing new features or functionality
- Fixing bugs or issues
- Refactoring existing code
- Adding API endpoints or screens
- Any change to production code

## Workflow Steps

### Step 1: Write User Story

Define what you're building before writing any code:

```
As a [role], I want to [action], so that [benefit]
```

### Step 2: [RED] Write Failing Tests

Delegate to the `@tdd-guide` agent to write comprehensive tests BEFORE any production code. Include:

1. **Happy path** — The core expected behavior.
2. **Security boundary** — Verify unauthorized users are rejected.
3. **Edge cases** — Null, empty, boundary values, special characters.
4. **Error paths** — Network failures, invalid input, DB errors.

```bash
# Run tests — they MUST fail (RED state)

# Node.js / TypeScript
npm test

# Flutter / Dart
flutter test

# Python
pytest

# Java
./gradlew test
```

**Verify RED state:**
- The test target compiles successfully.
- The new test is actually executed.
- The result is RED (failing).
- The failure is caused by missing implementation, NOT by syntax errors or broken setup.

A test that was only written but never compiled and executed does not count as RED.

**Git checkpoint:**
```bash
git add -A && git commit -m "test: add failing tests for [feature]"
```

### Step 3: [GREEN] Minimal Implementation

Delegate to the `@tdd-guide` agent to write the **minimum** code to make all tests pass. No more.

- Keep functions under 50 lines.
- Use immutable patterns (spread, copyWith, frozen dataclasses).
- Do NOT add code that isn't required by a test.

```bash
# Run tests — they MUST pass (GREEN state)

# Node.js / TypeScript
npm test

# Flutter / Dart
flutter test

# Python
pytest

# Java
./gradlew test
```

**Verify GREEN state:**
- All previously failing tests now pass.
- All previously passing tests still pass.
- No skipped or disabled tests.

**Git checkpoint:**
```bash
git add -A && git commit -m "feat: implement [feature] to pass tests"
```

### Step 4: [REFACTOR] Improve Code

Refactor for clarity while keeping all tests green:

- Remove duplication.
- Improve naming.
- Extract helpers for complex logic.
- Optimize performance.
- Ensure files stay under 800 lines.

```bash
# Run tests — they MUST still pass

# Node.js / TypeScript
npm test

# Flutter / Dart
flutter test

# Python
pytest

# Java
./gradlew test
```

**Git checkpoint:**
```bash
git add -A && git commit -m "refactor: clean up [feature] implementation"
```

### Step 5: [COVERAGE] Verify 95%+

Check coverage and write additional tests for any missed branches:

```bash
# Node.js / TypeScript
npm run test:coverage

# Flutter / Dart
flutter test --coverage
genhtml coverage/lcov.info -o coverage/html

# Python
pytest --cov=. --cov-report=term-missing

# Java
./gradlew jacocoTestReport
```

**Required: 95%+ across branches, functions, lines, and statements.**

If coverage is below 95%, go back to Step 2 and write tests for the uncovered branches before proceeding.

## Framework-Specific Patterns

### Flutter / Dart

**Widget test:**
```dart
testWidgets('displays user name after login', (tester) async {
  await tester.pumpWidget(
    const MaterialApp(home: ProfileScreen()),
  );

  // Verify loading state
  expect(find.byType(CircularProgressIndicator), findsOneWidget);

  // Wait for async completion
  await tester.pumpAndSettle();

  // Verify loaded state
  expect(find.text('John Doe'), findsOneWidget);
});
```

**Mocking:**
```dart
// Use MockClient from package:http
final mockClient = MockClient((request) async {
  return http.Response('{"name": "John"}', 200);
});
```

**State management (BLoC):**
```dart
blocTest<UserBloc, UserState>(
  'emits [loading, loaded] when fetch succeeds',
  build: () => UserBloc(repository: mockRepo),
  act: (bloc) => bloc.add(FetchUser()),
  expect: () => [UserLoading(), UserLoaded(testUser)],
);
```

### Python (Django / FastAPI)

**API test:**
```python
def test_create_user_returns_201(client):
    response = client.post("/users/", json={"name": "Jane"})
    assert response.status_code == 201
    assert response.json()["name"] == "Jane"

def test_create_user_rejects_empty_name(client):
    response = client.post("/users/", json={"name": ""})
    assert response.status_code == 422
```

**Mocking:**
```python
@patch("app.services.email.send_email")
def test_signup_sends_welcome_email(mock_send, client):
    client.post("/signup/", json={"email": "a@b.com"})
    mock_send.assert_called_once_with("a@b.com", subject="Welcome")
```

### Java (Spring Boot)

**Controller test:**
```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    @Autowired MockMvc mockMvc;
    @MockBean UserService userService;

    @Test
    void getUser_returnsUser() throws Exception {
        when(userService.findById(1L)).thenReturn(new User("Jane"));

        mockMvc.perform(get("/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("Jane"));
    }
}
```

### Node.js / TypeScript (Jest/Vitest)

**Unit test:**
```typescript
describe('calculateTotal', () => {
  it('sums line items correctly', () => {
    const items = [{ price: 10, qty: 2 }, { price: 5, qty: 1 }];
    expect(calculateTotal(items)).toBe(25);
  });

  it('returns 0 for empty array', () => {
    expect(calculateTotal([])).toBe(0);
  });

  it('throws on negative quantities', () => {
    expect(() => calculateTotal([{ price: 10, qty: -1 }]))
      .toThrow('Quantity must be positive');
  });
});
```

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Bad | Do This Instead |
|-------------|-------------|-----------------|
| Testing implementation details | Breaks on refactor | Test user-visible behavior |
| Shared state between tests | Tests affect each other | Each test sets up its own data |
| Brittle CSS selectors | Breaks on styling changes | Use `data-testid` or semantic selectors |
| No error path tests | Bugs in error handling | Test every `catch` / error state |
| Skipping the RED step | No proof the test catches bugs | Always verify failure first |
| Mocking everything | Tests pass but code fails | Mock only external dependencies |

## Git Checkpoint Summary

The preferred compact workflow produces 3 commits per TDD cycle:

| Checkpoint | Commit Message | Validates |
|-----------|---------------|-----------|
| RED | `test: add failing tests for [feature]` | Tests fail for the right reason |
| GREEN | `feat: implement [feature] to pass tests` | All tests pass |
| REFACTOR | `refactor: clean up [feature] implementation` | Tests still pass, code improved |

## Success Metrics

- 95%+ code coverage achieved
- All tests passing (green)
- No skipped or disabled tests
- Fast test execution (<30s for unit tests)
- E2E tests cover critical user flows
- Tests catch bugs before production

---

**Remember**: Tests are not optional. They are the safety net that enables confident refactoring, rapid development, and production reliability.
