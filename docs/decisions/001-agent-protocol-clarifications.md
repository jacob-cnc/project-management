# ADR-001: Agent Protocol Clarifications

**Date:** 2026-09-24

**Status:** Accepted

**Deciders:** Jacob (owner), Quick (proposed)

## Context

During Kiro's onboarding to the project-management framework, four points were

identified as underspecified in the existing docs. Left unresolved, each would

force agents to guess or re-ask on every project. This ADR settles all four as

authoritative conventions.

## Options Considered

For the cross-project research question (#4), two structural options were weighed:

### Option A: Add a shared `docs/knowledge/` research tree

- Pros: dedicated home for cross-project findings

- Cons: parallel tree that can drift out of sync with actual conventions; duplicates per-project `knowledge/`

### Option B: Promote reusable patterns into `docs/` steering files

- Pros: keeps the framework lean; matches the protocol's own "Standards-Evolving" model; single source of truth for how-we-do-things

- Cons: requires the discipline to distill rather than dump

Option B was chosen.

## Decision

1. **Git branch naming.** `CONVENTIONS.md` is authoritative: feature branches are

   `feature/<short-description>` (slash form). Any shorthand like `feature-*`

   elsewhere does not override the repo.

2. *`DECISION-NEEDED` numbering.** `DECISION-NEEDED-NNN` files use their own

   sequence, independent of `HANDOFF-NNN`. Both live in

   `projects/<name>/handoffs/` but count separately (they are different document

   types with different lifecycles).

3. **Agent logging.** Significant autonomous actions are logged in the relevant

   project's `projects/<name>/AGENT-LOG.md` (most recent at top). There is

   intentionally no cross-project log — activity is scoped per project to support

   the AS9100-adjacent audit trail.

4. **Cross-project research/patterns.** Reusable patterns that span projects are

   promoted into `docs/` steering files rather than a separate research tree:

   - Conventions/rules → `docs/CONVENTIONS.md`

   - Reusable technical patterns → `docs/PATTERNS.md` (new steering file)

   - Per-project `knowledge/` remains for project-specific findings; only

     patterns that prove reusable get promoted up.

## Consequences

- Agents have definitive answers to all four points — no re-asking per project.

- A new steering file, `docs/PATTERNS.md`, is introduced (see CONVENTIONS addendum).

- `CONVENTIONS.md` gains a short addendum cross-referencing this ADR.

- Being cross-project conventions, this ADR is treated as immutable once accepted;

  supersede with a new ADR if any point changes.
