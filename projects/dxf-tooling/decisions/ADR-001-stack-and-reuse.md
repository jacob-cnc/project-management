# ADR-001: Tkinter GUI and reuse the proven split logic verbatim

**Date:** 2026-10-19  
**Status:** Accepted  
**Deciders:** jacob-cnc (approved), default/Kiro agent (proposed)

## Context

The DXF cleanup logic already exists as a proof-of-concept
(`split_lines_at_intersections.py`) that has been validated on the physical
ProtoTRAK DPM RMX machine. The task is to productize it as a lightweight,
local-only Windows desktop app with a before/after preview and a report. Two
decisions were needed: the GUI framework, and how to treat the proven logic.

## Options Considered

### Option A: Tkinter GUI
- Pros: Ships with Python, zero extra dependencies, bundles cleanly with PyInstaller, keeps the package small; the handoff explicitly says "Tkinter is fine."
- Cons: Plainer-looking preview than a Qt canvas.

### Option B: PySide6 / PyQt GUI
- Pros: Nicer preview and widgets.
- Cons: Heavier dependency, larger PyInstaller output, more packaging complexity — unneeded for a single-purpose shop tool.

### Reuse approach
- Reuse the proven `split_lines_at_intersections.py` algorithm **verbatim** vs. rewriting it "cleaner."

## Decision

- **GUI: Tkinter.** Dependency-free and sufficient for a pick-in / clean-out tool with a matplotlib preview embedded.
- **Reuse the proven split algorithm exactly.** Port it into a `dxfclean` package without changing its results. New behavior (SPLINE conversion, reporting) wraps around it; it is never "improved" without a regression test proving the delrin-peg acceptance case still passes.

## Consequences

- The repo stays small and the exe stays double-clickable.
- The machine-validated behavior is protected by an automated acceptance test (11 line segments + 2 arcs + 1 circle, zero zero-length, no interior crossings) that must pass before any release.
- Follow-up work: SPLINE → arc/line conversion with flagging, and PyInstaller packaging (requires installing PyInstaller into the real Python 3.12 interpreter).
