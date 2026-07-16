---
name: build
description: Use when the user wants to implement something. Reads the spec (or accepts an inline description), dispatches agents to build task by task, and quizzes the user after each task before committing.
---

# Build

Reads a spec (or an inline description of what to build), breaks the work into tasks using the planner agent, then implements each task — pausing after each one for the user to review and pass a 2-question quiz before committing.

## Flow

```
Read spec or accept inline description
    ↓
simflow-planner: break into tasks (parallel vs sequential)
    ↓
For each task:
    simflow-implementer: build the task
        ↓
    Present code/design to user for review
        ↓
    2 MCQs about this task
        ↓ (wrong)
    Ask user to re-read → new MCQs → repeat
        ↓ (both correct)
    Commit in user's name only
        ↓
All tasks done → suggest simflow:test
```

## Input Detection

Check for an existing spec file first:
- Look for `docs/simflow/specs/*.md` — use the most recent one
- If multiple exist, ask the user which one to use
- If none exists, ask the user to describe what to build inline

## Task Planning

Dispatch the `simflow-planner` agent with the spec or inline description. The planner returns:
- A list of tasks with clear scope
- Which tasks are independent (can run in parallel)
- Which tasks have dependencies (must run sequentially)
- Which agent is best suited for each task

Use `simflow:dispatch` to run independent tasks in parallel.

## Per-Task Flow

For each task (after implementation):

1. **Present the work** — show the user what was built: key files changed, key decisions made, anything non-obvious.

2. **Ask for review** — say:
   > "Take a look at the code above. Let me know when you're ready for the quiz."

   Wait for confirmation.

3. **Quiz** — generate **2 MCQs** about this specific task. Test understanding of key implementation decisions or constraints — not trivia.

   Format:
   ```
   Q1. [Question about a key decision in this task]
   A) ...
   B) ...
   C) ...
   D) ...

   Q2. [Question about how this task integrates or a constraint it must meet]
   A) ...
   B) ...
   C) ...
   D) ...
   ```

   **Both must be correct to commit.**

   If wrong:
   > "Take another look at [specific area]. Come back when you're ready and I'll give you fresh questions."

   Generate new questions and repeat until both correct.

4. **Commit** — commit only this task's changes:
   ```bash
   git add <changed files>
   git commit -m "<short description of what this task built>"
   ```
   **Critical:** Use only the user's configured git name and email. No Co-Authored-By. No additional authors.

5. Move to the next task.

## Completion

When all tasks are done:
> "All tasks complete. Want me to run `simflow:test` to verify everything works?"
