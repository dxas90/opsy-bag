---
name: agent-workspace
description: |
  Create and manage long-running agent task workspaces for cross-codebase work.

  Use when: (1) Starting a new long-running task that spans multiple repos (e.g., "document all services"),
  (2) Tracking progress on bulk operations (fix all Sentry issues, handle Renovate PRs),
  (3) Resuming work on a previously started cross-codebase task,
  (4) Organizing work that shouldn't pollute individual repo git histories.

  Location: ~/git/agent-workspace/<task-name>/
---

# Agent Workspace Management

Workspaces provide isolated tracking for long-running, cross-codebase agent tasks without polluting individual repository git histories.

## CRITICAL: Gather Requirements First

**NEVER start creating a workspace without user input.** If the user provides only a task name (e.g., "create acd-documentation"), you MUST ask them for the required information before proceeding.

### Required Information to Gather

Ask the user these questions before creating the workspace:

1. **Goal**: What does success look like for this task?
2. **Scope**: Which repos/orgs are included? What's excluded?
3. **Workflow**: What specific steps should be performed for each item?
4. **Validation**: How do we verify each item is complete? How do we verify the whole task is done?
5. **Output**: Where do results go? (PRs, docs, reports?)

### Example Conversation

```
User: create acd-documentation workspace

Agent: I'll help you set up the ACD documentation workspace. First, I need to understand the task:

1. **Goal**: What should these ACD documents contain? (e.g., architecture overview, data flows, dependencies)
2. **Scope**: Which organizations/repos? All of dxp? Specific services only?
3. **Output**: Where should the docs live? (e.g., `docs/ARCHITECTURE.md` in each repo, PRs)
4. **Validation**: What makes an ACD "complete"? (e.g., specific sections required, review needed)
```

**Only after receiving answers**, create the workspace with the README and tasklist.

## Workspace Structure

```
~/git/agent-workspace/
├── <task-name>/
│   ├── README.md              # Task definition (answers all required questions)
│   └── tasklist.md            # Progress tracking with validation status
└── ...
```

## Creating a New Workspace

### 1. Create Directory

```bash
mkdir -p ~/git/agent-workspace/<task-name>
```

### 2. Create README.md - Task Definition

The README **must answer all these questions** before work begins:

```markdown
# <Task Name>

## Goal

<One sentence describing what success looks like>

## Required Questions

### What repos need to be cloned?

- [ ] List all repositories required for this task
- [ ] Specify organization (dxp, hyperspace, platform-mesh, openmfp)
- [ ] Note any repos that must be at specific branches

| Repo | Org | Branch | Purpose |
|------|-----|--------|---------|
| repo-name | dxp | main | <why needed> |

### Where should the task be performed?

- **Working directory**: <where agent should cd to for each item>
- **Output location**: <where artifacts are created>
  - PRs to target repos
  - Docs to specific locations
  - Reports to workspace

### What needs to be performed?

For each item in the tasklist:

1. <Specific step 1>
2. <Specific step 2>
3. <Specific step 3>

### Which claude-knowledge agents/skills should be used?

Reference agents from `~/git/dxp/claude-knowledge/.claude/agents/`:

| Agent | When to Use |
|-------|-------------|
| repo-analyzer | First pass to understand each repo |
| repo-documenter | Generate documentation |
| developer | Make code changes |
| git | Commit and create PRs |

Reference skills from `~/git/dxp/claude-knowledge/.claude/skills/`:

| Skill | When to Use |
|-------|-------------|
| /commit | Creating commits |
| /pr-review | Reviewing changes |

### How is completion validated?

**Per-item validation** (checked after each item):

- [ ] <Validation criterion 1>
- [ ] <Validation criterion 2>
- [ ] <Validation criterion 3>

**Task-level validation** (checked at end):

- [ ] <Overall success criterion>

## Scope

- **Included**: <what qualifies>
- **Excluded**: <what is explicitly out of scope>

## Context

<Background information for resuming work>

## Notes

<Running notes added during execution>
```

### 3. Create tasklist.md - Progress Tracking

```markdown
# <Task Name> - Task List

## Status Legend

| Symbol | Meaning |
|--------|---------|
| `[ ]` | Not started |
| `[~]` | In progress |
| `[x]` | Complete - validated |
| `[?]` | Complete - needs validation |
| `[!]` | Blocked |
| `[-]` | Skipped (with reason) |

## Summary

- **Total**: X items
- **Complete**: Y items
- **Validated**: Z items
- **Remaining**: W items
- **Last updated**: YYYY-MM-DD

## Items

### <Category>

| Status | Item | Validation | Notes |
|--------|------|------------|-------|
| [ ] | item-name | | |
| [x] | done-item | PR #123 merged | Validated 2024-01-15 |
| [?] | needs-check | PR #456 open | Needs review |
| [!] | blocked-item | | Waiting on X |
```

## Validation Workflow

### When Completing an Item

1. Perform the task per README workflow
2. Run per-item validation checks
3. If all pass → mark `[x]` with validation evidence
4. If incomplete → mark `[?]` with what's pending

### When Resuming Work

1. Read README.md for full context
2. Check tasklist.md for `[?]` items needing validation first
3. Validate any `[?]` items before starting new work
4. Find next `[ ]` item to start

### When Validating Completion

1. Verify all items are `[x]` or `[-]`
2. Run task-level validation checks from README
3. Document final status in Notes section

## Populating a Tasklist

### For Repository-Based Tasks

```bash
# List all repos in an org
gh repo list dxp --limit 1000 --json name,isArchived \
  --jq '.[] | select(.isArchived == false) | .name' \
  --hostname github.tools.sap
```

### For Issue-Based Tasks (Sentry)

Use `mcp__sentry__list_issues` to get issues, add to tasklist with issue URLs.

### For PR-Based Tasks (Renovate)

```bash
gh search prs --owner dxp --author "renovate[bot]" --state open \
  --json repository,number,title --hostname github.tools.sap
```

## Example: ACD Documentation Task

### README.md

```markdown
# ACD Documentation for DXP Services

## Goal

Create Architecture Concept Documents for all DXP microservices.

## Required Questions

### What repos need to be cloned?

| Repo | Org | Branch | Purpose |
|------|-----|--------|---------|
| iam-service | dxp | main | Document this service |
| extension-manager-service | dxp | main | Document this service |
| claude-knowledge | dxp | main | Reference for agents/templates |

### Where should the task be performed?

- **Working directory**: Each target repo (`~/git/dxp/<repo>/`)
- **Output location**: PR to each repo adding `docs/ARCHITECTURE.md`

### What needs to be performed?

For each service:

1. Clone/update the repo
2. Run repo-analyzer agent to understand structure
3. Run repo-documenter agent with ACD template
4. Create PR with the documentation
5. Validate against checklist

### Which claude-knowledge agents/skills should be used?

| Agent | When to Use |
|-------|-------------|
| repo-analyzer | Understand repo structure and purpose |
| repo-documenter | Generate the ACD document |
| git | Create branch and PR |

### How is completion validated?

**Per-item validation:**

- [ ] `docs/ARCHITECTURE.md` exists in PR
- [ ] Document covers: Purpose, Components, Dependencies, Data Flow
- [ ] PR CI passes
- [ ] No placeholder text remaining

**Task-level validation:**

- [ ] All non-archived services have ACDs
- [ ] Consistent format across all documents
```

### tasklist.md

```markdown
# ACD Documentation - Task List

## Summary

- **Total**: 15 services
- **Complete**: 3
- **Validated**: 2
- **Remaining**: 12
- **Last updated**: 2024-01-15

## Items

### Core Services

| Status | Item | Validation | Notes |
|--------|------|------------|-------|
| [x] | iam-service | PR #45 merged | |
| [x] | extension-manager-service | PR #112 merged | |
| [?] | portal-service | PR #78 open | Awaiting review |
| [ ] | content-service | | |

### Operators

| Status | Item | Validation | Notes |
|--------|------|------------|-------|
| [ ] | flux-operator | | |
| [ ] | account-operator | | |
```

## Best Practices

- **Answer all questions first** - Don't start work until README is complete
- **Validate before marking complete** - Use `[?]` for unvalidated work
- **Link all artifacts** - PR numbers, commit SHAs, issue URLs
- **Check `[?]` items on resume** - Validate pending items before new work
- **Use claude-knowledge agents** - Leverage existing specialized agents

## Resource Limits

### ⚠️ CRITICAL: Maximum 5 Parallel Agents

**NEVER spawn more than 5 agents in parallel per main task.** Exceeding this limit will crash Claude.

```markdown
<!-- ✅ CORRECT: 5 or fewer parallel agents -->
Spawning agents for: repo-1, repo-2, repo-3, repo-4, repo-5
[Wait for completion]
Spawning agents for: repo-6, repo-7, repo-8

<!-- ❌ FORBIDDEN: More than 5 parallel agents -->
Spawning agents for: repo-1, repo-2, repo-3, repo-4, repo-5, repo-6, repo-7, repo-8
[CRASH]
```

### Batch Processing Pattern

When processing many items, batch them into groups of 5:

```markdown
## Batch Execution

### Batch 1 (items 1-5)
- [ ] item-1 → agent spawned
- [ ] item-2 → agent spawned
- [ ] item-3 → agent spawned
- [ ] item-4 → agent spawned
- [ ] item-5 → agent spawned
[Wait for all to complete before continuing]

### Batch 2 (items 6-10)
- [ ] item-6 → agent spawned
- [ ] item-7 → agent spawned
...
```

### Why This Limit Exists

- Claude has memory constraints for concurrent agent management
- Each spawned agent consumes significant context
- Exceeding 5 parallel agents causes context overflow and crashes
- Sequential batching ensures stable execution
