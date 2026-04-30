---
description: Step-by-step planning workflow for complex features and architectural changes. Use when the task requires significant design, phasing, or multi-file changes. Produces a structured implementation plan with TDD cycles, security audit, and phased delivery.
---

# Planning Workflow

This workflow produces a detailed, actionable implementation plan for complex features, refactors, or architectural changes.

## When to Use

- New features spanning multiple files or modules
- Architectural changes or migrations
- Complex refactoring efforts
- Any task where you're unsure of the approach

## When NOT to Use

- Trivial bug fixes or one-liner changes
- Simple config updates
- Tasks with an obvious single-step solution

## Workflow Steps

### Step 1: Understand the Request

Before planning, gather full context:

1. **Read the request carefully** — identify the core goal and any constraints.
2. **Ask clarifying questions** if the request is ambiguous or underspecified. Do not guess.
3. **List assumptions** — anything you're assuming that wasn't explicitly stated.
4. **Identify success criteria** — what does "done" look like?

### Step 2: Research the Codebase

Explore the affected areas before proposing changes:

```bash
# Understand the project structure
find . -type f -name "*.ts" -o -name "*.dart" -o -name "*.py" -o -name "*.java" | head -50

# Check existing patterns in affected modules
grep -r "pattern_to_find" src/ --include="*.ts" -l

# Review recent changes for context
git log --oneline -10 --name-only
```

Identify:
- **Existing patterns** — How does the codebase already solve similar problems?
- **Reusable code** — What can be extended vs. built from scratch?
- **Dependencies** — What modules/packages are already in use?
- **Test patterns** — How are existing tests structured?

### Step 3: Security Audit

Before designing the solution, invoke the `@security-reviewer` agent (or mentally check) against the security checklist:

- [ ] No hardcoded API keys, passwords, or tokens
- [ ] All external input validated at boundaries
- [ ] Parameterized queries for database operations
- [ ] Sanitized HTML output where applicable
- [ ] Authz/Authn checked for all sensitive paths
- [ ] Error messages scrubbed of internal details

Document how each point will be addressed in the plan.

### Step 4: Design the Solution

Delegate to the `@architect` agent to design the system structure and the `@planner` agent to format the phased implementation plan:

**Phase sizing:**
- **Phase 1**: Minimum viable — smallest slice that provides value
- **Phase 2**: Core experience — complete happy path
- **Phase 3**: Edge cases — error handling, polish
- **Phase 4**: Optimization — performance, monitoring

Each phase must be **independently mergeable**. Avoid plans that require all phases to complete before anything works.

### Step 5: Write the Implementation Plan

Use this format:

```markdown
# Implementation Plan: [Feature Name]

## Overview
[2-3 sentence summary — what it does, why it matters, approach]

## User Review Required
> [!IMPORTANT]
> [Breaking changes, critical design decisions, security implications]

## Architecture & Security
- **Architecture**: [Pattern/Structure vetted by `@architect`]
- **Security Checklist**: [How each point is addressed via `@security-reviewer`]
- **Storage/State**: [Immutable state management pattern]
- **Design/UX**: [Aesthetic considerations if UI is involved]

## Implementation Steps

### Phase 1: [Phase Name]
1. **[TDD] Step Name** (File: path/to/file)
   - Action: Write tests for X, then implement minimal Y
   - Commit: `test: ...` → `feat: ...`
   - Why: [Reasoning]
   - Risk: Low/Medium/High

### Phase 2: [Phase Name]
...

## Testing Strategy
- **Unit Tests**: [What to test, 95%+ target]
- **Integration Tests**: [API/DB flows]
- **E2E Tests**: [Critical user journeys]

## Risks & Mitigations
- **Risk**: [Description] → **Mitigation**: [Action]

## Success Criteria
- [ ] 95%+ test coverage across affected files
- [ ] No security vulnerabilities
- [ ] [Feature-specific criteria]
```

### Step 6: Red Flag Check

Before presenting the plan, verify it doesn't have these problems:

| Red Flag | Resolution |
|----------|-----------|
| Steps without file paths | Add exact file locations |
| No testing strategy | Add TDD steps to every phase |
| Phases that can't ship independently | Re-scope phase boundaries |
| Functions that will exceed 50 lines | Plan to split into helpers |
| Files that will exceed 800 lines | Plan to extract modules |
| Hardcoded values in the design | Plan for constants and env vars |
| Missing error handling | Add error path steps |
| No security considerations | Run Step 3 again |
| Architectural Red Flags | Consult `@architect` Red Flags list |

### Step 7: Present for Review

Present the plan and **wait for user approval** before implementing. Highlight:

- Any decisions that need user input
- Trade-offs you considered
- Open questions

Do NOT start implementation until the user approves.

## After Approval: Execute

Once approved, follow this execution pattern for each step:

1. **Write tests first** — Follow `/wf-tdd` (RED → GREEN → REFACTOR).
2. **Commit after each step** — Use conventional commits.
3. **Verify coverage after each phase** — Must stay at 95%+.
4. **Update the plan** if you discover issues — get re-approval for significant changes.

## Best Practices

1. **Be specific** — Use exact file paths, function names, variable names.
2. **Prioritize immutability** — Plan for new objects, not in-place mutation.
3. **Think small** — Functions <50 lines, files <800 lines.
4. **Explain why** — Document reasoning, not just actions.
5. **Consider edge cases** — Null, empty, invalid, unauthorized.
6. **Fail loudly** — Plan for clear error messages, not silent failures.

---

**Remember**: A great plan is specific, actionable, and considers both the happy path and edge cases. The best plans enable confident, incremental implementation.
