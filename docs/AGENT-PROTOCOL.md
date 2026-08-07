# Agent Communication Protocol

Standard for how AI agents (Quick, Kiro, etc.) coordinate on shared projects.

## Roles

| Agent | Role | Strengths |
|-------|------|-----------|
| **Quick** | Project manager, researcher, planner | Architecture decisions, research, documentation, file organization, web search, task orchestration |
| **Kiro** | Implementation engineer | Code generation, refactoring, testing, debugging, IDE-integrated workflow |
| **Human (Jacob)** | Owner, decision-maker | Final approval, domain expertise, hardware testing, deployment |

## Handoff Protocol

### Quick → Kiro (Implementation Request)

When Quick needs code implemented, it creates/updates a **handoff file**:

`projects/<name>/handoffs/HANDOFF-<NNN>.md`

```markdown
# Handoff: [Brief Title]

**From:** Quick  
**To:** Kiro  
**Date:** YYYY-MM-DD  
**Priority:** High | Medium | Low  
**Status:** Pending | In Progress | Complete | Blocked

## Objective
What needs to be built/changed.

## Context
- Relevant decisions: ADR-XXX
- Requirements: REQ-XXX
- Current state: [what exists now]

## Specification
- Detailed description of what to implement
- Input/output expectations
- Constraints and edge cases

## Files to Create/Modify
- `path/to/file.py` — [what to do]

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## References
- [links to research, docs, examples]
```

### Kiro → Quick (Completion Report)

When Kiro completes work, it updates the handoff status and appends:

```markdown
## Completion Notes
**Completed:** YYYY-MM-DD

### What Was Done
- [summary of implementation]

### Deviations from Spec
- [anything that differed from the original request and why]

### Open Questions
- [anything that needs Quick/Human decision]

### Test Results
- [pass/fail summary]
```

### Either Agent → Human (Decision Needed)

When an agent hits a decision point beyond its authority:

`projects/<name>/handoffs/DECISION-NEEDED-<NNN>.md`

```markdown
# Decision Needed: [Title]

**From:** [Agent]  
**Date:** YYYY-MM-DD  
**Blocking:** [what's waiting on this]

## Question
[Clear question requiring human judgment]

## Options
1. [Option A] — [tradeoffs]
2. [Option B] — [tradeoffs]

## Recommendation
[Agent's recommendation if it has one]
```

## Status Signaling

Agents communicate current state via `STATUS.md` updates:

| Signal | Meaning |
|--------|---------|
| `🟢 Ready` | Available for new work |
| `🔵 In Progress` | Actively working on a task |
| `🟡 Blocked` | Waiting on decision/dependency |
| `🔴 Error` | Something failed, needs attention |

## Conflict Resolution

1. If agents disagree on approach → create an ADR with both positions, escalate to Human
2. If code changes conflict → the agent that committed first wins; second agent rebases
3. If scope creep is detected → flag in STATUS.md, do not proceed without approval

## Communication Log

All significant inter-agent communications are logged in `AGENT-LOG.md` with:
- Who initiated
- What was requested/delivered
- Outcome

## File Locking Convention

Before making major changes to a shared file, an agent notes it in STATUS.md:
```
## Active Locks
- `src/cam_engine.py` — Kiro (refactoring, ETA 30min)
```

Remove the lock entry when done.
