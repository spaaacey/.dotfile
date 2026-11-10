---
name: scope
description: ONLY invoke when the user's message explicitly contains the literal /scope command; never pick this skill on your own, even for change requests. Entry point for any change, trivial or big. Quickly researches the change, sizes it, then routes it - trivial: build via a Sonnet 5.5 implementer subagent; fits one session: grill, then build here; multi-session: grill, then write a plan doc that fresh agent sessions can execute without guessing.
argument-hint: <change description>
---

Change requested: $ARGUMENTS

If no change was given, ask for one (plain text, one line) and stop.

## 1. Quick research

Find the code the change touches: grep/read, or an Explore subagent if it spreads across many files. Check for existing components, helpers, flags or libraries that already do part of it. Keep it short: just enough to size the work, not to design it.

## 2. Size it

Pick exactly one tier:

- **Trivial**: ALL of the following hold:
  - only one reasonable way to do it; no product/UX/architecture decisions left open
  - small blast radius: a few files, no schema/IPC/persistence/public API changes
  - fits easily in this session
- **One-session**: has open decisions or real design work, but grilling plus building fits in this context window.
- **Multi-session**: too big for one window, splits naturally into several independent slices, or needs parallel agents.

When torn between two tiers, pick the bigger one: a needless grill costs a minute, a wrong assumption costs a rewrite.

State the verdict in one line with the reason, e.g. "Trivial: one-line CSS fix in X." or "Multi-session: touches IPC, persistence and 3 UI surfaces."

## 3. Route

### Trivial → Sonnet implementer subagent
Don't implement it yourself. Spawn the `implementer` subagent (foreground) and pass the Agent tool's `model` parameter as `"sonnet"` (Sonnet 5.5). Give it a self-contained brief, since it has no memory of this conversation:
- the change
- the files/locations you found
- the existing code to reuse
- how to verify it

Relay its report: what changed, how it was verified, commit SHAs, anything unverified. No grilling, no plan doc.

### One-session → grill, then build here
1. Call the Skill tool with "grilling". Seed it with your research findings so it doesn't re-ask facts you already have.
2. Once the user confirms shared understanding, build it in this same context. The conversation is the best source; don't write a plan doc.
3. If grilling shows the work is bigger than one session, say so and switch to the multi-session route.

### Multi-session → grill, plan, stop
1. Call the Skill tool with "grilling", seeded with your research findings.
2. Once the user confirms, do whatever research is still needed (APIs, docs, existing components, libraries) so no unknowns are left for the implementers.
3. Call the Skill tool with "to-plan".
4. Stop. Offer to build it: one `implementer` subagent per ticket, given the plan path and ticket number. Run every unblocked ticket in parallel, using `isolation: "worktree"` when more than one runs at once, and start newly unblocked tickets as their blockers land. Only start when the user says so.

Don't compact or clear between grilling and to-plan: the plan must be written from the live conversation, not a summary of it.
