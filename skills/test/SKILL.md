---
name: test
description: Use when the user wants to test the implementation. Dispatches the tester agent to run existing tests, identify coverage gaps, and write new tests. Commits new tests in the user's name only. Suggests debug for implementation failures.
---

# Test

Runs the test phase using `simflow-tester`. Works standalone from anywhere in the lifecycle — no prior `simflow:build` required.

## Flow

```
Detect what to test (spec + recent commits or inline description)
        ↓
simflow-tester: run existing tests → identify gaps → write new tests
        ↓
Existing tests:
    All pass → continue
    Failures → classify: implementation bug or test infrastructure issue?
        ↓
New tests written → commit in user's name only
        ↓
Report: what passed, what failed, what's covered, what's still a gap
        ↓ implementation bugs found
Suggest simflow:debug with specific failure details
```

---

## Step 1: Detect Input

**With spec + commits:** check `docs/simflow/specs/` for the most recent spec (by YYYY-MM-DD filename prefix), and check the last 10 commits (or since the last git tag if one exists) to understand what was just built.

**Without spec or commits:** ask the user: "What should I test, and what's the expected behavior?" Pass their description to `simflow-tester` as the working brief.

Both paths are valid. The tester agent adapts to whichever is available.

---

## Step 2: Run Tester Agent

Dispatch `simflow-tester` with:
- The spec content (or user's inline description)
- The list of recently changed files from git
- The project root path so it can discover the test framework

---

## Step 3: Handle Results

**Test failures — classify first:**

If failures look like **implementation bugs** (wrong output, uncaught exception, business logic error):
> "Tests found [N] implementation failures. Running `simflow:debug` — or do you want to look at the failures first?"
Trigger `simflow:debug` with the specific failure details.

If failures look like **test infrastructure issues** (wrong import path, missing test fixture, environment not configured):
Report them directly to the user — these are setup issues, not code bugs. Do not trigger debug.

---

## Step 4: Commit New Tests

After new tests are written and pass:

```bash
git status
git add <new test files only>
git commit -m "test: add coverage for <feature>"
```

**No Co-Authored-By. No additional authors. Only the user's configured git name and email.**

Do not commit failing tests.

---

## Output

Report in plain language:
- How many existing tests passed / failed
- What new tests were written and what behavior they cover
- Any behaviors that still have no test coverage (and whether that's acceptable or a gap)
- Next recommended action
