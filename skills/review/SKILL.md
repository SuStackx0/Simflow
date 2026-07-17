---
name: review
description: Use when the user wants to check if the implementation matches the spec and requirements. Finds gaps, triggers a build cycle for missing pieces, and loops until all requirements are satisfied. All commits in the user's name only.
---

# Review

Checks the implementation against the spec. If gaps exist, identifies exactly what's missing and triggers `simflow:build` for only those gaps. Loops until the reviewer returns a clean result.

## Flow

```
Locate spec + read implementation
        ↓
simflow-reviewer → PASS / PASS-WITH-NOTES / FAIL
        ↓ PASS or PASS-WITH-NOTES
Done — confirm all requirements satisfied
        ↓ FAIL
List exact gaps with locations
        ↓
If no spec exists → write inline requirements to a temp spec first
        ↓
Trigger simflow:build for gap tasks only
(full build flow including user review quiz and commit per task)
        ↓
Loop back to review
        ↓
After 3 full review→build cycles with the same gap still present:
    Stop and escalate to user
```

---

## Step 1: Locate Input

**Spec:** look in `docs/simflow/specs/` for the most recent file by YYYY-MM-DD filename prefix. If multiple exist on the same date, ask the user which to use.

**No spec:** ask the user to describe the requirements. Write those requirements to `docs/simflow/specs/YYYY-MM-DD-inline-requirements-spec.md` before invoking the reviewer — the reviewer agent requires a spec file to work from.

**Implementation:** read the files that correspond to the spec. If it's not obvious which files, check recent git commits and ask if needed.

---

## Step 2: Invoke Reviewer

Dispatch `simflow-reviewer` with:
- The full spec file content
- The implementation files to review

The reviewer returns one of three verdicts:
- **PASS**: every requirement satisfied — done
- **PASS-WITH-NOTES**: every requirement satisfied, non-blocking quality issues noted — done (notes are informational)
- **FAIL**: one or more requirements not met — proceed to Step 3

---

## Step 3: Fill Gaps

For each gap the reviewer identified:
- Note exactly what's missing and where it should be
- Track this gap with a counter (starts at 1 per unique gap)

Trigger `simflow:build` scoped to only the gap tasks. The full build flow applies — including the user review step and 2-question quiz before each commit. Do not skip these steps.

All commits during this phase: **user's git name and email only. No Co-Authored-By.**

---

## Step 4: Loop

After the gap build completes, automatically re-run the reviewer (return to Step 2). Do not wait for the user to invoke `simflow:review` again.

**At the start of every review cycle, print the cycle header:**
```
── Review cycle N of 3 ─────────────────────────
```
Replace N with the current cycle number (1, 2, 3). After the reviewer runs, also print:
```
Result: [PASS / PASS-WITH-NOTES / FAIL — X gap(s) remaining]
```

This keeps the user informed of progress without requiring them to ask.

**Loop limit:** if the same gap appears in the reviewer output for **3 consecutive cycles** without being resolved, stop the loop and escalate:
> "After 3 build attempts, `[gap description]` is still not resolved. This may indicate a conflict between the spec and the implementation approach, or the spec requirement may be unclear. Here's what was attempted: [summary]. How would you like to proceed?"

Wait for user direction before continuing.

---

## Completion

When the reviewer returns PASS or PASS-WITH-NOTES:
> "All requirements from the spec are satisfied. Implementation is complete."

List what was verified — give the user a clear record of what was checked.
