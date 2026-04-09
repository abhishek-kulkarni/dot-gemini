# dot-gemini

AI agent configuration, coding rules, and automated workflows for [Gemini](https://gemini.google.com/) and [Antigravity](https://antigravity.dev/).

## Structure

```
.gemini/
├── GEMINI.md                          # Global rules (auto-loaded)
├── AGENTS.md                          # Agent operating standards
├── agents/                            # Specialized AI agents
│   ├── code-reviewer.md               # General code quality reviewer
│   ├── flutter-dart-code-reviewer.md   # Deep Flutter/Dart reviewer
│   ├── py-code-reviewer.md             # Deep Python reviewer
│   ├── security-reviewer.md            # Security vulnerability scanner
│   ├── tdd-guide.md                    # Test-driven development guide
│   ├── planner.md                      # Implementation planning
│   ├── refactor-cleaner.md             # Technical debt cleanup
│   └── doc-updater.md                  # Documentation & codemap generator
├── rules/                             # Always-follow coding rules
│   ├── coding-standards.md             # Immutability, size limits, constants
│   ├── security.md                     # Pre-commit security checklist
│   └── delivery.md                     # Conventional commits, coverage, shipping
└── antigravity/                       # Antigravity-specific config
    └── global_workflows/               # Slash-command workflows (/wf-*)
        ├── wf-tdd.md                   # Red-Green-Refactor TDD cycle
        ├── wf-plan.md                  # Feature planning workflow
        ├── wf-do-clean-refactor.md     # Code cleanup & refactoring
        ├── wf-get-commit-message.md    # Commit message generator
        └── wf-review-code.md          # Multi-agent code review
```

## How It Works

### Rules (always active)

`GEMINI.md` is auto-loaded into every AI conversation. It references `rules/` for detailed coding, security, and delivery standards that apply to all projects.

### Agents (invoke with `@agent-name`)

Specialized agents with deep expertise. Invoke by mentioning them in chat:

- `@code-reviewer` — General quality, immutability, patterns across all languages.
- `@flutter-dart-code-review` — Flutter widgets, state management, Dart idioms.
- `@python-reviewer` — PEP 8, type hints, Django/FastAPI/Flask.
- `@security-reviewer` — OWASP Top 10, secrets detection, dependency audits.
- `@tdd-guide` — Test-first methodology with 95%+ coverage.
- `@planner` — Structured implementation plans for complex features.
- `@refactor-cleaner` — Dead code removal, complexity reduction.
- `@doc-updater` — Documentation and codemap generation.

### Workflows (Antigravity only, invoke with `/wf-*`)

Step-by-step automated workflows:

- `/wf-tdd` — Full Red-Green-Refactor cycle with git checkpoints.
- `/wf-plan` — Research → design → write plan → get approval.
- `/wf-do-clean-refactor` — Incremental cleanup with zero regressions.
- `/wf-get-commit-message` — Analyze diff → generate conventional commit.
- `/wf-review-code` — Orchestrate reviewers based on detected languages.

## Per-Project Rules

For language or project-specific rules, add a `GEMINI.md` at the project root:

```
my-flutter-app/
├── GEMINI.md              # Flutter-specific rules (auto-loaded)
└── ...
```

Both the global `~/.gemini/GEMINI.md` and project-level `GEMINI.md` are loaded automatically.

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

## License

MIT
