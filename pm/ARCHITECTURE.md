# Architecture

Status: Planned; unverified until the relevant Phase 0 gates pass.

## System boundary

Browser
→ FastAPI web service
→ SQLite and persistent storage
→ separately supervised worker
→ FFmpeg/ffprobe, Tesseract, whisper.cpp, configured AI provider, and WordPress/PowerPress

The web process handles short requests and review workflows. The worker owns long-running media, OCR, transcription, generation, and publishing jobs. Long-running work must not run inside FastAPI request handling.

## Planned runtime processes

- `podcast-web`: FastAPI application and operator-facing API/UI
- `podcast-worker`: polling worker with transactional job claiming, leases, heartbeats, cancellation, and stale-job recovery
- SQLite: shared application and job state on persistent storage

Docker Compose on the Unraid VM is the current deployment preference, not an approved implementation in PM-0001.

## Planned storage boundaries

- incoming source media: read-only where practical
- generated episode artifacts: persistent and isolated by episode
- application database: persistent, backed up, and never stored in the repository
- transient work: temporary filenames followed by atomic rename after validation
- secrets: environment or secret store only

Exact mount paths must be discovered and recorded during Phase 0.1; examples in `.env.example` are placeholders.

## Episode lifecycle

Planned happy path:

`NEW → READY → PROCESSING_AUDIO → OCR_PROCESSING → TRANSCRIBING → BUILDING_OUTLINE → GENERATING_NOTES → READY_FOR_REVIEW → WORDPRESS_DRAFT → PUBLISHED`

Any stage may enter `FAILED`. The system must retain the last successful stage and enough evidence to retry safely.

## Job reliability

A job is expected to record at least:

- job and episode identifiers
- stage and status
- worker identifier
- claimed and heartbeat timestamps
- attempt count and last error
- cancellation request
- input/configuration fingerprint

Claims must be atomic. A processing job with an expired lease becomes recoverable according to an approved policy.

## Idempotency

Expensive and external operations must identify already completed work using fingerprints and stored external identifiers. Examples include audio processing, OCR, transcription, notes generation, media upload, and WordPress post creation. A retry updates the known draft rather than creating a duplicate.

## Media and content principles

- Validate final MP3 measurements after encoding.
- Start audio compatibility testing with 44.1 kHz joint stereo, 128 kbps, -16 LUFS integrated, and no more than -1 dBTP; Phase 0 evidence may change this decision.
- Preserve per-slide boundaries and OCR confidence.
- Preserve the timestamped raw transcript; cleaning produces a derivative.
- Slides define the main structure; transcript material expands it.
- Transcript-to-section matching should respect sermon order and permit weak or unmatched results rather than fabricate a match.
- Structured JSON is the canonical generated-content form; formatted content is a derivative.

## WordPress boundary

The PowerPress enclosure path is a known uncertainty. Phase 0 must prove the integration using a disposable draft and a known-good reference before the application relies on it. A small authenticated WordPress bridge is preferred if standard REST metadata is insufficient. Direct database modification is not approved.

## Security boundary

The application must use least-privilege credentials, never log secrets, validate file paths and media types, and require a deliberate operator action before publishing. Production content and credentials are outside Git.

## Deferred choices

Framework versions, container images, precise schema, queue timing, AI providers, and PowerPress integration details remain unapproved until the appropriate directive records evidence and a decision.
