# CLAUDE.md

This file defines working conventions for contributors and coding agents in this repository.

## Repository intent

This repository is a starter template. Keep the structure simple, readable, and easy to adapt as product code is added.

## Current repository shape

- The repo currently contains documentation and local tooling scaffolding.
- `mise.toml` manages the default local Python and `uv` toolchain.
- `.claude/` contains assistant configuration, rules, and local workflows.
- Project-specific source, tests, and automation should be added deliberately instead of assumed.

## Core principles

- Prefer small, focused commits.
- Keep changes easy to review.
- Do not include secrets or credentials in source control.
- Update docs whenever behavior or setup changes.

## Code change expectations

- Preserve existing style unless a refactor is explicitly requested.
- Avoid unrelated edits in the same PR.
- Add tests for behavior changes where applicable.
- Keep public APIs stable unless changes are documented.

## Documentation expectations

- `README.md` should always explain setup, run, and test workflows.
- If no run or test workflow exists yet, state that explicitly instead of leaving placeholders.
- Add architecture or decision notes under `docs/` when complexity grows.
- Record non-obvious assumptions directly in code comments or docs.

## Branch and PR guidance

- Use short-lived feature/fix branches.
- Open PRs with a clear summary of what changed, why it changed, and how it was validated.

## For AI coding assistants

- Make the smallest safe change that satisfies the request.
- Validate edits (tests/lint/build) when possible.
- If constraints conflict, prioritize safety and correctness.
- If unsure about product intent, ask for clarification before broad changes.
- Keep documentation aligned with the repository's actual contents and available commands.

## Local-only files

Local assistant and editor config files should remain untracked per `.claude/.gitignore`.
