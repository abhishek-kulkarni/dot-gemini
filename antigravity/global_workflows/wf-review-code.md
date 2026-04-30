---
description: Comprehensive code review workflow that orchestrates the appropriate review agents based on detected languages and change scope. Use this to run a full review of staged or recent changes.
---

---
tools: Use any tools necessary for code review
---

# Review Code Workflow

This workflow orchestrates the appropriate review agents based on the languages and patterns detected in the current changes.

## Trigger

Run this workflow when:
- Code changes are ready for review (staged or recently committed).
- Before merging a PR or pushing to a shared branch.
- After completing a feature or bug fix.

## Workflow Steps

### Step 1: Detect Scope & Languages

Analyze the diff to determine which languages and frameworks are involved:

```bash
# Get list of changed files
git diff --staged --name-only
git diff --name-only

# If no staged/unstaged changes, check recent commits
git log --oneline -3 --name-only
```

Classify files by language:
- `.ts`, `.tsx`, `.js`, `.jsx` → **Node.js / TypeScript**
- `.dart` → **Flutter / Dart**
- `.py` → **Python**
- `.java`, `.kt` → **Java / Kotlin**
- `.css`, `.scss`, `.html` → **Web / UI**

### Step 2: Run Language-Specific Reviewer

Based on detected languages, invoke the appropriate reviewer:

| Detected Language | Agent to Invoke | Priority |
|-------------------|-----------------|----------|
| Python (`.py`) | `@py-code-reviewer` | First (specialized) |
| Flutter/Dart (`.dart`) | `@flutter-dart-code-reviewer` | First (specialized) |
| Any language | `@code-reviewer` | Always (general quality) |

> **Note**: For Python projects, run `@py-code-reviewer` first. For Flutter/Dart projects, run `@flutter-dart-code-reviewer` first. Then run `@code-reviewer` for general quality checks (immutability, coverage, git workflow).

### Step 3: Run Security Review

**Always** run the security reviewer when changes touch:
- Authentication or authorization logic
- API endpoints or route handlers
- Database queries or ORM models
- User input handling or form processing
- Payment or sensitive data flows
- External API integrations
- Dependency updates (`package.json`, `pubspec.yaml`, `requirements.txt`, `pom.xml`)

Invoke: `@security-reviewer`

### Step 4: Verify Test Coverage

If the reviewers flag coverage gaps (< 95%), invoke:

Invoke: `@tdd-guide` — to help write missing tests using the Red-Green-Refactor cycle.

### Step 5: Consolidate & Report

Combine all findings into a single unified report:

```
## Unified Review Report

### Code Review (code-reviewer)
- **CRITICAL**: [List issues or "None"]
- **HIGH**: [List issues or "None"]
- **MEDIUM**: [List issues or "None"]
- **LOW**: [List issues or "None"]

### Security Review (security-reviewer)
- **CRITICAL**: [List issues or "None"]
- **HIGH**: [List issues or "None"]
- **MEDIUM**: [List issues or "None"]
- **LOW**: [List issues or "None"]

### Coverage Status
[Pass/Fail from Step 4]

---

## Combined Summary

| Agent | CRITICAL | HIGH | MEDIUM | LOW | Verdict |
|-------|----------|------|--------|-----|---------|
| `@code-reviewer` | 0 | 1 | 2 | 0 | BLOCK |
| `@security-reviewer` | 0 | 0 | 1 | 0 | APPROVE |
| `@py-code-reviewer` | 0 | 0 | 0 | 3 | APPROVE |
| `@flutter-dart-code-reviewer` | 0 | 0 | 1 | 0 | APPROVE |

**Overall Verdict**: BLOCK — Resolve 1 HIGH issue before merge.

## Suggested Next Steps
- Call `@tdd-guide` to write missing tests.
- Call `@refactor-cleaner` to address dead code findings.
```

## Decision Matrix

| Condition | Action |
|-----------|--------|
| Any CRITICAL finding | **BLOCK** — Must fix immediately |
| Any HIGH finding | **BLOCK** — Must fix before merge |
| Only MEDIUM findings | **WARNING** — Can merge with caution |
| Only LOW findings | **APPROVE** — Merge freely |
| Coverage < 95% | **BLOCK** — Write tests first |

## Quick Reference: Agent Capabilities

| Agent | Specialization |
|-------|---------------|
| [code-reviewer](file:///Users/abhishekkulkarni/.gemini/agents/code-reviewer.md) | General quality, immutability, React/Node/Flutter/Python/Java patterns |
| [flutter-dart-code-review](file:///Users/abhishekkulkarni/.gemini/agents/flutter-dart-code-reviewer.md) | Deep Flutter/Dart: widgets, state management, Dart idioms, accessibility, l10n |
| [python-reviewer](file:///Users/abhishekkulkarni/.gemini/agents/py-code-reviewer.md) | Deep Python: PEP 8, type hints, Pythonic idioms, Django/FastAPI/Flask |
| [security-reviewer](file:///Users/abhishekkulkarni/.gemini/agents/security-reviewer.md) | OWASP Top 10, secrets detection, dependency audits, auto-remediation |
| [tdd-guide](file:///Users/abhishekkulkarni/.gemini/agents/tdd-guide.md) | Test-first methodology, 95%+ coverage, framework-specific testing |
| [refactor-cleaner](file:///Users/abhishekkulkarni/.gemini/agents/refactor-cleaner.md) | Dead code removal, complexity reduction, immutability enforcement |