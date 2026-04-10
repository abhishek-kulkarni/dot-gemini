# Build Resolution Standards

These rules dictate how compilation, type errors, and dependency failures must be handled globally across all projects.

## When to Trigger Build Resolution

**Mandatory build resolution triggers:**
- Before any commit is staged (code must compile and pass static analysis without critical errors).
- When a CI/CD build fails.
- When `flutter analyze`, `tsc`, `mypy`, or `javac` reports errors in locally modified files.

## Resolution Principles

The primary objective during build resolution is **speed to green**.

1. **Minimal Diffs:** Implement the smallest functional change needed to placate the compiler or type checker. Use the `@build-error-resolver` agent for surgical precision.
2. **No Architectural Refactoring:** Unless the architecture itself fundamentally prevents compilation, do not reorganize files, redesign classes, or refactor clean code dynamically during a build fix phase. Keep `@refactor-cleaner` away from this process to prevent sprawling diff scopes.
3. **No Unrelated Code Touch:** Only modify files explicitly mentioned in the compiler error trace.
4. **Prefer Explicit Over Implicit:** If inference fails, add explicit type definitions before casting. If nullable warnings appear, add explicit null checks (`if != null`, `?.`) rather than unsafe forces (`!`, `as any`).

## Common Triage Steps

- **Dependency Mismatch:** Sync lockfiles (`npm install`, `flutter pub get`) and ensure version ranges are properly resolved. Often, clearing the package cache resolves ghost errors.
- **Type Nullability:** Verify if upstream API types changed to nullable. If they did, handle correctly instead of suppressing.
- **Missing Classes/Interfaces:** Provide minimal stubs or standard code generation patches to implement missing abstract overrides.

## Agent & Workflow Routing

| Task | Agent / Workflow | Description |
|------|------------------|-------------|
| Build Compilation Failed | `/wf-resolve-build` | Full end-to-end diagnostic and patch workflow. |
| Surgical Type Errors | `@build-error-resolver` | Direct agent invocation to fix a rigid type issue. |
