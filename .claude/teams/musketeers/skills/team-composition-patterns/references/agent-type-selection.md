# Agent Type Selection Guide

Decision matrix for choosing the right `subagent_type` when spawning teammates.

## Decision Matrix

```
Does the teammate need to modify files?
├── YES → Does it need a specialized role?
│         ├── YES → Which role?
│         │         ├── Code review → musketeers:Athos
│         │         ├── Bug investigation → musketeers:Porthos
│         │         ├── Feature building → musketeers:Aramis
│         │         └── Team coordination → musketeers:DArtagnan
│         └── NO → general-purpose
└── NO → Does it need deep codebase exploration?
          ├── YES → Explore
          └── NO → Plan (for architecture/design tasks)
```

## Agent Type Comparison

| Agent Type                   | Can Read | Can Write | Can Edit | Can Bash | Specialized        |
| ---------------------------- | -------- | --------- | -------- | -------- | ------------------ |
| general-purpose              | Yes      | Yes       | Yes      | Yes      | No                 |
| Explore                      | Yes      | No        | No       | No       | Search/explore     |
| Plan                         | Yes      | No        | No       | No       | Architecture       |
| musketeers:DArtagnan        | Yes      | Yes       | Yes      | Yes      | Team orchestration |
| musketeers:Athos    | Yes      | Yes       | Yes      | Yes      | Code review        |
| musketeers:Porthos    | Yes      | Yes       | Yes      | Yes      | Bug investigation  |
| musketeers:Aramis | Yes      | Yes       | Yes      | Yes      | Feature building   |

## Common Mistakes

| Mistake                               | Why It Fails                   | Correct Choice                          |
| ------------------------------------- | ------------------------------ | --------------------------------------- |
| Using `Explore` for implementation    | Cannot write/edit files        | `general-purpose` or `Aramis` |
| Using `Plan` for coding tasks         | Cannot write/edit files        | `general-purpose` or `Aramis` |
| Using `general-purpose` for reviews   | No review structure/checklists | `Athos`                         |
| Using `Aramis` for research | Has tools but wrong focus      | `Explore` or `Plan`                     |

## When to Use Each

### general-purpose

- One-off tasks that don't fit specialized roles
- Tasks requiring unique tool combinations
- Ad-hoc scripting or automation

### Explore

- Codebase research and analysis
- Finding files, patterns, or dependencies
- Understanding architecture before planning

### Plan

- Designing implementation approaches
- Creating task decompositions
- Architecture review (read-only)

### DArtagnan

- Coordinating multiple teammates
- Decomposing work and managing tasks
- Synthesizing results from parallel work

### Athos

- Focused code review on a specific dimension
- Producing structured findings with severity ratings
- Following dimension-specific checklists

### Porthos

- Investigating a specific hypothesis about a bug
- Gathering evidence with file:line citations
- Reporting confidence levels and causal chains

### Aramis

- Building code within file ownership boundaries
- Following interface contracts
- Coordinating at integration points
