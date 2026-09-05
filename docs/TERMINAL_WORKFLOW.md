# Quiz terminal workflow

`scripts/quiz_terminal.py` is the ordinary local interface for real
Brightspace exports. It follows one staged workflow:

1. **Unbind** extracts a ZIP or unpacked export into verified evidence, a
   Reading Room, and a reviewer workbook.
2. **Review** happens in `compose/reviewer_working.xlsx`. The sibling
   `reviewer_baseline_DO_NOT_EDIT.xlsx` preserves the extracted source state.
3. **Compose** reads only explicit reviewer columns, materializes the decision
   overlay, promotes accepted changes into a new model copy, selects one,
   several, or all quizzes, and runs a separate strict authoring-readiness
   check for each.
4. **Rebind** is available one quiz/package at a time only if that exact
   Compose result is ready. It builds a local package and ZIP, then validates
   the package closure, model projection, receipts, and folder/ZIP equality.

The terminal does not sign in to Brightspace, import a package, or claim an
LMS round trip. A successful Rebind earns only `validated locally`.

## Guided use

```bash
.venv/bin/python scripts/quiz_terminal.py
```

The wizard asks what to do, shows the exact read/write boundary, offers one,
several, or all quizzes for Compose, and pauses before both Compose and
Rebind. Return accepts a displayed default. A dragged terminal path may be
quoted or contain escaped spaces.

Unbind deliberately extracts all quizzes and parses the complete available
Question Library. In D2L exports, quiz XML can hold placements or references
while reusable question bodies live in `questiondb.xml`; narrowing extraction
too early could hide a quiz-to-library relationship or a library-only record.
The optional Unbind `--quiz-entity-key` narrows only the initial readiness
report. It does not filter the extracted evidence.

## Scripted use

```bash
.venv/bin/python scripts/quiz_terminal.py unbind course-export.zip \
  --workspace output/my-course__quiz_workflow

.venv/bin/python scripts/quiz_terminal.py status \
  output/my-course__quiz_workflow

.venv/bin/python scripts/quiz_terminal.py compose \
  output/my-course__quiz_workflow \
  --quiz-entity-key 'cc:quiz:...'

.venv/bin/python scripts/quiz_terminal.py compose \
  output/my-course__quiz_workflow \
  --quiz-entity-key 'cc:quiz:first' \
  --quiz-entity-key 'cc:quiz:second'

.venv/bin/python scripts/quiz_terminal.py compose \
  output/my-course__quiz_workflow \
  --all-quizzes

.venv/bin/python scripts/quiz_terminal.py rebind \
  output/my-course__quiz_workflow \
  --quiz-entity-key 'cc:quiz:...'
```

For a multi-quiz export, use the guided wizard to choose from titles and entity
keys, or take keys from the model/Reading Room for scripted use. Rebind may
omit `--quiz-entity-key` only when exactly one selected quiz is ready.

## What Compose does and does not approve

Extracted questions remain evidence until their instance-level build support
passes the pinned capability registry. Compose never upgrades that evidence by
preference. A `NOT READY` result is a completed and useful stop: its report
lists exact blocker codes and remediation, and no package is built.

Accepted settings decisions are written to a settings receipt. Any supported
setting without an accepted reviewer decision uses the builder's recorded safe
default; inspect that receipt before Rebind. Unsupported settings remain a
readiness/build refusal.

The optional `--phase5-candidate-authorization` is an advanced, exact-bound,
time-limited local trial route. It is not import or round-trip evidence and is
never created automatically by the terminal.

## Sharing boundary

An Unbind workspace contains question and answer content. Share the reviewer
workbook or its Reading Room export intentionally; do not assume the entire
workspace is safe for a public repository. If the source export contained
images/files, the default self-contained mode copies resolved assets and keeps
their workbook/Reading Room links. Reference mode is smaller but may leave
links dependent on the original source location.

The advanced full-screen synthetic proof remains available from the wizard or
through `scripts/quiz_binder_tui.py`. It demonstrates the bounded build route;
it is not a substitute for readiness on a real extracted quiz.
