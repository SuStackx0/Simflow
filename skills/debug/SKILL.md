---
name: debug
description: Use when something is broken, not working as expected, or throwing errors. Systematic root cause analysis first, then fix. Can dispatch debugger subagents when the problem spans multiple areas.
---

# Debug

Systematic debugging. Always find the root cause before proposing a fix. Can fan out `simflow-debugger` subagents when the problem is complex or spans multiple parts of the codebase.

## Flow

```
Understand the problem (symptom, reproduction steps, context)
    ↓
Hypothesize root causes
    ↓
Simple problem → single simflow-debugger agent
Complex / multi-area problem → dispatch multiple simflow-debugger agents in parallel
    ↓
Confirm root cause before fixing
    ↓
Apply fix
    ↓
Verify fix resolves the original symptom
    ↓
Commit in user's name only
```

## Problem Understanding

Before dispatching any agent, gather:
- **Symptom**: what is the actual incorrect behavior?
- **Expected**: what should be happening instead?
- **Reproduction**: what steps trigger it reliably?
- **Context**: recent changes, environment, relevant files

If the user hasn't provided these, ask — don't start debugging blind.

## Root Cause First

Never skip straight to a fix. A symptom can have multiple possible causes. The debugger agent must:
1. Identify the most likely root causes (ranked by likelihood)
2. Verify the actual root cause through inspection or testing
3. Only then propose a fix

## Multi-Agent Dispatch

Fan out multiple `simflow-debugger` agents when:
- The bug could originate in more than one layer (e.g., frontend and backend)
- There are multiple independent hypotheses worth investigating simultaneously
- The codebase is large enough that one agent can't hold enough context

Use `simflow:dispatch` for parallel agent fan-out.

## Fix and Verify

After the fix:
- Confirm the original symptom no longer reproduces
- Check that the fix doesn't break adjacent behavior
- Commit the fix:
  ```bash
  git add <changed files>
  git commit -m "fix: <short description of what was fixed>"
  ```
  **Critical:** Use only the user's configured git name and email. No Co-Authored-By. No additional authors.
