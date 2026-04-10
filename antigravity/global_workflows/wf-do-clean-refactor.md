---
description: Step-by-step code cleanup and refactoring workflow. Use to reduce technical debt, remove dead code, and enforce immutability. Ensures zero regressions with 95%+ coverage maintained throughout.
---

// turbo-all

# Clean Refactor Workflow

This workflow guides a safe, incremental refactoring session that reduces complexity and removes dead code without changing behavior.

## When to Use

- After `@code-reviewer` flags dead code or large files
- Before major feature work to reduce diff noise
- Periodically as codebase maintenance
- After removing a feature or dependency

## When NOT to Use

- During active feature development
- Right before production deployment
- Without proper test coverage
- On code you don't understand

## Workflow Steps

### Step 1: Establish Baseline

Run the full test suite and capture current coverage before making any changes:

```bash
# Node.js / TypeScript
npm run test:coverage

# Flutter / Dart
flutter test --coverage

# Python
pytest --cov=. --cov-report=term-missing

# Java
./gradlew jacocoTestReport
```

Record the baseline coverage number. This is your floor — coverage must stay at or above this level throughout.

### Step 2: Identify Code Smells

Scan the codebase for issues, prioritized by impact:

| Priority | Smell | Threshold |
|----------|-------|-----------|
| 1 | Dead imports/variables | Any |
| 2 | Commented-out code | Any (it's in git history) |
| 3 | `console.log` / `print()` | Any debug logging |
| 4 | Magic numbers/strings | Any unexplained constants |
| 5 | Mutation patterns | Any in-place mutation |
| 6 | Functions >50 lines | Split into focused helpers |
| 7 | Files >800 lines | Extract modules by responsibility |
| 8 | Deep nesting >4 levels | Use early returns, extract helpers |
| 9 | Duplicated logic (3+ occurrences) | Extract shared utility |

Work through **one category at a time**. Do not mix categories in a single commit.

### Step 3: Apply Refactoring (One Category)

Pick the highest-priority category from Step 2 and delegate to the `@refactor-cleaner` agent to apply fixes:

**Dead code removal:**
```bash
# Find unused exports (Node.js)
npx ts-prune

# Find unused imports (Dart)
dart fix --dry-run

# Find unused imports (Python)
autoflake --check -r .
```

**After each batch of changes, run tests immediately:**

```bash
# Node.js / TypeScript
npm test

# Flutter / Dart
flutter test

# Python
pytest

# Java
./gradlew test
```

If any test breaks — **revert and investigate**. Do not continue.

**Git checkpoint:**
```bash
git add -A && git commit -m "refactor: remove dead imports across [module]"
```

### Step 4: Repeat for Next Category

Go back to Step 3 and pick the next category. Each category gets its own commit:

```
refactor: remove commented-out code from services/
refactor: extract magic numbers to constants in utils/
refactor: convert mutation patterns to immutable updates
refactor: split processOrders into focused helpers
```

### Step 5: Verify Zero Regressions

After all categories are addressed, run the full test suite and coverage check:

```bash
# Node.js / TypeScript
npm run test:coverage

# Flutter / Dart
flutter test --coverage

# Python
pytest --cov=. --cov-report=term-missing

# Java
./gradlew jacocoTestReport
```

**Required:**
- All tests pass (zero failures).
- Coverage is **at or above the baseline** from Step 1.
- If coverage dropped, write additional tests before finishing.

### Step 6: Report Summary

Produce a summary of what was cleaned:

```
## Refactor Summary

| Category | Count | Impact |
|----------|-------|--------|
| Dead code removed | 15 items | Cleaner imports, smaller bundle |
| Functions split | 3 functions | Below 50-line threshold |
| Mutations fixed | 8 patterns | Immutability enforced |
| Constants extracted | 5 values | No more magic numbers |

Coverage: 96.2% (baseline was 95.1%) — improved.
Regressions: 0
```

## Refactoring Patterns

### Flatten Deep Nesting
```
// BEFORE                          // AFTER
if (items) {                       if (!items) return;
  for (item of items) {            items
    if (item.active) {               .filter(i => i.active && i.valid)
      if (item.valid) {              .forEach(handle);
        handle(item);
      }
    }
  }
}
```

### Enforce Immutability
```
// BEFORE: Mutation               // AFTER: Immutable
user.name = newName;              const updated = { ...user, name: newName };
users.push(newUser);              const updated = [...users, newUser];
```

### Extract Constants
```
// BEFORE: Magic numbers          // AFTER: Named constants
if (retries > 3) { ... }         const MAX_RETRIES = 3;
setTimeout(fn, 5000);            const TIMEOUT_MS = 5000;
```

## Safety Rules

1. **Never change behavior** — Refactoring is purely structural.
2. **Tests must stay green** — If a test breaks, revert and investigate.
3. **One commit per category** — Don't mix dead code removal with logic restructuring.
4. **Preserve all comments** — Keep existing documentation unless it references removed code.
5. **Be conservative** — When in doubt, don't remove.

---

**Remember**: Start small, test often, and commit frequently. A clean codebase is a fast codebase.
