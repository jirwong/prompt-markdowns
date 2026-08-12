# PR-Review Workflow Skills

A strict human-in-the-loop PR-review workflow, in two variants, packaged as
opencode skills.

## Install

Copy these skills into the target repository:

```bash
cp -r .opencode/skills/* <target-repo>/.opencode/skills/
```

opencode auto-loads skills from `.opencode/skills/` — no config change
required. After copying, quit and restart opencode so the new skills load.

## Usage

Invoke the workflow by asking for it (e.g., "review this change with
sub-agent PR review"), or the agent self-triggers via the skill descriptions.

## Variants

- **pr-review-standalone** — self-contained workflow with Phases 0
  (brainstorming & task breakdown), 1 (iterative per-PR execution with
  sub-agent review), and 2 (final whole-branch review).
- **pr-review-superpowers** — condensed variant (lighter Phase 0, no Phase 2)
  that delegates brainstorming to the superpowers brainstorming skill and
  planning to writing-plans. Requires superpowers to be installed in the
  target repo.
