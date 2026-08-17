# Project Charter

## Product

Podcast Publisher is a planned self-hosted application that helps a church turn sermon audio and ordered slide images into processed podcast audio, structured sermon notes, and a reviewable WordPress/PowerPress draft.

## Operating model

GitHub is the shared control plane:

- The Program Manager owns architecture, directives, acceptance, change orders, and phase approval.
- The implementation agent writes code, runs tests on the target VM, records evidence, and reports blockers.
- Completion requires code, tests, evidence, and acceptance—not a completion claim alone.

## Primary user outcome

A trusted operator can provide a sermon title, sermon date, source WAV, and ordered slides; review the derived material; and deliberately create or update a WordPress draft without duplicating media or posts.

## V1 goals

- Preserve user-entered title and sermon date as authoritative fields.
- Process and validate podcast-compatible MP3 audio.
- Retain per-slide OCR results and timestamped raw transcription.
- Use slides as the primary sermon outline and transcript material as supporting context.
- Generate structured, editable episode content without inventing unsupported claims.
- Create a reviewable WordPress/PowerPress draft through a proven integration.
- Recover safely from interruption and avoid duplicated expensive work or publishing artifacts.
- Run on the designated Unraid VM with persistent storage.

## Non-goals for initial delivery

- Automatic production publishing without human review.
- A vector database, Redis, or Celery.
- Excellent notes generation from a small local LLM as a V1 requirement.
- Multi-tenant hosting, mobile applications, or a general-purpose CMS.
- Replacing the church’s established WordPress/PowerPress workflow.
- Assuming a cloud provider is free; provider use must be explicitly configured and fail closed.

## Inputs and outputs

Expected inputs:

- user-entered sermon title and sermon date
- source WAV
- ordered PNG slide images
- explicitly configured processing and publishing settings

Expected outputs:

- validated MP3 and audio measurements
- per-slide OCR JSON
- raw timestamped transcript and cleaned derivative
- structured sermon outline and episode-content JSON
- human-reviewable WordPress draft and PowerPress enclosure
- logs and test evidence sufficient for review

## Constraints

- Validate the actual VM and external boundaries before application development.
- Keep the web process separate from long-running worker jobs.
- Use SQLite initially, with transactional job claiming and leases.
- Store sermon date separately from publication date.
- Treat credentials and production sermon content as non-repository data.
- Require Program Manager approval at phase gates.

## Success

V1 succeeds when every approved acceptance test passes on the target VM, recovery and idempotency behavior is demonstrated, a representative episode can be reviewed end to end, and the Program Manager approves the release evidence.
