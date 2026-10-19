# Status — DXF Tooling

**Phase:** Active Development  
**Last Updated:** 2026-10-19

## Current Sprint / Milestone

**Goal:** Stand up the DXF-Tooling repo, port the proven cleanup/render scripts, and get the delrin-peg acceptance test passing.

### Completed
- [x] ~~Review GitHub, project-management template, and handoff note~~ ✅
- [x] ~~Confirm dependencies (Python 3.12.10, ezdxf 1.4.5, matplotlib 3.11.2, gh auth, git)~~ ✅
- [x] ~~Create repo https://github.com/jacob-cnc/DXF-Tooling~~ ✅
- [x] ~~Scaffold this tracking folder from the project-management template~~ ✅

### In Progress
- [ ] Port proven scripts + build Tkinter app + acceptance test — kirocrew-worker (work session)

### Up Next
- [ ] SPLINE → arc/line conversion with flagging
- [ ] PyInstaller single-exe package (requires `pip install pyinstaller`)

## Blockers

| Blocker | Impact | Owner | Since |
|---------|--------|-------|-------|
| _(none)_ | | | |

## Recent Decisions

- ADR-001: Tkinter GUI + reuse the proven split logic verbatim → `decisions/ADR-001-stack-and-reuse.md`

## Metrics (if applicable)

- Acceptance test: not yet run (target: 11 line segments + 2 arcs + 1 circle, 0 zero-length)
- Build status: initial population in progress
