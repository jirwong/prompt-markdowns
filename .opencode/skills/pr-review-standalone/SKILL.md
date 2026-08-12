---
name: pr-review-standalone
description: Run a strict human-in-the-loop migration/refactoring workflow with per-PR sub-agent review on the current repository. Triggers on PR review, migration, refactoring, human-in-the-loop, sub-agent review, design doc, plan doc, PR-by-PR execution.
---

# Operating rules

The objective is whatever the user asked for when they invoked this skill.

# Context
The repository in the current folder.

Please obey the following Operating Rules.

# Operating Rules: Strict Human-in-the-Loop with Sub-Agent PR Review

You must follow a strict, sequential workflow. Do NOT attempt to complete the entire migration at once. You must halt and wait for my explicit approval at defined checkpoints.

## Phase 0: Brainstorming & Task Breakdown

Work through the following brainstorm steps before drafting any plan. Do not skip ahead, and do not write implementation code during this phase.

1. **Analyze.** Examine the structure, dependencies, and logic of the source repository.
2. **Clarify.** Ask me clarifying questions one at a time to understand my purpose, constraints, and success criteria. Prefer multiple-choice questions when possible. Do not overwhelm me with several questions in one message.
3. **Propose approaches.** Present 2–3 different approaches for the work, with trade-offs and your recommendation. Lead with your recommended option and explain why.
4. **Present a design.** Once the approach is settled, present a concise design covering what will change and how the pieces fit together. Wait for my approval before proceeding.
5. **Write the design doc.** Once I approve the design, write it to a markdown design document (for example `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`, or a path I specify) and commit it. This is the durable record of the approved design that Phase 1 must not drift from.
6. **Self-review the design doc.** Review it with fresh eyes: scan for placeholders or TBDs, check that sections don't contradict each other, confirm the scope is right for a single plan, and resolve any ambiguity by picking one interpretation and making it explicit. Fix any issues inline, then re-commit.
7. **STOP AND WAIT.** Ask me to review the design doc before you draft the plan. Do not proceed to planning until I approve it.
8. **Draft the plan.** Draft a sequential migration plan. Start with a **Global Constraints** section listing the project-wide rules every task must obey, copied verbatim from the design doc and the Objective/Context (version floors, naming rules, platform requirements, and any rule like "Electron-compatible, no Node-only APIs"). Break the plan down into small, atomic tasks. Each task must represent a single logical feature or structural change that can be reviewed independently in a single Pull Request. For each task, specify the exact files involved, the interfaces it produces and consumes, the tests it requires, and break the task into small bite-sized steps (write failing test → verify it fails → implement → verify it passes → commit), with a commit after each step so each review diff stays small and reviewable.
9. **Self-review the plan.** Review it with fresh eyes: scan for placeholders or TBDs; confirm every task traces back to a decision in the design doc and to your stated Objective; and check that interfaces, function names, and types are consistent across tasks — a name used one way in an early task and differently in a later task is a bug. Fix any issues inline, then re-commit.
10. **Write the plan doc.** Write the plan to a markdown plan document (for example `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`, or a path I specify) and commit it. This is the durable contract that Phase 1 executes from.
11. **STOP AND WAIT.** Do not write any implementation code. Ask me to review and approve the task list.

## Phase 1: Iterative Execution with Sub-Agent PR Review (PR-by-PR)

Once I explicitly approve the task list from Phase 0, execute the tasks one by one using the following strict loop:

1. **Branch:** Create and checkout a new git branch for the current task, based on the current state of `main`.
2. **Implement (TDD):** Execute the task's bite-sized steps from the plan doc using test-driven development: for each step, write a failing test first, verify it fails, implement the minimal code to make it pass, verify the test passes, and commit that step before moving on.
3. **Commit & Push:** After the task's final step is committed, push the branch to the remote.
4. **Open PR:** Create a Pull Request against this repository using the `gh` CLI:
   ```bash
   gh pr create --title "<descriptive title>" --body "<summary of what changed and why>"
   ```
5. **Dispatch Sub-Agent Review:** Launch a separate, fresh sub agent (no shared conversation state) via the `task` tool with:
   - The PR URL and PR number.
   - The branch name.
   - The plan task this PR implements (from the plan doc), so the sub agent can check spec compliance against it.
   - The full text of the "Sub-Agent Review Instructions" block in `reference/sub-agent-review-instructions.md`.
6. **Sub-Agent Reviews:** The sub agent checks out the PR branch, performs a general review (code quality, correctness, structure, and test/lint/build status), posts review comments on the PR via `gh`, and submits an approval or change-request via `gh pr review`.
7. **Resolve Comments:** Read the sub agent's PR comments, address each one, push the fixes, and reply on each comment thread. Do not resolve a thread without a reply explaining what you changed.
8. **Re-review:** Repeat steps 5–7 until the sub agent approves the PR **or 3 review rounds have been exhausted**. If 3 rounds are exhausted without approval, STOP and escalate to me with a summary of the unresolved comments.
9. **STOP AND WAIT.** Once the sub agent approves, halt all execution and present me the PR URL plus a summary of what changed and what the sub agent approved.
10. **You Review & Merge:** I will review the PR myself and merge it manually. The agent NEVER merges. Only after the PR is merged may you checkout `main`, pull the latest changes, and begin step 1 for the next task.

## Phase 2: Final Whole-Branch Review

Once every task in the approved plan has been merged, before declaring the work complete:

1. **Dispatch a final review:** Launch a separate, fresh sub agent via the `task` tool to review the entire branch against the plan doc and design doc — not just one PR. It checks the complete change set for cross-task issues: inconsistent naming, duplication introduced across PRs, drift between the design doc and the final code, and anything a per-PR review would miss.
2. **Resolve findings:** Address any Critical or Important findings from the final review with the same fix-and-re-review loop as Phase 1.
3. **STOP AND WAIT.** Once the final review is clean, halt and present me a summary of the whole branch: what changed, the design doc and plan doc paths, the merged PR list, and the final review verdict. I make the final call.

## General Rules: Termination, Escalation, and Error Handling

- **Max 3 review rounds** per PR. After the third round, halt and escalate to me — unless the sub agent approved on that third round, in which case stop and present the PR as in Phase 1 step 9. Never silently exceed the cap.
- **Evidence over assertions.** The sub agent must run tests, lint, and build and report their actual output before approving. You must do the same when resolving comments.
- **Fresh eyes.** The sub agent is always a separate, fresh agent with no shared conversation state with you.
- **Never merge.** Merging is always done manually by me. You never merge, never use `gh pr merge`, and never auto-approve your own PR.
- **Operational failures halt execution.** If a push fails, PR creation fails, `gh` auth is missing, or any command errors unexpectedly, STOP and report the error to me. Do not work around failures silently.
- **Track progress durably.** Record each completed step, task, and PR in a progress file as you go (for example `.superpowers/sdd/progress.md`, or a path I specify), so your place survives context compaction. Consult it before assuming a task is done — trust the recorded progress and git history over memory.
- **My word overrides the loop.** I may interject, comment, redirect, or change scope at any point. My instructions always take precedence over the workflow steps.

---

## Sub-Agent Review Instructions

Deliver the "Sub-Agent Review Instructions" block from `reference/sub-agent-review-instructions.md` verbatim to the sub agent.
