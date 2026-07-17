# Changelog

## [1.1.0] — 2026-07-17

### Added
- `simflow:status` — project state snapshot (spec, progress, tests, next step)
- `simflow:iterate` — change/extend existing code with light grill, targeted plan, quiz, and commit
- `memory: project` on all 5 agents — codebase knowledge persists across sessions

### Improved
- `simflow:review` — shows cycle counter (`── Review cycle N of 3 ──`) and gap count at each loop iteration
- `simflow:understand` and `simflow:build` — quiz gate is now fully unbypassable with explicit `<HARD-GATE>` block listing every bypass attempt

---

## [1.0.0] — 2026-07-16

### Added
- `simflow:understand` — brainstorm + grill → spec → 2 MCQ quiz → commit
- `simflow:build` — spec → plan → implement per task → quiz → commit
- `simflow:dispatch` — parallel agent fan-out utility
- `simflow:test` — run and write tests, commit new coverage
- `simflow:review` — spec vs implementation gap check with react loop
- `simflow:debug` — systematic root cause analysis, multi-agent support
- `simflow:using-simflow` — bootstrap skill injected at every session start
- `simflow-planner` agent (Opus) — task decomposition and dependency mapping
- `simflow-implementer` agent (Sonnet) — stack-agnostic code implementation
- `simflow-tester` agent (Sonnet) — test strategy and execution
- `simflow-debugger` agent (Sonnet) — root cause analysis
- `simflow-reviewer` agent (Opus) — spec compliance review
- Session-start hook for Claude Code and Codex
- All commits enforced in user's name only — no Co-Authored-By
