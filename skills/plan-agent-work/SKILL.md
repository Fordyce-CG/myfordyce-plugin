---
name: plan-agent-work
description: Use when a person asks Claude to add, split or organise tickets in My Fordyce for coding agents to work on. Covers writing tickets an agent can finish, sizing and splitting them, releases, drafts and duplicate checks.
---

# Planning tickets agents can finish

Agents rarely fail on hard problems. They fail on vague tickets. A ticket is ready for an agent when a person could check it is done by looking.

## What every ticket needs

- **Title:** a verb and a thing, one outcome. "Add CSV export to the orders page", not "Exports".
- **Outcome:** one sentence in the form "As a customer, I expect to get ___." If the blank needs more than one thing, split the ticket.
- **Why:** one or two sentences of intent the agent cannot work out on its own.
- **In scope / out of scope:** what changes, and what the agent must leave alone.
- **Paths:** the files or folders it is expected to touch, when known.
- **Acceptance criteria:** each one checkable by a person: what to look at and what they should see.

Use `validate` to check a planned ticket has the fields its project requires before you create it.

## Size

Tickets above the project's decomposition threshold (3 story points by default) work better split. Either create subtasks under a parent (`parent_task_id`), or plan the ticket as named stages with a commit at the end of each stage. Small tickets that land 90% right are cheap to finish; large ones are hard to review.

## Grouping

- Put tickets that ship together in the same `release`. `create_release_with_tasks` creates several at once.
- `list_releases` shows how far each release has got and which tickets are stuck.

## Drafts and duplicates

- Tickets an assistant drafts can wait as drafts until a person publishes them, depending on the project's settings. Tell the person when a ticket you created is waiting as a draft.
- Before creating a ticket, `find_duplicates` checks whether a similar open ticket exists. A new ticket that looks like an open one is still created but held for review until someone confirms or dismisses it. `list_triage` shows what is waiting for that decision.

## Before you create

Confirm the project with the person if it is not clear, and read back the tickets you are about to create when there are more than three.
