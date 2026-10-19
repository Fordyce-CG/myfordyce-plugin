---
name: run-agent-work
description: Use when a coding agent is taking work from a My Fordyce backlog. Covers getting the next ticket, keeping the claim alive, declaring the files it will change, recording a pull request, and handing the ticket back with a summary the next agent can start from.
---

# Working a ticket from My Fordyce

My Fordyce hands each agent one ticket that no other agent holds. The ticket is held on a lease the agent renews while it works. If the agent goes quiet, the lease lapses and the ticket returns to the plan for the next agent, with the last hand-off note attached.

## The loop

1. **Identify yourself.** Pass the same `agent_id` on every call (for example `claude-code@laptop`). Leases and the ticket's history are recorded against it.
2. **Get work.** Call `request_work` with the project. It returns one ready ticket and a `lease_token`, or `no_work_available`. If there is no work, stop. Use `claim` when a person has named a specific ticket.
3. **Read the contract.** Read the ticket's scope and acceptance criteria with `get_task` before changing anything. Do what the scope says and nothing outside it.
4. **Say which files you will change.** Call `declare` with the repo-relative paths. If another live ticket already holds a path, the reply names the holder and how long they have left. Add paths later with `extend` as the work grows.
5. **Keep the claim alive.** Saving progress with `patch` (pass `expected_version`) renews the lease. If you are working without writing, call `heartbeat`. On a version conflict, read the ticket again and retry just your change.
6. **Record evidence.** For each acceptance criterion, add evidence with `add_evidence` and the criterion's id: a test result, a screenshot link, the live URL.
7. **Open the pull request.** When the code is ready, call `open_pr` with the PR URL. This ends your lease while CI runs. Report what happens next with `report_phase` (`ci_passed`, `ci_failed`, `merged`, `verified`).
8. **Hand it back.** Call `release` with outcome `done` to send finished work to Review, or `abandon` to return the ticket to the plan.

## Writing the release summary

The next agent, or the person reviewing, starts from your summary and not from your context. Write it for them:

- What you changed, with the branch or pull request
- What you tested and the result
- Decisions you made and why
- What is left, and anything a person needs to decide
- What you tried that did not work, and why

The last point matters most. Agents report what they did and rarely volunteer what failed.

Evidence that arrives later, such as a CI run or the merge, goes on the attempt with `add_evidence`. Do not rewrite the ticket to add it.

## Finished or blocked

- **Finished work goes to Review**, even when a person still has to check it. Review is where people check work.
- **A blocked ticket** is one where agent work remains but cannot continue: a decision, a dependency, an outside step. Set `blocked_by` or explain in the release note and release with `abandon`. Do not hold the lease while you wait.
- If a reply says a person has asked for the ticket back, finish the current step, release with a summary, and start nothing new on it.
