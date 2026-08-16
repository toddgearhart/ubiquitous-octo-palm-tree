# Podcast Publisher

Podcast Publisher is a planned self-hosted workflow for turning sermon audio and slides into reviewable podcast content and a WordPress/PowerPress draft.

This repository is currently in **Phase 0**. No application features have been implemented. GitHub is the coordination and audit layer between the Program Manager and the implementation agent.

## Start here

1. Read [`AGENTS.md`](AGENTS.md).
2. Read [`pm/CURRENT_DIRECTIVE.md`](pm/CURRENT_DIRECTIVE.md).
3. Check [`pm/STATUS.json`](pm/STATUS.json).
4. Work only within the current directive.
5. Record test evidence in [`pm/TEST_MATRIX.md`](pm/TEST_MATRIX.md).

## Current directive

**PM-0001 — Project skeleton and Phase 0.1 runtime validation**

The first implementation task is limited to repository scaffolding and validation of the actual VM runtime. It does not authorize podcast-processing or publishing features.

## Planned architecture

The intended runtime is a FastAPI web process and a separate worker process sharing SQLite and persistent storage. External boundaries include FFmpeg/ffprobe, Tesseract, whisper.cpp, an explicitly configured AI provider, and WordPress/PowerPress. The architecture remains provisional until Phase 0 validates the environment and integrations.

See [`pm/PROJECT.md`](pm/PROJECT.md), [`pm/ARCHITECTURE.md`](pm/ARCHITECTURE.md), and [`pm/ROADMAP.md`](pm/ROADMAP.md) for the controlled plan.

## Security

Do not commit credentials, production content, sermon media, transcripts, model files, databases, or generated artifacts. Copy `.env.example` to `.env` only in a trusted local environment.
