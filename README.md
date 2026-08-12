# prompt-markdowns

A collection of reusable prompt/instruction templates for AI coding agents.

## Contents

- **[operating-rules-pr-review-standalone.md](prompts/operating-rules-pr-review-standalone.md)** — Full operating rules for a strict human-in-the-loop workflow with sub-agent PR review. Covers brainstorming & task breakdown (Phase 0), iterative execution with per-PR sub-agent review (Phase 1), and a final whole-branch review (Phase 2), plus general rules for termination, escalation, and error handling.

- **[operating-rules-pr-review.md](prompts/operating-rules-pr-review.md)** — A condensed version of the above workflow. Same human-in-the-loop structure and sub-agent PR review loop, but with a lighter Phase 0 (brainstorming and planning) and no standalone Phase 2.

Both templates are designed for refactoring/migration work on a target repository and prepare the code for use in an Electron app.

## Usage

Copy the desired template into your agent instructions and fill in the placeholders:

- `[FILE(S) TO EXAMINE]` — the files to refactor
- `[REPOSITORY]` — the target repository
