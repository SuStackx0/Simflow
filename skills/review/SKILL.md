---
name: review
description: Use when the user wants to check if the implementation matches the spec and requirements. Finds gaps, triggers a build cycle for any missing pieces, and loops until everything is satisfied.
---

# Review

Checks the implementation against the spec. If gaps exist, identifies exactly what's missing and triggers a `simflow:build` cycle to fill them. Loops until all requirements are met.

## Flow

```
Read spec (docs/simflow/specs/) + read implementation
    ↓
simflow-reviewer: check every requirement against the code
    ↓
No gaps → done, confirm spec is fully implemented
    ↓ (gaps found)
List exactly what's missing or incomplete
    ↓
Trigger simflow:build for the gap tasks only
    ↓
Loop back to review
    ↓
Repeat until all requirements are satisfied
```

## Input Detection

- Look for the most recent spec in `docs/simflow/specs/`
- If multiple specs exist, ask the user which one to check against
- If no spec exists, ask the user to describe the requirements to check against

## Gap Analysis

The `simflow-reviewer` agent checks:
- Every stated requirement: is it implemented?
- Every edge case in the spec: is it handled?
- Interface contracts: do components interact as specified?
- Constraints: are all must/must-not conditions respected?

For each gap, the reviewer provides:
- The specific requirement that isn't met
- Where in the codebase it should be implemented
- What needs to change

## React Loop

After `simflow:build` fills the gaps, re-run the review automatically. Don't wait for the user to invoke it again — the loop continues until the reviewer confirms everything is satisfied.

Only exit the loop when the reviewer returns a clean result.

## Completion

When all requirements are met:
> "All requirements from the spec are satisfied. Implementation is complete."

List what was verified so the user has a clear record.
