---
name: understand
description: Use when the user has a new idea, feature, or plan. Runs a combined brainstorm + grill session to make sure both parties fully understand what needs to be built, then writes a spec and quizzes the user before committing.
---

# Understand

A combined brainstorm and grill session. The goal: by the end, both you and the user have a complete, shared, unambiguous understanding of what needs to be built — including edge cases, constraints, and open questions.

## Flow

```
Explore project context
    ↓
Brainstorm (one question at a time)
    ↓
Grill mode (challenge assumptions, find gaps)
    ↓
Write spec file
    ↓
User reviews spec
    ↓
2 MCQs → both correct → commit in user's name
    ↓ (if wrong)
Ask user to re-read → new MCQs → repeat until both correct
```

## Phase 1: Explore

Before asking anything, explore the project:
- Check existing files, docs, recent commits
- Understand the current codebase structure and patterns
- Note anything that will affect the design

## Phase 2: Brainstorm

Ask clarifying questions **one at a time**. Prefer multiple choice when possible. Cover:
- What problem does this solve?
- Who uses it and how?
- What are the success criteria?
- What are the constraints?

Offer visual aids (mockups, diagrams, flow charts) when they would genuinely help clarify. Only offer once and only when the content is visual in nature.

## Phase 3: Grill

Once you understand the idea, switch to grill mode. Challenge it:
- "What happens when X fails?"
- "What if the user does Y?"
- "Have you considered Z edge case?"
- "This conflicts with what you said about A — which takes priority?"

Keep grilling until you're confident there are no hidden assumptions or gaps. Be direct. The goal is to surface problems before they become bugs.

## Phase 4: Spec File

Write the spec to: `docs/simflow/specs/YYYY-MM-DD-<topic>-spec.md`

The spec should cover:
- **What**: what is being built
- **Why**: the problem it solves
- **How**: the approach and architecture
- **Constraints**: what it must and must not do
- **Edge cases**: the scenarios surfaced during grill
- **Open questions**: anything still unresolved

Keep it concise. A short, clear spec beats a long vague one.

Then say:
> "Spec written to `<path>`. Please review it and let me know when you're ready for the quiz."

Wait for the user to confirm they've read it.

## Phase 5: Quiz

Generate **2 MCQs** that test whether the user genuinely understood the spec — not trivia, but key decisions or constraints that matter for implementation.

Format:
```
Q1. [Question about a key decision or constraint]
A) ...
B) ...
C) ...
D) ...

Q2. [Question about an important edge case or approach]
A) ...
B) ...
C) ...
D) ...
```

**Both must be correct to proceed.**

If the user gets one or both wrong:
> "Take another look at the spec — specifically the section on [relevant part]. Come back when you're ready and I'll give you a new set of questions."

Generate fresh questions (not the same ones) and repeat until both are correct.

## Phase 6: Commit

Once both MCQs are answered correctly, commit the spec:

```bash
git add docs/simflow/specs/<filename>.md
git commit -m "add spec: <topic>"
```

**Critical:** Use only the user's configured git name and email. No Co-Authored-By. No additional authors. Check `git config user.name` and `git config user.email` to confirm before committing.
