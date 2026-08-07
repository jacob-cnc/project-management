# Project Management

Central hub for planning, tracking, and organizing engineering/manufacturing software projects.

## Repository Structure

```
project-management/
├── projects/           # Active project tracking (one folder per project)
│   └── _template/      # Copy this to start a new project
├── templates/          # Reusable templates for docs, specs, decisions
├── docs/               # Cross-project standards and conventions
└── .github/            # Issue templates for GitHub workflow
```

## Quick Start — New Project

1. Copy `projects/_template/` → `projects/<project-name>/`
2. Fill in `PROJECT.md` (the project charter)
3. Populate `requirements/` as you define scope
4. Track decisions in `decisions/` using the ADR format
5. Update `STATUS.md` as work progresses

## Project Types This Supports

- **CNC/Machine Control** — controller firmware, motion planning, HAL configs
- **CAM/Toolpath Generation** — geometry engines, post-processors, tool libraries
- **Manufacturing Quality** — QMS systems, inspection tools, compliance docs
- **Shop/Inventory Tools** — storage organizers, tool trackers, asset management
- **Analysis & Simulation** — tolerance stackups, FEA integration, optimization

## Conventions

| Convention | Rule |
|---|---|
| Naming | `kebab-case` for folders/files |
| Status | `STATUS.md` in each project root — single source of truth |
| Decisions | ADR format in `decisions/` — numbered sequentially |
| Requirements | One `.md` per functional area in `requirements/` |
| Agent Notes | Agents append to `AGENT-LOG.md` when making autonomous changes |

## For AI Agents

If you are an AI agent working on a project:
1. Read `projects/<name>/PROJECT.md` first for context
2. Check `STATUS.md` for current state and blockers
3. Log significant actions in `AGENT-LOG.md`
4. Reference decisions in `decisions/` before making architectural choices
5. Do not modify `requirements/` without explicit user approval
