# SimFlow

A lightweight full dev lifecycle plugin for Claude Code and Codex. SimFlow gives you a structured workflow — understand, build, test, review, debug — without the ceremony.

Built to be simple: fewer gates, less boilerplate, more trust in the developer.

---

## What It Does

SimFlow provides 6 skills and 5 agents that work together across the full dev lifecycle:

| Skill | What it does |
|---|---|
| `simflow:understand` | Brainstorm + grill an idea → write spec → quiz you on it → commit |
| `simflow:build` | Read spec → plan tasks → implement → review each task → commit |
| `simflow:dispatch` | Fan out independent tasks to agents in parallel |
| `simflow:test` | Run tests, find gaps, write new tests, commit them |
| `simflow:review` | Check implementation against spec → fix gaps → loop until clean |
| `simflow:debug` | Systematic root cause analysis → fix → verify → commit |

**You can start anywhere.** No forced order — start with `debug` when something's broken, `build` when you already know what to make, `test` whenever you're ready. Skills auto-detect prior outputs like spec files.

**All commits are in your name only.** SimFlow never adds Co-Authored-By or any additional author attribution.

---

## Installation

### Claude Code

1. Add SimFlow as a plugin in your `~/.claude/settings.json`:

```json
{
  "enabledPlugins": {
    "simflow@simflow-marketplace": true
  },
  "extraKnownMarketplaces": {
    "simflow-marketplace": {
      "source": {
        "source": "github",
        "repo": "SuStackx0/Simflow"
      }
    }
  }
}
```

2. Restart Claude Code. SimFlow loads automatically at every session start.

### Codex

1. Clone this repository into your Codex plugins directory:

```bash
git clone https://github.com/SuStackx0/Simflow ~/.codex/plugins/simflow
```

2. In your Codex config, add the plugin path and point to `hooks/hooks-codex.json`.

3. Restart Codex. The session-start hook injects SimFlow automatically.

---

## Skills

### `simflow:understand`

Use when you have a new idea, feature request, or plan you want to think through before building.

**Flow:**
1. Explores your project context
2. Asks clarifying questions one at a time (brainstorm phase)
3. Switches to adversarial mode — challenges assumptions, finds gaps (grill phase)
4. Writes a spec to `docs/simflow/specs/YYYY-MM-DD-<topic>-spec.md`
5. Asks you to review it
6. Quizzes you with 2 MCQs — both must be correct before moving on
7. Commits the spec in your name only

**Trigger it when:** you say "I have an idea for...", "I want to build...", "Let's plan..."

---

### `simflow:build`

Use when you're ready to implement — either from a spec or an inline description.

**Flow:**
1. Locates the spec or takes your inline description
2. Dispatches `simflow-planner` to break the work into tasks
3. For each task (or parallel group):
   - Implements using `simflow-implementer`
   - Presents the work for your review
   - Quizzes you with 2 MCQs about what was built
   - Commits on both correct — in your name only
4. Suggests `simflow:test` when all tasks are done

**Trigger it when:** you say "implement this", "build it", "add this feature", "write the code"

---

### `simflow:dispatch`

Use when you have multiple independent tasks that can run at the same time.

**Flow:**
1. Assigns each task to the right agent
2. Dispatches all concurrently via the Agent tool
3. Collects results and checks for conflicts
4. Reports what each agent did

**Trigger it when:** you say "run these in parallel", "fan out", "do these simultaneously"

For standalone use, give it specific tasks with clear goals — not vague descriptions.

---

### `simflow:test`

Use when you want to verify your implementation. Can run any time — not just after `build`.

**Flow:**
1. Reads the spec and recent commits (or your inline description)
2. Runs the existing test suite
3. Identifies uncovered behaviors and writes tests for them
4. Commits new tests in your name only
5. Surfaces implementation bugs → suggests `simflow:debug`

**Trigger it when:** you say "test this", "run tests", "check if it works"

---

### `simflow:review`

Use when you want to verify the implementation fully satisfies the spec.

**Flow:**
1. Reads the spec and implementation
2. Dispatches `simflow-reviewer` to check every requirement
3. If gaps found → triggers `simflow:build` for just the gaps
4. Loops until the reviewer returns PASS
5. Escalates after 3 cycles on the same gap

**Trigger it when:** you say "does this match the spec?", "check requirements", "is everything implemented?"

---

### `simflow:debug`

Use when something is broken.

**Flow:**
1. Gathers: symptom, expected behavior, reproduction steps, recent changes
2. Forms root cause hypotheses
3. Dispatches `simflow-debugger` (one or multiple in parallel for complex bugs)
4. Presents root cause to you with evidence — waits for your confirmation
5. Applies the fix
6. Runs test suite + original reproduction step to verify
7. Commits the fix in your name only

**Trigger it when:** you say "it's broken", "this isn't working", "I'm getting an error", "there's a bug"

---

## Agents

SimFlow includes 5 bundled agents. They are dispatched automatically by skills — you don't need to invoke them directly.

| Agent | Model | Role |
|---|---|---|
| `simflow-planner` | Opus | Breaks specs into tasks, assigns agents, maps dependencies |
| `simflow-implementer` | Sonnet | Executes coding tasks across any stack |
| `simflow-tester` | Sonnet | Writes and runs tests |
| `simflow-debugger` | Sonnet | Systematic root cause analysis |
| `simflow-reviewer` | Opus | Spec compliance and code quality review |

---

## Git Commit Policy

Every commit made by SimFlow uses only your configured git identity:

```bash
git config user.name   # your name
git config user.email  # your email
```

No Co-Authored-By. No additional authors. Zero exceptions. If your git config is not set, SimFlow will stop and ask you to configure it before committing anything.

---

## Contributing

Pull requests welcome. Keep it lean — SimFlow is deliberately simple. If you're adding a skill or agent, read the existing ones first and match the style.

---

## License

MIT — see [LICENSE](LICENSE).
