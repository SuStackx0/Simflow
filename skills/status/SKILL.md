---
name: status
description: Use when the user wants to know where they are in a project. Reads the spec, git log, and test results to produce a clear snapshot of project state — what's done, what's in progress, what's outstanding.
---

# Status

Gives you a fast, accurate snapshot of where the project stands right now. Useful at the start of any session, after time away, or whenever you need to reorient.

## Flow

```
Read spec file(s) → read git log → check test results → check open questions
        ↓
Print project state snapshot
        ↓
Suggest the most logical next skill to invoke
```

---

## What to Read

**Spec:** check `docs/simflow/specs/` for all spec files. Note each one's date, topic, and any open questions listed inside.

**Git log:** run `git log --oneline -20` to see recent commits. Identify:
- Which tasks have been committed (look for `feat:`, `fix:`, `test:` prefixes)
- When the last commit happened
- Whether there's uncommitted work in the working tree (`git status`)

**Tests:** check if a test runner exists and run it if possible. If tests were run recently, note the last known result. If you can't run them, say so — don't guess.

**Open questions:** re-read the spec's "Open Questions" section if it exists. Flag any that are still unresolved.

---

## Output Format

Print this snapshot clearly:

```
## SimFlow Project Status

### Spec
- File: docs/simflow/specs/<filename>
- Topic: <what's being built>
- Written: <date>
- Open questions: <N unresolved / none>

### Implementation Progress
- Tasks committed: [list from git log, e.g. "auth middleware (2d ago)", "user endpoint (2d ago)"]
- Tasks outstanding: [what the spec describes that hasn't been committed yet]
- Uncommitted changes: [yes — describe / no]

### Tests
- Last run: <date / unknown>
- Result: <X passed, Y failed / not yet run / could not determine>

### Last Activity
- <timestamp> — <what was last committed>

### Suggested Next Step
[One sentence: the most logical thing to do next based on the above state]
```

If there is no spec file: say so and suggest `simflow:understand` to create one.

If the git repo has no commits yet: say so and suggest `simflow:understand` or `simflow:build`.

Keep the output scannable. No paragraphs — just the facts.
