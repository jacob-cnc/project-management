# Project Conventions

## File & Folder Naming
- All lowercase, kebab-case: `tolerance-analysis`, `cam-engine`
- No spaces, no special characters except hyphens

## Status Tracking
Each project maintains a `STATUS.md` with:
- Current phase (planning / active-dev / testing / maintenance / archived)
- Active sprint/milestone goals
- Blockers and dependencies
- Next actions

## Decision Records (ADR)
Format: `decisions/NNN-title.md`
- Number sequentially (001, 002, ...)
- Once accepted, decisions are immutable (supersede with a new ADR)
- Template in `templates/decision-record.md`

## Requirements
- One markdown file per functional area
- Use MoSCoW priority (Must/Should/Could/Won't)
- Tag requirements with IDs: `[REQ-001]`, `[REQ-002]`

## Git Workflow
- `main` — stable, tested
- `dev` — integration branch
- Feature branches: `feature/<short-description>`
- Prefix commits: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`

## Agent Collaboration
- Agents MUST read PROJECT.md before starting work
- Agents MUST log actions in AGENT-LOG.md
- Agents SHOULD NOT create new top-level directories without approval
- Agents CAN create files within existing project structure

## Agent Protocol Clarifications (see ADR-001)

These resolve points left underspecified in the original protocol:

### Branch naming

Feature branches are `feature/<short-description>` (slash form). This is the

authoritative form — ignore any `feature-*` shorthand seen elsewhere.

### Handoff vs. decision numbering

`HANDOFF-NNN` and `DECISION-NEEDED-NNN` each use their own independent counter

within `projects/<name>/handoffs/`. They are separate document types:

- `HANDOFF-NNN.md` — work order (Quick → Kiro), append-only

- `DECISION-NEEDED-NNN.md` — escalation to Jacob

### Agent logging

Log significant autonomous actions to the relevant project's

`projects/<name>/AGENT-LOG.md`, most recent at top. No cross-project log exists

by design (per-project audit trail).

### Cross-project patterns

When a pattern proves reusable across projects, promote it into steering rather

than leaving it in a single project's `knowledge/`:

- Rules/conventions → this file `CONVENTIONS.md`)

- Reusable technical patterns → `docs/PATTERNS.md`

Per-project `knowledge/` stays for project-specific findings.
