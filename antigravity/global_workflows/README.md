# Global Workflows

Step-by-step automated workflows for Antigravity. Invoke with `/wf-*` slash commands in chat.

## Workflow Catalog

| Workflow | Command | Description |
|----------|---------|-------------|
| [TDD](wf-tdd.md) | `/wf-tdd` | Full Red-Green-Refactor cycle with git checkpoints and 95%+ coverage |
| [Plan](wf-plan.md) | `/wf-plan` | Research → design → write implementation plan → get approval |
| [Clean Refactor](wf-do-clean-refactor.md) | `/wf-do-clean-refactor` | Incremental code cleanup with zero regressions |
| [Commit Message](wf-get-commit-message.md) | `/wf-get-commit-message` | Analyze diff and generate conventional commit message |
| [Review Code](wf-review-code.md) | `/wf-review-code` | Orchestrate reviewers based on detected languages |
| [Resolve Build](wf-resolve-build.md) | `/wf-resolve-build` | Surgically fix compiler errors with minimal patches |

## When to Use Each

| Situation | Workflow |
|-----------|----------|
| Writing a new feature | `/wf-plan` → `/wf-tdd` |
| Fixing a bug | `/wf-tdd` |
| Cleaning up technical debt | `/wf-do-clean-refactor` |
| Ready to commit | `/wf-get-commit-message` |
| Code ready for review | `/wf-review-code` |
| Complex multi-file change | `/wf-plan` first |
| Compiler/type check fails | `/wf-resolve-build` |

## Annotations

- `// turbo-all` — All `run_command` steps in the workflow auto-run without user approval.
- `// turbo` — Only the immediately following step auto-runs.

## Languages Supported

All workflows include commands for:

- **Node.js / TypeScript** — `npm test`, `npm run test:coverage`
- **Flutter / Dart** — `flutter test`, `flutter test --coverage`
- **Python** — `pytest`, `pytest --cov`
- **Java** — `./gradlew test`, `./gradlew jacocoTestReport`
