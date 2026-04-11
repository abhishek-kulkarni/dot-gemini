---
description: Analyzes staged/unstaged changes and generates a concise, conventional commit message. Use before committing to get a well-formatted message.
---

---
tools: Use any tools necessary for analyzing git changes
---

# Commit Message Generator

Generate a descriptive yet concise conventional commit message based on the actual code changes.

> [!IMPORTANT]
> **Mandatory Rules:**
> 1. **ALWAYS run the git commands** in Step 1. DO NOT rely on previous conversation history or your memory of what was changed. The local state may have changed (commits, restores, or pushes).
> 2. **DO NOT run commands in the background** (e.g., set `WaitMsBeforeAsync` to a high value). Analysis requires immediate output to ensure accuracy.

## Process

### Step 1: Gather Changes

```bash
# Check staged changes first
git --no-pager diff --staged --stat
git --no-pager diff --staged

# Check unstaged changes
git --no-pager diff --stat
git --no-pager diff

# Check for new untracked files
git status --short
```

### Step 2: Analyze the Diff

For each changed file, identify:
- **What changed**: New functions, modified logic, deleted code, config changes.
- **Why it changed**: Bug fix? New capability? Cleanup? Performance improvement?
- **Scope**: Which feature area or module is affected.

### Step 3: Determine Commit Type

| Type | When to use |
|------|-------------|
| `feat` | New functionality visible to users or consumers of the API |
| `fix` | Bug fix — something was broken and is now correct |
| `refactor` | Code restructuring with no behavior change |
| `docs` | Documentation only (README, comments, JSDoc, DartDoc) |
| `test` | Adding or updating tests with no production code change |
| `chore` | Build config, CI, dependencies, tooling — no production logic |
| `perf` | Performance improvement with no behavior change |
| `ci` | CI/CD pipeline changes only |

### Step 4: Generate the Message

**Format:**
```
<type>: <description>
```

**Rules:**
1. **Lowercase** — No capital letter after the colon.
2. **Imperative mood** — "add", "fix", "refactor" — not "added", "fixes", "refactoring".
3. **No period** at the end.
4. **Under 72 characters** for the subject line.
5. **Describe the intent, not the obvious** — Focus on *why* or *what capability*, not a mechanical list of what files changed.

### What to Avoid

- ❌ `feat: add localization strings` — Too obvious, just restates the diff.
- ❌ `fix: fix bug` — Says nothing useful.
- ❌ `chore: update files` — Which files? Why?
- ❌ `feat: add new function handleSubmit and update UserForm component` — Too detailed for subject line.
- ❌ `refactor: refactor code` — Redundant.

### What to Write

- ✅ `feat: support offline mode for prayer tracking` — Describes the capability.
- ✅ `fix: prevent double-tap crash on submit button` — Describes the bug.
- ✅ `refactor: extract auth logic into dedicated service` — Describes the structural change.
- ✅ `perf: batch database writes for bulk import` — Describes the optimization.
- ✅ `chore: migrate to Dart 3.5 and update dependencies` — Describes the maintenance action.

## Multi-Line Body (Optional)

For complex changes, add a body separated by a blank line:

```
feat: add subscription tier gating for pro features

- Middleware checks subscription status on protected routes
- Free users see upgrade prompt instead of gated content
- Webhook handler syncs tier changes from Stripe
```

**Body rules:**
- Use bullet points for multiple related changes.
- Explain *why* if the change is non-obvious.
- Reference issue numbers if applicable: `Fixes #42`.

## Multiple Unrelated Changes

If the diff contains unrelated changes, suggest **separate commits**:

```
Suggested commits (split recommended):

1. fix: resolve null pointer in user profile loader
2. refactor: extract theme constants to design_tokens.dart
3. test: add missing widget tests for settings screen
```

## Output Format Constraints

You must output **ONLY** the raw formatted text wrapped in a single markdown code block. 
- **DO NOT** output any conversational filler before or after the code block.
- **DO NOT** add inline backticks around the commit messages inside the block.
- **DO NOT** add explanatory bullet points or italicized text under the commit messages in a split recommendation.

Always output exactly one of:

**Single commit:**
```
feat: <description>
```

**Single commit with body:**
```
feat: <description>

- Detail 1
- Detail 2
```

**Split recommendation:**
```
These changes should be split into separate commits:

1. <type>: <description>
2. <type>: <description>
```