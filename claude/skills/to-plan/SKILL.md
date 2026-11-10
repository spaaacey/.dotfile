---
name: to-plan
description: Turn the current conversation (usually a finished grilling) into a plan doc in the repo, with tickets that separate fresh agent sessions can execute without guessing. Synthesis only, no new interview.
argument-hint: [feature name]
disable-model-invocation: true
---

Write the agreed design from this conversation into a plan doc. Do NOT re-interview the user: synthesize what was already decided. The only question you ask is the ticket-breakdown check in step 3.

## 1. Gather

- Re-read the conversation: decisions (with their Q numbers if grilling numbered them), rejected alternatives, constraints.
- Explore the codebase wherever the tickets will need detail you don't have yet. Use the project's own vocabulary (e.g. docs/terminology.md if it exists).
- Look for refactoring that makes the change easier ("make the change easy, then make the easy change"). It becomes the first ticket(s).
- If an open decision turns up, don't guess: ask it as a grilling-style question (AskUserQuestion, recommended option first) before writing.

## 2. Draft tickets

Each ticket is a **vertical tracer-bullet slice**:
- cuts a narrow but complete path through every layer it needs (main/preload/renderer/UI/tests), not one horizontal layer
- verifiable or demoable on its own once done
- sized to fit one fresh agent context window
- declares **Blocked by**: the tickets that must land first ("None" if it can start right away)

Exception: a wide mechanical refactor (rename/retype across many call sites) goes expand → migrate in batches → contract, each step its own ticket.

## 3. Check the breakdown with the user

Show a numbered list: title, blocked by, what it delivers. Ask one question (AskUserQuestion): does the granularity and dependency order look right? Apply changes and repeat until they approve.

## 4. Write the doc

Path: follow the repo's convention (default `docs/<feature>-plan.md`). Template:

```markdown
# <Feature> plan

## Goal
What the user gets, from their point of view. 2-4 sentences.

## Non-goals
Explicitly out of scope.

## Decisions
- <decision> - <why> (Q<n>)
- Rejected: <alternative> - <why not>

## Context for implementers
Key files/modules, existing components/helpers to reuse, relevant docs, gotchas. Pointers, not copies.

## Tickets

### 1. <Title>
- **Blocked by:** None
- **Delivers:** the end-to-end behaviour this makes work
- **Where:** files/modules to start from (pointers; verify they still exist)
- **Steps:** concrete steps; never "figure out X" (that's an open decision, so resolve it first)
- **Done when:**
  - [ ] acceptance criterion
  - [ ] acceptance criterion
- **Verify:** exact commands and manual/UI checks per the repo's rules

### 2. ...

## Status
| # | Ticket | Status |
|---|--------|--------|
| 1 | ... | todo |
```

Inline a code snippet only if it pins a decision more precisely than prose can (type shape, state machine, schema). Keep it to the part that carries the decision.

## 5. Self-check

Re-read each ticket as a fresh agent that has only this doc and the repo. Anything you'd have to guess or ask about: fix the doc. If the fix needs a decision only the user can make, ask them; never ship a plan with open questions.

## 6. Report

The doc path, the ticket count, and which tickets can start in parallel right away. Don't commit unless the repo's rules or the user say to. Don't start implementing.
