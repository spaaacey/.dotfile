---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Ask **exactly one question per message. Never more.** The **frontier** is every decision whose prerequisites are already settled — the questions you could ask _now_ without guessing at answers you haven't heard yet. Track the frontier internally, but surface only its single most consequential item: the one whose answer reshapes the most of the tree below it. Then stop and wait for the answer.

Ask each question with the **AskUserQuestion tool** (one question per call), never as plain text, so the UI notifies the user that input is needed:

- `header`: `Q<n>` (≤12 chars).
- `question`: the question title plus any needed context.
- `options`: the choices, with your recommended answer **first**, labelled with a trailing "(Recommended)". Put the reasoning in each option's `description`. Free-form answers come through the built-in "Other".

Number questions sequentially across the whole session (Q1, Q2, Q3…) so both of you can refer back to them — the numbering is a running count, not a list within one message.

Each answer reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next single question.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so if the fact-finding is slow, ask a frontier question that doesn't depend on it instead of stalling. The _decisions_ are the user's — put them one at a time and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
Once the user confirms: if another skill invoked this one (e.g. /scope), hand control back to it. Otherwise stop; the user decides what's next (e.g. build here, or /to-plan for a multi-session plan doc).
