---
description: A surgical workflow to resolve build failures, compilation errors, and strict type checking errors across Dart, Python, TS/JS, and Java with minimal diffs and zero architectural changes.
---

# Build Resolution Workflow

A surgical workflow to resolve build failures, compilation errors, and strict type checking errors across Dart, Python, TS/JS, and Java with minimal diffs and zero architectural changes.

## Process

### Step 1: Discover Errors
1. Analyze the project environment to determine the primary language and relevant build system (e.g. Flutter, TypeScript, Python).
2. Execute the language-specific diagnostic command to collect all compilation/type errors.
   - **Flutter/Dart:** `flutter analyze` or `flutter build`
   - **Node/TypeScript:** `npx tsc --noEmit --pretty`
   - **Python:** `mypy src/` or `ruff check .`
3. Identify the specific errors directly blocking the build.

### Step 2: Formulate Minimal Fixes (via Agent)
Delegate to the `@build-error-resolver` agent to devise the smallest possible change to resolve each error:
- **Type Inference:** Add missing explicit type annotations.
- **Null Safety:** Utilize `?.`, `??`, or null-checks (`if (x != null)`) to safely unpack parameters.
- **Missing Imports:** Insert the correct absolute or package import statements.
- **Missing Interfaces:** Provide missing overrides or default values for inherited classes.
- **Dependencies:** Add missing packages via standard CLI addition tools (`flutter pub add`, `npm install`).

### Step 3: Apply Fast Patches
1. Update code files surgically. 
2. **DO NOT** refactor the surrounding logic.
3. **DO NOT** shift the architecture or redesign classes to be "better" or cleaner. The only goal is getting the compiler to succeed.

### Step 4: Verify and Iterate
1. Re-run the diagnostic command.
2. If new errors cropped up (due to ripple effects of type changes), iterate on Step 2 and 3 until the diagnostic command runs entirely clean.

### Step 5: Clean the Cache (If Stuck)
If the build system begins throwing incomprehensible or caching-related errors, use nuclear cache clears:
- **Dart:** `flutter clean && flutter pub get` (and `build_runner` clean if code generation fails)
- **TS/JS:** `rm -rf node_modules .next .cache && npm install`
- **Python:** Remove `__pycache__`, `.mypy_cache`, and recreate `.venv`

### Step 6: Review & Finalize
Ensure that the minimal fixes haven't drastically broken logic tests, and then mark the task as complete once the build returns to a passing green state.