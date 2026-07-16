---
name: dispatch
description: Use when you have multiple independent tasks that can run in parallel. Fan out agents concurrently and collect results. Can be called standalone or used internally by other skills.
---

# Dispatch

A lightweight utility for running independent tasks in parallel using agents. Use this whenever tasks don't depend on each other's outputs and can be worked on simultaneously.

## When to Use

- Multiple independent features or fixes that don't share state
- Parallel research or exploration across different parts of a codebase
- Fan-out execution during `simflow:build` for independent task groups
- Any situation where sequential execution is unnecessary waiting

## Flow

```
Identify independent tasks
    ↓
Assign each task to the most suitable agent
    ↓
Dispatch all tasks concurrently
    ↓
Collect and synthesize results
    ↓
Report what was done and surface any conflicts or issues
```

## Task Assignment

Match each task to the right agent:
- **Architectural decisions, complex planning** → `simflow-planner`
- **Code implementation** → `simflow-implementer`
- **Testing** → `simflow-tester`
- **Debugging** → `simflow-debugger`
- **Code or spec review** → `simflow-reviewer`
- **General research or unknown** → general-purpose agent

Only dispatch agents when the task genuinely benefits from it. A task that takes 30 seconds doesn't need a dedicated agent.

## Conflict Detection

After all agents complete, check for conflicts:
- Did two agents modify the same file in incompatible ways?
- Are there assumptions made by one agent that conflict with another?
- Are there integration points that need attention?

Report any conflicts clearly before declaring completion.

## Output

Summarize what each agent did, what files were changed, and any issues that need the user's attention.
