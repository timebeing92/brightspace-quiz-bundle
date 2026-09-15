# Brightspace Quiz Bundle

This public repository is the sanitized runtime-distribution surface. The
broader development history and evidence corpus remain private; neither is
required to install or run this release.

Brightspace Quiz Bundle is the portable producer for **Quiz Binder**, presented
in the terminal as **Quiz Workshop**. Its ordinary workflow is:

- **Unbind** a Brightspace export into verified local evidence, a reviewer
  workbook, and a Reading Room.
- **Compose** one, several, or all selected quizzes from explicit reviewer
  proposals and approvals, with a separate strict readiness result for each.
- **Rebind** one authoring-ready quiz at a time into a locally validated
  Brightspace package and ZIP.

The tool never signs in to Brightspace or imports a package. Local validation
is not tenant import or round-trip evidence.

## Guided terminal

Create the environment, then launch the wizard:

```bash
python3.13 scripts/bootstrap_env.py --locked
.venv/bin/python scripts/quiz_terminal.py
```

The wizard is the recommended interface. It shows the exact source and output
paths, explains each stop, lets a reviewer choose one, several, or all quizzes
by title, and asks for confirmation before Unbind, Compose, or Rebind. You can
resume a ready Compose result directly with **Rebind**, or choose **Quit**.
Closing terminal input cancels the prompt without accepting its default.

The same workflow is scriptable:

```bash
.venv/bin/python scripts/quiz_terminal.py unbind course-export.zip \
  --workspace output/my-course__quiz_workflow

.venv/bin/python scripts/quiz_terminal.py status \
  output/my-course__quiz_workflow

.venv/bin/python scripts/quiz_terminal.py compose \
  output/my-course__quiz_workflow \
  --all-quizzes

.venv/bin/python scripts/quiz_terminal.py rebind \
  output/my-course__quiz_workflow \
  --quiz-entity-key 'cc:quiz:...'
```

Unbind preserves two workbook copies: edit
`compose/reviewer_working.xlsx` and leave
`compose/reviewer_baseline_DO_NOT_EDIT.xlsx` unchanged. Compose compares them
so extracted source evidence cannot be silently replaced by reviewer text.
Names, reasons and dates are optional in Quiz Workshop. Supplied metadata is
retained; only explicit accepted decisions are applied. Choose the required
policy in the wizard or pass `compose --metadata-policy required` when complete
attribution is needed. Rebind verifies the recorded workbook and Compose file
hashes before building; changed inputs require a new Compose run.

The workbook's primary surfaces are `All Questions` and `Quiz Occurrences`.
They show every library question, quiz use, direct or inferred library
relationships, quiz-native questions, image references, and available local
assets. `Question Entities` is a hidden technical appendix. Unbind always keeps
the complete quiz set and parses `questiondb.xml` when present because question
bodies may live in the library while quiz XML holds their placements or
references. Selecting one, several, or all quizzes during Compose controls
readiness analysis without discarding that shared evidence.

An honest `NOT READY` Compose result is completed work. The report identifies
the exact unresolved relationships, unsupported question kinds, missing
instance evidence, assets, settings, or identity decisions. No package is built
until the exact result passes the pinned registry.

See [the terminal workflow](docs/TERMINAL_WORKFLOW.md) and
[installation guide](INSTALL.md).

## Advanced synthetic proof

The earlier full-screen Bindery Ledger remains as an advanced, synthetic
product proof:

```bash
.venv/bin/python scripts/quiz_binder_tui.py \
  --output-dir output/synthetic-journey
```

It exercises a fixed build-approved specimen through Compose, Rebind, strict
validation, and folder/ZIP equality. It does not make an extracted real-course
quiz build-approved. See [the synthetic journey](docs/SYNTHETIC_JOURNEY.md).

## Ownership and portability

`coursecraft_workbench` owns quiz contracts, extraction/normalization meaning,
the capability registry, and live-verification evidence. Quiz Bundle owns the
terminal interaction, installation, and distribution layer. The release runs
its byte-pinned producer files locally; it does not call or require access to
the private Workbench repository at runtime. The exact upstream commit and
carried file digests are recorded in `upstream/workbench_pin.json`.

The public archive is intentionally runtime-minimized. It includes only the
synthetic fixtures required by its built-in proof, not the broader private
development corpus or any real course export.

## Verification

```bash
.venv/bin/python scripts/vendor_from_workbench.py --check
.venv/bin/python scripts/make_release_asset.py --check-only
```

The public runtime carries the built-in synthetic proof, not the private test
corpus. Run the full `python -m pytest` suite in the development repository.
See [release changes and boundaries](CHANGELOG.md).

The release is licensed under AGPL-3.0-or-later, with commercial terms
available by agreement. See [LICENSE](LICENSE),
[LICENSE_POSTURE.md](LICENSE_POSTURE.md), and [COMMERCIAL.md](COMMERCIAL.md).

The provisional Unbind envelope and synthetic progress event format remain
explicitly draft. Phase 5 campaign tools, tenant evidence, credentials, and
live Brightspace operations are excluded.
