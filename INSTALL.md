# Installation

Quiz Bundle `0.1.0-rc.6` uses a local Python virtual environment and direct,
inspectable script entry points. It is not a background service and does not
contact Brightspace during installation or use.

## Python

- Supported: Python 3.11, 3.12, or 3.13.
- Verified for this release candidate: Python 3.13.
- Python 3.14 is not yet admitted.

## Create the environment

From the extracted release folder:

```bash
python3.13 scripts/bootstrap_env.py --locked
```

For source-repository development and tests:

```bash
python3.13 scripts/bootstrap_env.py --locked --dev
```

The default environment is `.venv/`. The bootstrap script does not delete an
existing environment or write outside the selected path. Replace `python3.13`
with the local name of a supported interpreter if needed.

## Start the guided workflow

```bash
.venv/bin/python scripts/quiz_terminal.py
```

Choose **Unbind a Brightspace export for review**. The wizard asks for the ZIP
or unpacked export and a new output folder, then shows the exact reads, writes,
asset behavior, and no-network boundary before it runs.

The new workflow contains:

- a verified Unbind receipt and content-minimized status report;
- a local authored-content Reading Room;
- a detailed workbook and a reviewer-facing workbook;
- `compose/reviewer_working.xlsx`, which is the review copy to edit;
- `compose/reviewer_baseline_DO_NOT_EDIT.xlsx`, which preserves source state;
- a workflow record used to re-verify evidence before later steps.

Run the wizard again and choose **Continue a reviewed workflow with Compose**
after proposals and approvals are saved. Compose can check one, several, or all
quizzes and produces a separate readiness report for each. Rebind is offered
one quiz/package at a time only when that exact report is ready.

Unbind itself defaults to the complete export: all quizzes and the available
Question Library are parsed together. This is necessary because quiz XML often
contains placements or references while reusable question bodies live in
`questiondb.xml`. Quiz selection controls later readiness work; it does not
discard library evidence.

Detailed commands and boundaries are in
[docs/TERMINAL_WORKFLOW.md](docs/TERMINAL_WORKFLOW.md).

## Scripted workflow

```bash
.venv/bin/python scripts/quiz_terminal.py unbind /path/to/export.zip \
  --workspace output/course__quiz_workflow
.venv/bin/python scripts/quiz_terminal.py status output/course__quiz_workflow
.venv/bin/python scripts/quiz_terminal.py compose output/course__quiz_workflow \
  --all-quizzes
.venv/bin/python scripts/quiz_terminal.py rebind output/course__quiz_workflow \
  --quiz-entity-key 'cc:quiz:...'
```

By default, available source images/files are copied into the local evidence
run so workbook and Reading Room links remain useful. `--asset-mode reference`
is the explicit smaller alternative.

An Unbind workspace contains authored questions and answers. Share the reviewer
workbook or its Reading Room export intentionally; do not publish the entire
workspace without reviewing its contents.

## Optional source-tree verification

These commands apply to a development checkout. The minimized public release
does not ship the full test suite.

```bash
.venv/bin/python scripts/vendor_from_workbench.py --check
.venv/bin/python -m pytest
.venv/bin/python scripts/make_release_asset.py --check-only
```

## Advanced synthetic proof

```bash
.venv/bin/python scripts/quiz_binder_tui.py \
  --output-dir output/synthetic-journey
```

The full-screen proof requires a POSIX TTY with at least 80×12 cells. `--plain`
uses its linear screen-reader/automation surface, `--ascii` substitutes ASCII
chrome, and `--mono` removes color.

## Dependency and license policy

- `requirements.txt` gives compatible runtime ranges.
- `requirements-lock.txt` records the exact runtime resolution.
- `requirements-dev.txt` adds the source-tree test runner.
- `sbom/runtime.cdx.json` is the CycloneDX runtime inventory.

The public distribution uses AGPL-3.0-or-later with commercial terms available
by agreement. See `LICENSE`, `LICENSE_POSTURE.md`, and `COMMERCIAL.md`.
