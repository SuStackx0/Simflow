---
name: using-simflow
description: Bootstrap skill — always loaded at session start. Explains what SimFlow is, what skills exist, and when to trigger them automatically.
---

# SimFlow

You have SimFlow — a lightweight full dev lifecycle plugin for Claude Code and Codex. SimFlow gives you skills and agents to understand, build, test, review, and debug software without ceremony.

## Skills

Use the `Skill` tool to invoke any of these. The skill loads its full instructions — follow them exactly.

| Skill | What it does |
|---|---|
| `simflow:understand` | Brainstorm + grill an idea → write spec → quiz user → commit |
| `simflow:build` | Read spec or description → plan → implement task by task → quiz → commit |
| `simflow:dispatch` | Fan out independent tasks to agents in parallel |
| `simflow:test` | Run tests, identify gaps, write new tests, commit them |
| `simflow:review` | Check implementation against spec → fix gaps → loop until clean |
| `simflow:debug` | Systematic root cause analysis → confirm with user → fix → commit |

## Triggering Rules

**Auto-trigger** the matching skill immediately via the `Skill` tool — without asking for permission — when the user's message clearly matches a skill's purpose:

| User says something like... | Trigger |
|---|---|
| "I have an idea for...", "I want to build...", "Let's plan..." | `simflow:understand` |
| "Implement this", "Build it", "Add this feature", "Write the code" | `simflow:build` |
| "Run these in parallel", "Fan out", "Do these simultaneously" | `simflow:dispatch` |
| "Test this", "Run tests", "Check if it works" | `simflow:test` |
| "Does this match the spec?", "Check requirements", "Is everything implemented?" | `simflow:review` |
| "It's broken", "This isn't working", "I'm getting an error", "There's a bug" | `simflow:debug` |

**Manual trigger**: the user can always invoke a skill directly by naming it or typing `/simflow:skill-name`.

**When ambiguous**: if the message could match two skills, pick the one with the stronger signal and invoke it. If you genuinely cannot tell, ask one clarifying question first.

**For standalone `simflow:dispatch`**: the user should provide a list of specific independent tasks. If they say "run in parallel" with vague tasks, ask: "What are the specific tasks you want to run in parallel, and for each — what's the goal and which files are involved?"

## Core Rules

These apply everywhere in SimFlow, in every skill, in every agent:

- **Git commits are always in the user's name only.** Check `git config user.name` and `git config user.email` before committing. Never add Co-Authored-By. Never add any additional author. Zero exceptions.
- **No forced entry point.** Any skill can start the session. Skills auto-detect prior outputs like spec files.
- **No guessing.** If you need information to proceed, ask. Don't invent requirements, file paths, or behaviors.
- **Trust the user.** Don't add steps, gates, or checks they haven't asked for.
