# Roadmap

Every phase is gated. Dates are intentionally omitted until Phase 0 establishes the target VM’s capabilities.

## Phase 0 — Prove external boundaries

### 0.1 Runtime validation — active

Inventory and benchmark Python, FFmpeg/ffprobe, Tesseract, whisper.cpp, filesystem mounts, permissions, and required network access on the actual VM. Establish the placeholder repository skeleton. Directive: PM-0001.

### 0.2 Representative fixtures and golden references

Approve synthetic or sanitized sample audio/slides, expected OCR/transcript characteristics, and one existing known-good PowerPress episode for comparison.

### 0.3 WordPress REST boundary

Prove least-privilege authentication, disposable image/media upload, draft creation, featured image assignment, cleanup, and safe error handling.

### 0.4 PowerPress proof of concept

Prove a disposable draft with a correct enclosure, player, RSS entry, size, MIME type, duration, and cleanup. Decide whether a companion WordPress bridge is required.

## Phase 1 — Application foundation

Create the approved Python project, configuration validation, database migrations, web/worker process boundaries, health checks, logging, and test harness. No media pipeline yet.

## Phase 2 — Episode intake and durable jobs

Implement episode records, safe file discovery, metadata entry, transactional job claiming, leases, heartbeats, cancellation, stale-job recovery, and stage persistence.

## Phase 3 — Audio pipeline

Implement fingerprinted two-pass normalization, encoding, atomic outputs, final MP3 validation, retry behavior, and representative quality checks.

## Phase 4 — Slides and OCR

Implement ordered slide intake, per-slide OCR and confidence, preserved boundaries, structured output, and correction/retry flow.

## Phase 5 — Transcription

Implement whisper.cpp presets, timestamped raw transcript, cleaned derivative, model/configuration fingerprints, and performance evidence.

## Phase 6 — Outline and source fusion

Build the slide-authoritative outline, sequence-aware transcript matching, source attribution, and explicit weak/unmatched states.

## Phase 7 — Episode-content generation

Implement structured notes generation through explicitly configured providers, fail-closed provider policy, editable outputs, grounding checks, and optional local fallback experiments.

## Phase 8 — Review and WordPress draft

Implement the operator review workflow, media upload, featured image, idempotent draft creation/update, PowerPress enclosure path, and deliberate publish handoff.

## Phase 9 — Hardening and release

Complete recovery drills, backup/restore evidence, security review, observability, performance testing, deployment/runbook documentation, and end-to-end acceptance.
