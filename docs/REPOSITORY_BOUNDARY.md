# Quiz Binder repository boundary

Status: approved by the operator on 2026-07-20 and extended for the public
runtime release decision on 2026-09-04.

## Decision

The GitHub repository is named `brightspace-quiz-bundle`. **Quiz Binder** is the
product name presented to people.

The boundaries are:

| Surface | Owns |
| --- | --- |
| `coursecraft_workbench` | Quiz contracts, extraction and normalization meaning, capability registry and evidence levels, corpus research, live-verification records, and upstream regression proof. |
| `brightspace-quiz-bundle` | Portable quiz producer, pinned downstream distribution, release identity and assets, installation, and the terminal Quiz Workshop. |
| `coursecraft-workshop-space` | A hosted Quiz Binder only if a later zero-install need is established and separately authorized. |

The bundle is not a second semantic owner. Its promoted files are byte-bound to
an immutable Workbench commit by `upstream/workbench_pin.json`, and its checks
fail on local edits or source drift.

## Activation pin

The activation source is Workbench `main` commit
`5f1b78b3da8d1e5701ffff4e302b8503b6cd17f6`, after the Phase 5 remediation,
live restart, audit, and capability-registry update were complete.

That commit remains the historical activation point. The current immutable
promotion commit and the digest of every carried file are recorded in
`upstream/workbench_pin.json`.

## Included now

- extraction and normalization;
- direct real-course Unbind extraction, reviewer workbook, Reading Room,
  durable receipt, and verification;
- review station, review queues, and plain-language state copy;
- share sanitization, marginalia re-ingest, and proposer/approver promotion;
- strict authoring readiness and exact-candidate authorization;
- package assembly, conformance validation, structural validation, and package
  diffing;
- quiz schemas, capability registry, and deliberately synthetic proof fixtures;
- mechanical promotion, drift checking, and deterministic release preparation.
- guided real-export orchestration across Unbind, reviewer baseline/working
  copies, Compose readiness, and strictly gated local Rebind.

## Explicitly excluded

- raw exports or private authored content;
- sandbox identifiers or tenant credentials;
- Phase 5 campaign manifests, operator kits, observation cards, and live
  receipts;
- claims that exceed the pinned capability registry;
- hosted Hall code;
- automatic instance-evidence promotion or unrestricted real-course Rebind;
- the private development corpus and historical course-derived fixtures from
  the public runtime archive;
- public visibility for the existing Git history until its historical fixture
  content is removed or replaced.

## Change rule

Behavioral or contract changes start in Workbench. Bundle-only changes may
alter distribution, installation, terminal presentation, or release mechanics,
but may not reinterpret upstream data or capability evidence.
