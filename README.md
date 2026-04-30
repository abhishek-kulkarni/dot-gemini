# dot-gemini

AI agent configuration, coding rules, and automated workflows for [Gemini](https://gemini.google.com/) and [Antigravity](https://antigravity.dev/).

Inspired by and adapted from [everything-claude-code](https://github.com/affaan-m/everything-claude-code).

## Structure

```
.gemini/
├── GEMINI.md                          # Global rules (auto-loaded)
├── AGENTS.md                          # Agent operating standards
├── agents/                            # Specialized AI agents
│   ├── code-reviewer.md               # General code quality reviewer
│   ├── architect.md                   # System design & scalability
│   ├── flutter-dart-code-reviewer.md   # Deep Flutter/Dart reviewer
│   ├── py-code-reviewer.md             # Deep Python reviewer
│   ├── security-reviewer.md            # Security vulnerability scanner
│   ├── tdd-guide.md                    # Test-driven development guide
│   ├── planner.md                      # Implementation planning
│   ├── refactor-cleaner.md             # Technical debt cleanup
│   ├── doc-updater.md                  # Documentation & codemap generator
│   └── build-error-resolver.md         # Fixes compilation & type errors
├── rules/                             # Always-follow coding rules
│   ├── coding-standards.md             # Immutability, size limits, constants
│   ├── security.md                     # Pre-commit security checklist
│   ├── delivery.md                     # Conventional commits, coverage, shipping
│   ├── code-review.md                  # Review triggers, severity levels, routing
│   ├── testing.md                      # 95%+ coverage, TDD, edge cases
│   ├── build-resolution.md             # Speed to green, minimal diffs
│   ├── dart/                           # Dart & Flutter specific
│   │   ├── coding-standards.md         # Null safety, sealed types, async patterns
│   │   ├── security.md                 # Mobile security, WebView, data protection
│   │   ├── testing.md                  # BLoC, Riverpod, widget & golden tests
│   │   └── build-resolution.md         # Null safety, generic types, build_runner
│   └── python/                         # Python specific
│       ├── coding-standards.md         # PEP 8, type hints, comprehensions
│       ├── security.md                 # SQL injection, dangerous functions, auth
│       ├── testing.md                  # pytest, fixtures, FastAPI & Django
│       └── build-resolution.md         # Mypy inference, missing types, imports
└── antigravity/                       # Antigravity-specific config
    └── global_workflows/               # Slash-command workflows (/wf-*)
        ├── wf-tdd.md                   # Red-Green-Refactor TDD cycle
        ├── wf-plan.md                  # Feature planning workflow
        ├── wf-do-clean-refactor.md     # Code cleanup & refactoring
        ├── wf-get-commit-message.md    # Commit message generator
        ├── wf-review-code.md          # Multi-agent code review
        └── wf-resolve-build.md         # Surgical build fixing
```

## How It Works

### Rules (always active)

`GEMINI.md` is auto-loaded into every AI conversation. It references `rules/` for detailed coding, security, and delivery standards that apply to all projects.

### Agents (invoke with `@agent-name`)

Specialized agents with deep expertise. Invoke by mentioning them in chat:

- `@code-reviewer` — General quality, immutability, patterns across all languages.
- `@flutter-dart-code-review` — Flutter widgets, state management, Dart idioms.
- `@architect` — System design, scalability, patterns, and trade-off analysis.
- `@python-reviewer` — PEP 8, type hints, Django/FastAPI/Flask.
- `@security-reviewer` — OWASP Top 10, secrets detection, dependency audits.
- `@tdd-guide` — Test-first methodology with 95%+ coverage.
- `@planner` — Structured implementation plans for complex features.
- `@refactor-cleaner` — Dead code removal, complexity reduction.
- `@doc-updater` — Documentation and codemap generation.
- `@build-error-resolver` — Multi-language compiler/build error fixer.

### Workflows (Antigravity only, invoke with `/wf-*`)

Step-by-step automated workflows:

- `/wf-tdd` — Full Red-Green-Refactor cycle with git checkpoints.
- `/wf-plan` — Research → design → write plan → get approval.
- `/wf-do-clean-refactor` — Incremental cleanup with zero regressions.
- `/wf-get-commit-message` — Analyze diff → generate conventional commit.
- `/wf-review-code` — Orchestrate reviewers based on detected languages.
- `/wf-resolve-build` — Diagnose → apply minimal fix → clear cache → verify.

## Per-Project Rules

For language or project-specific rules, copy the appropriate `GEMINI.md` template to your project root. It will be auto-loaded alongside the global rules.

### Setup for Flutter / Dart Projects

```bash
# Copy the Dart template to your project root
cp ~/.gemini/rules/dart/GEMINI.md ~/path/to/my-flutter-app/GEMINI.md
```

Then customize the template for your project:
- Set your state management library (Riverpod, BLoC, Provider, etc.)
- Set your routing library (go_router, auto_route, etc.)
- Add project-specific conventions and decisions

### Setup for Python Projects

```bash
# Copy the Python template to your project root
cp ~/.gemini/rules/python/GEMINI.md ~/path/to/my-python-api/GEMINI.md
```

Then customize the template for your project:
- Set your framework (FastAPI, Django, Flask, etc.)
- Set your ORM and database
- Set your package manager (poetry, pip, uv, etc.)
- Add project-specific conventions and decisions

### How It Loads

```
~/.gemini/GEMINI.md          ← Global rules (always loaded)
    ↓ references
~/.gemini/rules/*.md         ← Common rules (coding, security, delivery, testing)
    ↓ language rules referenced from project GEMINI.md
~/.gemini/rules/dart/*.md    ← Dart-specific rules
~/.gemini/rules/python/*.md  ← Python-specific rules

my-project/GEMINI.md         ← Project-specific rules (auto-loaded when in this project)
```

Both the global `~/.gemini/GEMINI.md` and project-level `GEMINI.md` are loaded automatically. The language-specific rule files are read when referenced.

## Key Standards

| Standard | Target |
|----------|--------|
| Test coverage | 95%+ on new/modified code |
| Function size | < 50 lines |
| File size | < 800 lines |
| Nesting depth | < 4 levels |
| Commit format | `<type>: <description>` |
| Security | Mandatory pre-commit checklist |
| Immutability | No in-place mutation |

## Languages Supported

All agents and workflows include patterns for:

- **Node.js / TypeScript** — React, Next.js, Express
- **Flutter / Dart** — BLoC, Riverpod, Provider, GetX, MobX
- **Python** — Django, FastAPI, Flask
- **Java** — Spring Boot, Gradle, Maven

## Installation

```bash
# Clone to your home directory
git clone https://github.com/abhishek-kulkarni/dot-gemini.git ~/.gemini
```

### Quick Start

1. Clone the repo (above).
2. Copy a language template to your project:
   ```bash
   # For Flutter/Dart
   cp ~/.gemini/rules/dart/GEMINI.md ~/my-flutter-app/GEMINI.md

   # For Python
   cp ~/.gemini/rules/python/GEMINI.md ~/my-python-api/GEMINI.md
   ```
3. Customize the project `GEMINI.md` with your framework, architecture, and conventions.
4. Start coding — the rules and agents are active automatically.

## License

MIT
