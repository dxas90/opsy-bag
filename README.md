# opsy-bag

A lightweight starter repository for bootstrapping a new project with baseline contributor guidance and local development tooling.

## Current contents

- `README.md` for repository overview and setup notes
- `CLAUDE.md` for contributor and coding-agent conventions
- `.claude/` for local Claude workflows, rules, agents, and commands
- `mise.toml` for local toolchain bootstrap
- `.mcp.json` for MCP server configuration

## Getting started

1. Click **Use this template** on GitHub, or clone this repo directly.
2. Rename the repository to match your project.
3. Replace this README with your project-specific setup, run, and test instructions.
4. Update `CLAUDE.md` with any team-specific conventions.
5. Add your application code and supporting directories.

## Tooling

This template is configured with `mise` for local tool management.

- Python `3`
- `uv` `0.11.3`
- Automatic `.venv` creation through `mise`

If you use `mise`, run:

```bash
mise install
```

## What is not included yet

This repository does not currently include application source code, tests, CI workflows, or deployment configuration. Add those as you shape the project.

## Suggested first edits

- Replace the repository name and description.
- Add run, build, and test commands for your stack.
- Document required environment variables.
- Add source, test, and docs directories as needed.
- Add CI, deployment, and ownership details.

## Recommended structure

Adjust to your stack, but this layout is a good baseline:

```text
.
├── README.md
├── CLAUDE.md
├── src/
├── tests/
├── docs/
├── .claude/
└── .github/
```

## Development workflow

1. Create a feature branch from `main`.
2. Make focused changes.
3. Add or update tests.
4. Open a pull request with a short summary and validation notes.

## Local assistant files

The `.claude/` directory is intended for local assistant workflows and repository rules. Keep local-only overrides untracked according to `.claude/.gitignore`.

## Template customization checklist

- [ ] Project name, purpose, and scope
- [ ] Setup instructions
- [ ] Run and test commands
- [ ] CI/CD details
- [ ] Deployment notes
- [ ] Ownership and support contacts

## License

Add your license here (for example, MIT, Apache-2.0, or proprietary).
