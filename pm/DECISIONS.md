# Decision Log

Decisions are append-only. Superseded decisions remain visible and point to the replacement.

## D-001 — GitHub is the control plane

Status: ACCEPTED  
Date: 2026-08-16

The repository records directives, status, evidence, blockers, decisions, change orders, code, and review history. Conversational memory is not the project record.

## D-002 — Independent phase gates

Status: ACCEPTED  
Date: 2026-08-16

The implementation agent may set `AWAITING_REVIEW`; only the Program Manager may approve a milestone or authorize a new major phase.

## D-003 — Separate web and worker processes

Status: ACCEPTED IN PRINCIPLE; runtime not yet validated  
Date: 2026-08-16

Long-running processing belongs in a separately supervised worker, not inside FastAPI requests. SQLite is the initial shared state store. Redis and Celery are not approved for V1.

## D-004 — Prove PowerPress before feature build-out

Status: ACCEPTED  
Date: 2026-08-16

Phase 0 must prove the WordPress/PowerPress enclosure path using a disposable draft before OCR, transcription, or notes features are relied upon. A small authenticated WordPress bridge is preferred if standard REST metadata is insufficient. Direct database modification is not approved.

## D-005 — Slides are authoritative structure

Status: ACCEPTED IN PRINCIPLE  
Date: 2026-08-16

Ordered slides define the sermon outline and point order. Transcript material supplies context and expansion. The matching design must respect sequence and allow weak or unmatched results.

## D-006 — Cloud AI use fails closed

Status: ACCEPTED IN PRINCIPLE  
Date: 2026-08-16

Only explicitly configured providers may be used. The application must not infer that a provider or request is free and must not silently switch to a paid option.

## D-007 — Phase 0.1 precedes application implementation

Status: ACCEPTED  
Date: 2026-08-16

The actual VM runtime, storage, permissions, and tool performance must be recorded before application features or dependency choices are approved.
