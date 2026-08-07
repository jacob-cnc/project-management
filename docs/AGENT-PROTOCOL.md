# Agent Communication Protocol

Operational reference for how Quick, Kiro, and Jacob coordinate on engineering/manufacturing software projects.

## Core Principle

> **Quick reduces ambiguity. Kiro reduces effort. Jacob validates reality.**
> The interface between agents is lightweight, versioned, and easy to update.
> Specs and steering files are that interface — invest in making them clear and concise.

---

## Roles

| Agent | Role | Strengths | Authority |
|-------|------|-----------|-----------|
| **Quick** | Project manager, researcher, planner | Architecture decisions, research, documentation, file organization, web search, task orchestration | Owns scope, priority, spec accuracy |
| **Kiro** | Implementation engineer | Code generation, refactoring, testing, debugging, IDE-integrated workflow | Owns implementation approach, code structure |
| **Jacob** | Owner, domain expert, decision-maker | Final approval, domain expertise (machining, manufacturing, quality systems), hardware testing, deployment | Final authority on everything. Neither agent ships domain-critical logic without human sign-off |

---

## Communication Patterns

### Quick → Kiro (Downstream)

- Requirements docs with acceptance criteria
- Prioritized backlogs and task breakdowns
- Decision records ("we chose X because Y")
- Constraints and non-functional requirements ("must run offline", "no external deps")
- Scope boundaries ("this is in, this is out")
- Research findings ("here's what I investigated and ruled out — don't re-explore these dead ends")

### Kiro → Quick (Upstream)

- Technical feasibility feedback ("this requirement conflicts with that one")
- Implementation notes ("I made this assumption — verify it's correct")
- Discovered complexity ("this is actually 3 tasks, not 1")
- Status updates embedded in task/spec files
- Completion reports on handoffs

### Either Agent → Jacob (Escalation)

- Domain questions ("is this tolerance stackup logic correct?")
- Scope decisions ("should we add this feature?")
- Trade-off calls that affect UX or shop-floor usability
- Anything touching hardware behavior, safety, or compliance

---

## Workflow Models

### 1. Spec-First (features with clear scope)

```
Quick writes spec → Kiro implements → Quick reviews → Jacob validates
```

**Use when:** You know what you want before building starts.
**Example:** Adding a new G-code post-processor with known requirements.

### 2. Spike-Then-Spec (exploratory work)

```
Kiro does technical spike → Quick writes realistic spec from findings → Kiro implements
```

**Use when:** You're not sure what's possible yet.
**Example:** Investigating LinuxCNC HAL integration options, testing a geometry library.

### 3. Iterative Drafting (documents, configs, reports)

```
Quick drafts v1 → Kiro refines/automates → Quick reviews → iterate
```

**Use when:** The output is a document or artifact, not pure code.
**Example:** QMS procedures, configuration templates, API docs.

### 4. Standards-Driven (ongoing maintenance)

```
Quick defines standards in steering files → Kiro follows automatically → Quick audits periodically
```

**Use when:** Consistency matters more than speed.
**Example:** Code style, commit conventions, document formatting across the QMS.

### 5. Standards-Evolving (new domain)

```
Start building → extract patterns from what works → codify into steering files
```

**Use when:** You're building in a new domain and conventions haven't emerged yet.
**Example:** First QMS build — document structure evolved as AS9100 requirements became clearer.

---

## Handoff Protocol

### Quick → Kiro (Implementation Request)

Create: `projects/<name>/handoffs/HANDOFF-<NNN>.md`

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
- Research: [link to knowledge/research/ findings — what was already explored and ruled out]

## Specification
- Inputs / outputs / behavior
- Constraints and edge cases
- What NOT to build (scope boundary)

## Acceptance Criteria
- [ ] Criterion 1 — unambiguous, testable
- [ ] Criterion 2

## Files to Create/Modify
- `path/to/file.py` — [what to do]

## References
- [links to research, docs, examples]
```

**Rules for Quick when writing handoffs:**
- Be explicit about acceptance criteria — "done" must be unambiguous
- Separate "must have" from "nice to have" clearly
- Write constraints, not solutions — tell Kiro *what*, let Kiro figure out *how*
- Include "what I already ruled out" from research so Kiro doesn't re-explore dead ends
- Don't over-specify implementation — no class hierarchies, no function signatures (unless there's an interface contract)

### Kiro → Quick (Completion Report)

Append to the handoff file (do not edit history):

```markdown
## Completion Notes
**Completed:** YYYY-MM-DD

### What Was Done
- [summary of implementation]

### Deviations from Spec
- [anything that differed from the original request and why]

### Assumptions Made
- [any assumptions that need verification]

### Open Questions
- [anything that needs Quick/Human decision]

### Test Results
- [pass/fail summary]
```

**Rules for Kiro when completing handoffs:**
- Flag ambiguity early rather than guessing
- Summarize decisions made during implementation so Quick can review
- Keep changes atomic — one logical change per commit/task
- Don't gold-plate — implement what's specified, no more

### Either Agent → Human (Decision Needed)

Create: `projects/<name>/handoffs/DECISION-NEEDED-<NNN>.md`

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

## Agent's Recommendation
[If the agent has one, with reasoning]
```

---

## Spec Granularity Guidelines

Not all specs should be the same size:

| Project Type | Spec Approach | Rationale |
|---|---|---|
| UI/UX features | Short specs, fast iteration | Easier to try than to specify perfectly |
| Workflow/process | Medium specs with clear states | Need to define the happy path + edge cases |
| Mathematical/algorithmic | Thorough specs upfront | Correctness constraints — can't iterate to correct stackup logic |
| Compliance/QMS | Thorough specs with traceability | Changing one procedure cascades to forms and work instructions |
| Exploratory/R&D | Spike first, spec after | Don't know enough to spec yet |

**General rule:** A 2-task spec reviewed quickly beats a 20-task spec that drifts — *unless* the domain has correctness or compliance constraints that require upfront completeness.

---

## Status Signaling

Agents communicate current state via `STATUS.md`:

| Signal | Meaning |
|--------|---------|
| 🟢 Ready | Available for new work |
| 🔵 In Progress | Actively working on a task |
| 🟡 Blocked | Waiting on decision/dependency |
| 🔴 Error | Something failed, needs attention |

---

## File Locking Convention

Before making major changes to a shared file, note it in STATUS.md:
```
## Active Locks
- `src/cam_engine.py` — Kiro (refactoring, ETA 30min)
```
Remove the lock entry when done.

---

## Conflict Resolution

1. If agents disagree on approach → create an ADR with both positions, escalate to Human
2. If code changes conflict → the agent that committed first wins; second agent rebases
3. If scope creep is detected → flag in STATUS.md, do not proceed without approval
4. If reality diverges from spec → update the spec (specs are living documents, not contracts)

---

## Anti-Patterns to Avoid

| Anti-Pattern | Why It's Bad | Fix |
|---|---|---|
| Quick over-specifying implementation | Constrains Kiro unnecessarily, often leads to worse solutions | Write constraints, not solutions |
| Kiro making product decisions silently | Quick owns scope and priority | Flag and escalate |
| Specs that never get updated | Become misleading — worse than no spec | Update spec when reality diverges |
| Too many abstraction layers | Handoff doc takes longer than the code | Rethink granularity |
| Skipping review | Misunderstandings compound | Quick reviews Kiro's output — this is where errors surface cheaply |
| Agents shipping domain-critical logic without Jacob | Only Jacob knows if the machining logic is actually correct | Always get human sign-off on domain logic |
| Re-researching settled questions | Wastes time | Check `decisions/` and `knowledge/` before exploring |

---

## Document Versioning Rules

- Handoff files are **append-only** during active work — don't edit history, add completion notes
- Decision records (ADRs) are **immutable** once accepted — supersede with a new ADR
- Specs and STATUS.md are **living documents** — update them when reality changes
- All documents version alongside code — same repo, same git history
- This supports audit trail requirements for AS9100-adjacent work

---

## Quick's Research Role

Quick's value extends beyond spec-writing. Before specs exist, Quick:

1. **Researches** libraries, approaches, standards, prior art
2. **Compares** options with tradeoffs documented in `knowledge/research/`
3. **Rules out** dead ends so Kiro doesn't re-discover them
4. **Summarizes** findings into actionable specs

When Quick hands off a spec, it should include "here's what I already investigated and ruled out" — this reduces Kiro's exploration time significantly.

---

## Communication Log

All significant inter-agent communications are logged in `AGENT-LOG.md` with:
- Who initiated
- What was requested/delivered
- Outcome
- Any assumptions that need verification
