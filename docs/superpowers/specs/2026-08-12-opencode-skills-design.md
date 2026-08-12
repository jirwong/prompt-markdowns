# Design: Convert prompt-markdowns PR-review prompts to opencode skills

Date: 2026-08-12

## Objective

Convert the two prompt templates in `prompts/` into opencode skills that can be
copied into another repository and used there.

## Context

- `prompts/operating-rules-pr-review-standalone.md` — self-contained workflow
  with Phases 0 (brainstorming & task breakdown), 1 (iterative per-PR
  execution with sub-agent review), and 2 (final whole-branch review).
- `prompts/operating-rules-pr-review-superpowers.md` — condensed variant that
  invokes the superpowers brainstorming and writing-plans skills; lighter
  Phase 0 and no Phase 2.

Both are strict human-in-the-loop workflows: the agent never merges, PRs are
reviewed by a fresh sub-agent with at most 3 review rounds, and execution halts
at defined checkpoints for explicit user approval.

## Decisions

1. **Both variants are converted** to opencode skills.
2. **One skill per prompt** — no decomposition into orchestrator/reviewer or
   per-phase skills.
3. **Superpowers references are kept** in `pr-review-superpowers`. The target
   repo is expected to have superpowers installed. A dependency note is
   documented in the README.
4. **Skill location:** `.opencode/skills/<name>/SKILL.md` in this repo. This
   repo loads them (useful for testing), and the folder copies cleanly into the
   target repo.
5. **Skill names** mirror the source filenames:
   - `pr-review-standalone`
   - `pr-review-superpowers`
6. **Objective is contextual.** Replace `[INSERT YOUR OBJECTIVE HERE]` with an
   instruction that the objective is whatever the user asked for when invoking
   the skill.

## Structure (Approach C: lean skill + reference files)

```
.opencode/skills/
├── README.md                                  # install/copy instructions
├── pr-review-standalone/
│   ├── SKILL.md                               # lean: frontmatter + workflow phases + pointer to reference
│   └── reference/
│       └── sub-agent-review-instructions.md   # verbatim block (incl. spec-compliance step)
└── pr-review-superpowers/
    ├── SKILL.md                               # lean: frontmatter + workflow phases + pointer to reference
    └── reference/
        └── sub-agent-review-instructions.md   # verbatim block (no spec-compliance step)
```

### Frontmatter

`pr-review-standalone/SKILL.md`:

```markdown
---
name: pr-review-standalone
description: Run a strict human-in-the-loop migration/refactoring workflow with per-PR sub-agent review on the current repository. Triggers on PR review, migration, refactoring, human-in-the-loop, sub-agent review, design doc, plan doc, PR-by-PR execution.
---
```

`pr-review-superpowers/SKILL.md`:

```markdown
---
name: pr-review-superpowers
description: Run a strict human-in-the-loop PR-by-PR workflow that delegates brainstorming to the superpowers brainstorming skill and planning to writing-plans, with per-PR sub-agent review. Triggers on PR review, migration, refactoring, human-in-the-loop, sub-agent review, superpowers, PR-by-PR execution.
---
```

### Skill body content

Each `SKILL.md` body contains the workflow phases as-is, with these
adaptations:

- **Objective:** replace `[INSERT YOUR OBJECTIVE HERE]` with "The objective is
  whatever the user asked for when they invoked this skill."
- **Context:** keep "The repository in the current folder."
- **Sub-agent dispatch:** in the step that launches a fresh sub-agent, add
  "(use the `task` tool)" to make the opencode-native mechanism explicit.
- **Sub-Agent Review Instructions:** remove the verbatim block from the body
  and replace it with a pointer: "Deliver the 'Sub-Agent Review Instructions'
  block from `reference/sub-agent-review-instructions.md` verbatim to the sub
  agent."

Everything else (phases, checkpoints, max-3-rounds rule, never-merge,
error handling, escalation) is preserved verbatim from the source.

### Reference files

`reference/sub-agent-review-instructions.md` is copied verbatim from each
source prompt's block:
- `pr-review-standalone` keeps its spec-compliance step.
- `pr-review-superpowers` does not have that step.

### README.md (at `.opencode/skills/`)

- What these skills are: a strict human-in-the-loop PR-review workflow in two
  variants.
- Copy/install instructions:
  - `cp -r .opencode/skills/* <target-repo>/.opencode/skills/`
  - opencode auto-loads skills from `.opencode/skills/` — no config change
    required.
  - After copying, quit and restart opencode so the new skills load.
- Usage: invoke by asking for the workflow (e.g., "review this migration with
  sub-agent PR review"), or the agent self-triggers via the skill descriptions.
- Dependency note: `pr-review-superpowers` requires superpowers to be installed
  in the target repo (it invokes brainstorming and writing-plans).
  `pr-review-standalone` is self-contained.
- Keep it short — usage-focused, not a re-spec.

## Non-goals

- No decomposition into orchestrator/reviewer skills.
- No opencode config changes in this repo (skills auto-load from
  `.opencode/skills/`).
- No rewriting of the workflow logic itself.
