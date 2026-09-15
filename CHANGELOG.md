# Release changes

## 0.1.0-rc.7 — 2026-09-15

- Finalize the guided Quiz Workshop with direct resumption of ready Rebind
  results and an explicit Quit action.
- Cancel on end-of-input instead of accepting default prompt confirmations.
- Allow blank descriptive review metadata by default in Quiz Workshop, while
  preserving explicit acceptance, validating supplied dates and providing a
  required policy. The generic producer retains its strict default.
- Verify workbook and Compose-artifact hashes before displaying readiness or
  building a package; require recomposition after changes.
- Retain local-only, readiness-gated package creation and the synthetic proof.

The full-screen real-export interface, Word intake, rich-math review adapters
and typed-equation conversion remain future work. Local package validation does
not establish a Brightspace import or round trip.

## 0.1.0-rc.6

First public runtime-minimized guided Unbind, Compose and Rebind workflow,
including full-library extraction and one/several/all quiz selection.
