---
name: dispatch
description: Use when you have 2 or more independent tasks that can run in parallel. Fan out agents concurrently using the Agent tool and collect results. Called standalone or internally by build and debug.
---

# Dispatch

Runs independent tasks in parallel using concurrent Agent tool invocations. Use this when tasks don't share state or depend on each other's output, and sequential execution would just be wasted time.

## When to Use

- Two or more independent features, fixes, or investigations that don't share files or state
- Parallel exploration across different parts of a codebase
- Fan-out from `simflow:build` when the planner identifies a parallel task group
- Any situation where one task doesn't need the output of another before starting

**Do not dispatch an agent for:**
- A task that changes fewer than 5 lines or touches only a single config value — handle it directly
- Tasks that share files and would produce conflicting edits — run sequentially
- Tasks where one depends on the result of another — run sequentially

---

## Flow

```
Identify independent tasks
        ↓
Assign each task to the right agent
        ↓
Dispatch all tasks concurrently (Agent tool, one invocation per task)
        ↓
Wait for all to complete
        ↓
Check for conflicts (same file modified by multiple agents)
        ↓ conflicts found
    Report conflicts to user before declaring completion
        ↓ no conflicts
Report what each agent built, what files changed
```

---

## Agent Assignment

Match each task to the most suitable agent:

| Task type | Agent |
|---|---|
| Architectural decisions, task planning | `simflow-planner` |
| Writing code, implementing features | `simflow-implementer` |
| Writing or running tests | `simflow-tester` |
| Investigating bugs | `simflow-debugger` |
| Reviewing code or spec compliance | `simflow-reviewer` |
| General research, unknown type | Handle directly without spawning an agent |

---

## Dispatching

Use the **Agent tool** with multiple concurrent invocations — one per task. Launch all of them in a single response so they run in parallel. Each agent invocation should include:
- The task description
- Relevant context (spec path, files to read, overall goal)
- What output format you expect back

---

## After Completion

**Conflict detection**: collect the list of files each agent modified. If any file appears in more than one agent's output, that is a conflict. Report it:
> "Conflict: both [Agent A] and [Agent B] modified `<file>`. Here's what each changed: [summary]. Which should take precedence?"

Wait for user direction before resolving.

**If any agent failed**: report all results first (including successes), then surface the failures clearly with enough context to retry or redirect.

**Output summary**: for each agent, report what was built and which files were changed. Keep it scannable — one or two lines per agent.
