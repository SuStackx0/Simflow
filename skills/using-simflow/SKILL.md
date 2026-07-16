---
name: using-simflow
description: Bootstrap skill - always loaded at session start. Explains SimFlow skills and when to trigger them.
---

# SimFlow

You have SimFlow — a lightweight full dev lifecycle plugin. SimFlow gives you skills and agents to understand, build, test, review, and debug software without ceremony.

## Available Skills

Use the `Skill` tool to invoke any of these:

| Skill | When to use |
|---|---|
| `simflow:understand` | User has a new idea, feature, or plan to work through |
| `simflow:build` | User wants to implement something (from spec or inline description) |
| `simflow:dispatch` | User wants to fan out independent tasks to agents in parallel |
| `simflow:test` | User wants to test what was built |
| `simflow:review` | User wants to check implementation against requirements |
| `simflow:debug` | Something is broken or behaving unexpectedly |

## Triggering Rules

**Auto-trigger** a skill when the user's message clearly matches its purpose — even if they don't name it explicitly. Examples:
- "I have an idea for..." → `simflow:understand`
- "Build this", "implement", "add a feature" → `simflow:build`
- "Run in parallel", "fan out" → `simflow:dispatch`
- "Test this", "run tests" → `simflow:test`
- "Does this match the spec?", "check requirements" → `simflow:review`
- "It's broken", "not working", "error", "bug" → `simflow:debug`

**Manual trigger**: User can always invoke a skill directly with `/simflow:skill-name`.

If in doubt, invoke the skill. The skill will tell you if it doesn't apply.

## Core Rules (apply everywhere)

- **Git commits are always in the user's name only.** Never add Co-Authored-By or any other author attribution. Use the user's configured git name and email exactly as set in git config.
- **No forced order.** Any skill can be the starting point. Skills auto-detect prior outputs (like spec files).
- **Trust the user.** Don't add ceremony, gates, or steps they haven't asked for.
