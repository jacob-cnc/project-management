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
