---
name: debug
description: Use when something is broken, not working as expected, or throwing errors. Root cause before fix, always. Can dispatch multiple debugger agents in parallel when the problem spans multiple areas. Commits fix in the user's name only.
---

# Debug

Systematic debugging. Always find the root cause before touching any code. Never apply a fix to a symptom.

## Flow

```
Gather: symptom + reproduction steps + context
        ↓
Form root cause hypotheses (ranked by likelihood)
        ↓
Single-area problem → one simflow-debugger
Multi-area problem → dispatch multiple simflow-debugger agents in parallel
        ↓
Present root cause + evidence to user → wait for confirmation
        ↓ root cause is in user's code
Apply fix → run test suite + original reproduction step
        ↓ both pass
Commit in user's name only
        ↓ root cause is in a dependency
Report with workaround options → wait for user approval before touching anything
        ↓ ambiguous (two equally likely causes)
Present both hypotheses with evidence → ask user for additional context
```

---

## Step 1: Gather Context

Before dispatching any agent, you need:
- **Symptom**: what is the actual incorrect behavior? (exact error message, wrong output, unexpected state)
- **Expected**: what should happen instead?
- **Reproduction**: what steps trigger it reliably? Can it be reproduced every time or only sometimes?
- **Context**: any recent changes (check `git log --oneline -20`), relevant files, environment details

If the user hasn't provided these, ask. Do not start debugging blind.

---

## Step 2: Hypothesize

Before dispatching agents, form your own ranked list of the most likely root causes based on what you know. This gives the debugger agent a starting point and prevents it from exploring in the wrong direction.

---

## Step 3: Dispatch

**Single-area problem** (bug clearly lives in one layer or file): dispatch one `simflow-debugger` agent.

**Multi-area problem** (bug could originate in multiple layers — e.g., frontend and backend, or two independent services): use `simflow:dispatch` to fan out multiple `simflow-debugger` agents in parallel, each investigating one hypothesis or area.

Provide each agent with: the symptom, reproduction steps, your hypotheses, and which area they should investigate.

**Ambiguous root cause (two equally likely causes, no distinguishing evidence from static analysis):** present both hypotheses to the user with your evidence for each:
> "I've narrowed it down to two equally likely root causes: [A] because [evidence], and [B] because [evidence]. Can you tell me more about [specific question that would distinguish them]?"

Wait for user input before proceeding.

---

## Step 4: Confirm Root Cause

Present the root cause and evidence to the user before applying any fix:
> "Root cause: [description]. Evidence: [specific file:line or log output]. Proposed fix: [what to change]. Shall I apply it?"

Wait for the user to confirm or redirect. Do not apply the fix without confirmation.

**If the root cause is in a third-party dependency:** do not apply a workaround without user approval:
> "The root cause is in `[dependency]` version [X], specifically [behavior]. This is outside your code. Options: [A — upgrade to version Y which has a fix], [B — apply this workaround in your code], [C — open an issue upstream]. Which do you want?"

---

## Step 5: Fix and Verify

After user confirms the fix approach:
1. Apply the fix
2. Run the project's existing test suite
3. Reproduce the original symptom and confirm it no longer occurs
4. Both must succeed before committing

If either fails: report what's still broken and return to Step 2.

---

## Step 6: Commit

```bash
git status
git add <only files changed by the fix>
git commit -m "fix: <imperative-mood description of what was fixed, ≤72 chars>"
```

Examples:
- `fix: handle null user in auth middleware`
- `fix: correct off-by-one in pagination offset`

**No Co-Authored-By. No additional authors. Only the user's configured git name and email.**
