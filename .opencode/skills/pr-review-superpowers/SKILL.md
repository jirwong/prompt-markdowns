---
name: pr-review-superpowers
description: Run a strict human-in-the-loop PR-by-PR workflow that delegates brainstorming to the superpowers brainstorming skill and planning to writing-plans, with per-PR sub-agent review. Triggers on PR review, migration, refactoring, human-in-the-loop, sub-agent review, superpowers, PR-by-PR execution.
---

# Operating rules

The objective is whatever the user asked for when they invoked this skill.

# Context
The repository in the current folder.

Please obey the following Operating Rules.

# Operating Rules: Strict Human-in-the-Loop with Sub-Agent PR Review

You must follow a strict, sequential workflow. Do NOT attempt to complete the entire implementation at once. You must halt and wait for my explicit approval at defined checkpoints.

## Phase 0: Brainstorming & Task Breakdown
1. Invoke the superpowers brainstorming skill.
2. Analyze the structure, dependencies, and logic of the source repository.
3. Invoke the superpowers writing-plans skill.
4. Draft a sequential implementation plan. Break the plan down into small, atomic tasks. Each task must represent a single logical feature or structural change that can be reviewed independently in a single Pull Request.
5. **STOP AND WAIT.** Do not write any implementation code. Ask me to review and approve the task list.

## Phase 1: Iterative Execution with Sub-Agent PR Review (PR-by-PR)

Once I explicitly approve the task list from Phase 0, execute the tasks one by one using the following strict loop:

1. **Branch:** Create and checkout a new git branch for the current task, based on the current state of `main`.
2. **Implement:** Write the code to fulfill the task.
3. **Commit & Push:** Commit the changes with a descriptive message and push the branch to the remote.
4. **Open PR:** Create a Pull Request against this repository using the `gh` CLI:
   ```bash
   gh pr create --title "<descriptive title>" --body "<summary of what changed and why>"
   ```
5. **Dispatch Sub-Agent Review:** Launch a separate, fresh sub agent (no shared conversation state) via the `task` tool with:
   - The PR URL and PR number.
   - The branch name.
   - The full text of the "Sub-Agent Review Instructions" block in `reference/sub-agent-review-instructions.md`.
6. **Sub-Agent Reviews:** The sub agent checks out the PR branch, performs a general review (code quality, correctness, structure, and test/lint/build status), posts review comments on the PR via `gh`, and submits an approval or change-request via `gh pr review`.
7. **Resolve Comments:** Read the sub agent's PR comments, address each one, push the fixes, and reply on each comment thread. Do not resolve a thread without a reply explaining what you changed.
8. **Re-review:** Repeat steps 5–7 until the sub agent approves the PR **or 3 review rounds have been exhausted**. If 3 rounds are exhausted without approval, STOP and escalate to me with a summary of the unresolved comments.
9. **STOP AND WAIT.** Once the sub agent approves, halt all execution and present me the PR URL plus a summary of what changed and what the sub agent approved.
10. **You Review & Merge:** I will review the PR myself and merge it manually. The agent NEVER merges. Only after the PR is merged may you checkout `main`, pull the latest changes, and begin step 1 for the next task.

## General Rules: Termination, Escalation, and Error Handling

- **Max 3 review rounds** per PR. After the third round, halt and escalate to me — unless the sub agent approved on that third round, in which case stop and present the PR as in Phase 1 step 9. Never silently exceed the cap.
- **Evidence over assertions.** The sub agent must run tests, lint, and build and report their actual output before approving. You must do the same when resolving comments.
- **Fresh eyes.** The sub agent is always a separate, fresh agent with no shared conversation state with you.
- **Never merge.** Merging is always done manually by me. You never merge, never use `gh pr merge`, and never auto-approve your own PR.
- **Operational failures halt execution.** If a push fails, PR creation fails, `gh` auth is missing, or any command errors unexpectedly, STOP and report the error to me. Do not work around failures silently.
- **My word overrides the loop.** I may interject, comment, redirect, or change scope at any point. My instructions always take precedence over the workflow steps.

---

## Sub-Agent Review Instructions

Deliver the "Sub-Agent Review Instructions" block from `reference/sub-agent-review-instructions.md` verbatim to the sub agent.
