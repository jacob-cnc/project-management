# DXF Tooling — DXF Cleanup App for ProtoTRAK DPM RMX

**Started:** 2026-10-19  
**Owner:** jacob-cnc  
**Repo:** https://github.com/jacob-cnc/DXF-Tooling  
**Status:** Active Development

## Purpose

A lightweight, local-only Windows desktop app that cleans messy / CAD-exported
DXF files into geometry that chains cleanly on a **ProtoTRAK DPM RMX** mill
control. It takes a DXF in and produces a "machinable" DXF out where curves are
real arcs/circles (not splines) and every line is broken at each point another
entity touches it, so no entity runs through another's intersection or overlaps
a colinear line. This productizes a working, machine-validated proof-of-concept.

## Scope

### In Scope
- Convert SPLINE entities to arc/line approximations; flag anything that can't convert cleanly.
- Break every LINE at each intersection/touch point into discrete segments.
- Drop zero-length / degenerate segments.
- Preserve geometry exactly (subdivide only — never move, trim, or merge), plus layers and units.
- Simple Tkinter GUI: DXF in → cleaned DXF out (`<name>_clean.dxf`), before/after preview, short report.
- Package as a double-clickable Windows executable (PyInstaller), accounting for the MS Store python-stub trap.

### Out of Scope
- No toolpath / G-code generation (clean chainable geometry only).
- No cloud, accounts, or network calls — local only.
- No "improving" the proven split logic's results without a regression test proving the acceptance case still passes.

## Architecture Overview

```
[messy DXF] → [dxfclean: spline-convert → split lines at intersections → drop degenerate] → [clean DXF]
                                      ↓
                          [render preview + report]  →  [Tkinter GUI]
```

## Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| Language | Python 3.12 | Real interpreter, NOT the MS Store PATH stub |
| Geometry | ezdxf 1.4.5 | DXF read/write |
| Preview | matplotlib 3.11.2 | Render LINE/ARC/CIRCLE/polyline to PNG |
| GUI | Tkinter | Dependency-free, bundles cleanly with PyInstaller |
| Packaging | PyInstaller | Single double-clickable .exe (later milestone) |
| Platform | Windows | |

## Key Constraints

- Output must contain **no splines** and no line passing through another entity's interior.
- Geometry, layers, and units must be preserved exactly.
- Must pass the delrin-peg regression (see Milestone 2) — proven correct on the physical machine.
- MS Store `python` stub does not work; always use the full interpreter path.

## Milestones

| # | Milestone | Target Date | Status |
|---|-----------|-------------|--------|
| 1 | Repo + scaffold + proven scripts ported | 2026-10-19 | 🟡 In progress |
| 2 | Acceptance test passing (11 segments, 0 zero-length, no interior crossings) | 2026-10-19 | ⬜ Not started |
| 3 | Tkinter app: pick DXF, preview, report, write clean DXF | 2026-10-20 | ⬜ Not started |
| 4 | SPLINE → arc/line conversion with flagging | — | ⬜ Not started |
| 5 | PyInstaller single-exe package | — | ⬜ Not started |

## References

- Handoff note: `C:\Users\jhonick\Desktop\kiro tools\HANDOFF.md`
- Proven PoC scripts: `C:\Users\jhonick\Desktop\kiro tools\` (`split_lines_at_intersections.py`, `render_dxf.py`, `build_delrin_peg_arcs.py`)
- Regression fixture: `fixtures/delrin_peg_ARCS_unsplit.dxf`
- Target control: ProtoTRAK DPM RMX
