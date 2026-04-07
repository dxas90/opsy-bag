---
description: "Spawn an agent team using presets (review, debug, feature, fullstack, research, security, migration) or custom composition"
argument-hint: "<preset|custom> [--name team-name] [--members N] [--delegate]"
---

# Team Spawn

Spawn a multi-agent team using preset configurations or custom composition. Handles team creation, teammate spawning, and initial task setup.

## Pre-flight Checks

1. Verify that `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` is set:
   - If not set, inform the user: "Agent Teams requires the experimental feature flag. Set `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1` in your environment."
   - Stop execution if not enabled

2. Parse arguments from `$ARGUMENTS`:
   - First positional arg: preset name or "custom"
   - `--name`: team name (default: auto-generated from preset)
   - `--members N`: override default member count
   - `--delegate`: enter delegation mode after spawning

## Phase 1: Team Configuration

### Preset Teams

If a preset is specified, use these configurations:

**`review`** — Multi-dimensional code review (default: 3 members)

- Spawn 3 `Athos` agents with dimensions: security, performance, architecture
- Team name default: `review-team`

**`debug`** — Competing hypotheses debugging (default: 3 members)

- Spawn 3 `Porthos` agents, each assigned a different hypothesis
- Team name default: `debug-team`

**`feature`** — Parallel feature development (default: 3 members)

- Spawn 1 `DArtagnan` agent + 2 `Aramis` agents
- Team name default: `feature-team`

**`fullstack`** — Full-stack development (default: 4 members)

- Spawn 1 `Aramis` (frontend), 1 `Aramis` (backend), 1 `Aramis` (tests), 1 `DArtagnan`
- Team name default: `fullstack-team`

**`research`** — Parallel codebase, web, and documentation research (default: 3 members)

- Spawn 3 `general-purpose` agents, each assigned a different research question or area
- Agents have access to codebase search (Grep, Glob, Read) and web search (WebSearch, WebFetch)
- Team name default: `research-team`

**`security`** — Comprehensive security audit (default: 4 members)

- Spawn 1 `Athos` (OWASP/vulnerabilities), 1 `Athos` (auth/access control), 1 `Athos` (dependencies/supply chain), 1 `Athos` (secrets/configuration)
- Team name default: `security-team`

**`migration`** — Codebase migration or large refactor (default: 4 members)

- Spawn 1 `DArtagnan` (coordination + migration plan), 2 `Aramis` (parallel migration streams), 1 `Athos` (verify migration correctness)
- Team name default: `migration-team`

### Custom Composition

If "custom" is specified:

1. Use AskUserQuestion to prompt for team size (2-5 members)
2. For each member, ask for role selection: DArtagnan, Athos, Porthos, Aramis
3. Ask for team name if not provided via `--name`

## Phase 2: Team Creation

1. Use the `Teammate` tool with `operation: "spawnTeam"` to create the team
2. For each team member, use the `Task` tool with:
   - `team_name`: the team name
   - `name`: descriptive member name (e.g., "security-reviewer", "hypothesis-1")
   - `subagent_type`: "general-purpose" (teammates need full tool access)
   - `prompt`: Role-specific instructions referencing the appropriate agent definition

## Phase 3: Initial Setup

1. Use `TaskCreate` to create initial placeholder tasks for each teammate
2. Display team summary:
   - Team name
   - Member names and roles
   - Display mode (tmux/iTerm2/in-process)
3. If `--delegate` flag is set, transition to delegation mode

## Output

Display a formatted team summary:

```
Team "{team-name}" spawned successfully!

Members:
  - {member-1-name} ({role})
  - {member-2-name} ({role})
  - {member-3-name} ({role})

Use /team-status to monitor progress
Use /team-delegate to assign tasks
Use /team-shutdown to clean up
```
