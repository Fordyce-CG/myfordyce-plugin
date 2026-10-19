---
name: review-agent-work
description: Use when a person asks what their agents are doing, what is waiting for them, or wants to accept or send back finished work in My Fordyce. Covers the board, Review, triage and pull request checks.
---

# Seeing and reviewing what agents did

Once several agents run at once, writing code stops being the slow step. Reading it is. This skill helps a person see the state of the work and act on what is waiting for them.

## Show the state of the work

- `open_kanban_board` opens the board in chat for a project. Use it when someone asks "what is everyone working on".
- `list_tasks` gives summary rows; filter by status to answer "what is in progress" or "what is waiting at Review".
- `list_task_statuses` gives the project's columns in board order, including which moves a column allows.

Lead with what needs the person: tickets waiting at Review, tickets on hold for a decision (`list_triage`), and tickets whose pull request failed its checks.

## Review finished work

For each ticket at Review, show the person:

1. The acceptance criteria and the evidence recorded against each one
2. The agent's release summary, including what it tried that failed
3. The pull request, if there is one. `verify_pr` shows which agent and which ticket produced a pull request, or that it was not produced under a tracked lease

Then do what the person decides:

- **Accept:** move the ticket to done with `move_task`.
- **Send back:** move it back to in progress with a note saying what to change. It returns to the plan for the next agent.

Do not accept or send back work on your own judgment. The person decides.

## When to land and when to send back

A useful rule: when work is mostly right and the ticket was small, land it and file the fix as a new ticket. Send it back when it misses an acceptance criterion or was built the wrong way.
