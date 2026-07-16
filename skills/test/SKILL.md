---
name: test
description: Use when the user wants to test the implementation. Dispatches the tester agent. Can be invoked standalone or suggested after build completes.
---

# Test

Runs the test phase using the `simflow-tester` agent. Works standalone or as the natural next step after `simflow:build`.

## Flow

```
Detect what was built (spec, recent commits, or user description)
    ↓
simflow-tester: design and execute test strategy
    ↓
Report results — what passed, what failed, what has no coverage
    ↓
If failures → surface to user with clear reproduction steps
    ↓
Optionally trigger simflow:debug for failures
```

## Input Detection

- Check recent commits to understand what was just built
- Check for a spec file in `docs/simflow/specs/` to understand expected behavior
- If neither exists, ask the user what to test and what the expected behavior is

## Test Strategy

The `simflow-tester` agent decides the appropriate test approach based on the project type and what was built. It will:
- Identify what needs testing based on the implementation and spec
- Run existing tests to catch regressions
- Write new tests for uncovered behavior
- Report coverage gaps

## Failures

For each failure, report:
- What failed and why
- How to reproduce it
- Whether it looks like a test issue or an implementation issue

If failures point to implementation bugs, suggest running `simflow:debug`.
