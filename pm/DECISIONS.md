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

The following entries were proposed by the implementation agent from PM-0001
VM evidence and decided by the Program Manager on 2026-08-17 (PR #1,
“Program Manager decision — complete PM-0001 on the designated VM”).

## D-008 — Phase 1 prerequisite provisioning on the runtime VM

Status: ACCEPTED WITH BOUNDS (Phase 0.1 completion scope)  
Date decided: 2026-08-17  
Evidence: P01-02, P01-05, P01-07, P01-11

PM-approved bounds:

- Operator may install `python3.13-venv`, `tesseract-ocr`, and
  `tesseract-ocr-eng`.
- Operator may install `git`, `build-essential`, `cmake`, `curl`, and
  `ca-certificates` as needed to build a pinned whisper.cpp release from
  its official repository.
- Download only the tiny, base, and base.en models required by PM-0001,
  recording versions, source, hashes when available, disk use, and
  commands.
- The current static FFmpeg/ffprobe build is accepted for Phase 0.1; do
  not replace it during this directive (standardization deferred).
- Docker/Compose vs systemd is deferred to the Phase 1 directive; Docker
  need not be installed to close PM-0001 (P01-11 stays NOT_AVAILABLE).

The VM lacks several Phase 1 prerequisites. Proposed operator-run
installation set (no passwordless sudo by design, so an operator executes):

- `python3.13-venv` — restores `python -m venv` (currently broken)
- `tesseract-ocr` + `tesseract-ocr-eng` — OCR runtime + English data
- whisper.cpp — either (a) `build-essential` + `git` to build from source,
  or (b) a prebuilt release binary downloaded by the agent; (a) is more
  maintainable, (b) avoids a compiler footprint
- FFmpeg provenance — keep the current static `~/.local/bin` builds or
  switch to distro `ffmpeg` packages (**superseded by the PM bounds
  above: static build accepted for Phase 0.1, standardization deferred**)
- Docker Engine + Compose **or** systemd units — deployment style choice
  (**deferred to Phase 1 by PM decision**)

## D-009 — Storage boundaries and mount design

Status: ACCEPTED — Option A  
Date decided: 2026-08-17  
Evidence: P01-09

PM-approved design:

- Unraid share passthrough (virtiofs/9p, whichever the installed
  Unraid/QEMU stack supports cleanly) for incoming sermon media and
  podcast output.
- Guest paths remain `/mnt/sermons/incoming` and `/mnt/podcast`.
- Application state stays on the guest’s persistent disk at
  `/var/lib/podcast-publisher`, with explicit ownership and backup
  planning.
- The operator configures host-side share mappings and creates/owns the
  state directory. Host paths, private network details, and credentials
  are not published.

Superseded option notes (B/C — SMB mounts, local disks + sync — not
chosen).

## D-010 — Runtime VM resource sizing

Status: ACCEPTED  
Date decided: 2026-08-17  
Evidence: P01-01, P01-07

PM-approved: resize the designated VM to at least **4 vCPU and 8 GiB RAM
before whisper.cpp benchmarking**, and re-record P01-01 afterward so the
evidence reflects the representative runtime.
