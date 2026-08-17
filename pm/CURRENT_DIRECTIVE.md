# PM-0001 — Project Skeleton and Phase 0.1 Runtime Validation

Status: ISSUED  
Phase: 0.1  
Program state while active: `IMPLEMENTING`

## Goal

Establish a reviewable repository skeleton and produce evidence about the actual Podcast Publisher VM runtime before any application feature is designed around unverified assumptions.

## Required work

### A. Project skeleton

- Create placeholder directories for `backend/`, `frontend/`, `wordpress-plugin/`, `migrations/`, `tests/`, `scripts/`, and `.github/`.
- Use short README or keep files only where Git requires a tracked placeholder.
- Preserve the PM control files and do not replace their workflow.
- Propose, but do not silently finalize, toolchain or dependency versions that depend on VM evidence.
- Keep `.env.example` free of secrets and update it only when an actual required setting is discovered.

### B. Runtime inventory on the actual VM

Record commands, executable paths, versions, exit codes, and relevant configuration for:

- operating system, architecture, CPU, memory, and available disk
- Python and package tooling
- FFmpeg and ffprobe, including relevant codecs/filters
- Tesseract and available language data
- whisper.cpp executable, model availability, and acceleration support
- Docker and Docker Compose if they are intended for deployment
- filesystem mounts intended for incoming media, generated output, and persistent application state
- permissions needed for the planned web and worker service identities
- DNS and HTTPS access to explicitly required external endpoints, without transmitting secrets

### C. Safe validation and benchmarks

Using synthetic or explicitly approved non-production fixtures:

- verify read/write behavior on intended writable mounts with disposable files and clean them up
- verify intended read-only boundaries are not treated as writable
- run an FFmpeg/ffprobe smoke test and record duration, output metadata, and wall time
- run a Tesseract smoke test on a representative clean slide and record output plus wall time
- run whisper.cpp against a short English clip with available tiny, base, and base.en models; record availability, wall time, real-time factor, and a qualitative transcript note
- identify missing tools/models as evidence, not as permission to install or redesign
- record every result in `pm/TEST_MATRIX.md`

## Deliverables

- tracked placeholder project directories
- updated `pm/STATUS.json`
- completed Phase 0.1 rows in `pm/TEST_MATRIX.md` with reproducible evidence
- detailed entries in `pm/BLOCKERS.md` for anything that prevents acceptance
- proposed decision entries for any architecture or installation choice requiring approval
- a focused pull request with no application features

## Do not implement

- FastAPI routes, frontend behavior, database schema, queue/worker logic, Docker deployment, or CI pipelines
- audio-processing, OCR, transcription, matching, notes-generation, or publishing features
- WordPress or PowerPress integration
- production UI, production data migration, or production configuration
- unapproved package, system, model, or service installation
- any new phase

## Acceptance

PM-0001 is ready for review only when:

1. The requested placeholder directories exist without runnable feature code.
2. All inventory and smoke-test rows in `pm/TEST_MATRIX.md` are PASS, FAIL, BLOCKED, or NOT_AVAILABLE—never left ambiguous.
3. Each executed test records the VM context, command or procedure, observed result, and evidence location.
4. Intended storage paths and permissions are documented without exposing private content.
5. Missing prerequisites and failed checks have blocker entries with evidence and bounded options.
6. No credentials, production sermon content, model binaries, databases, or generated media are committed.
7. The diff contains no application feature implementation.
8. `pm/STATUS.json` names the branch and commit, summarizes results, and is set to `AWAITING_REVIEW`.
9. The pull request explains what was checked, limitations, risks, and any decisions requested.

Stop at `AWAITING_REVIEW`. Phase 0.2 requires a new directive.
