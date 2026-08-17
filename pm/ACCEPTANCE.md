# Acceptance Policy

## Evidence standard

A milestone is accepted from code, tests, evidence, and the directive’s criteria. A status claim by itself is insufficient.

Evidence should identify:

- directive and test identifier
- branch and commit
- environment and relevant versions
- exact command or reproducible procedure
- expected and observed result
- pass/fail/blocker disposition
- artifact, log, screenshot, or output location when useful
- limitations and cleanup performed

Evidence must not expose credentials, private sermon content, or unnecessary personal data.

## Review states

- `IMPLEMENTING`: approved work is in progress
- `TESTING`: implementation is in its validation cycle
- `BLOCKED`: progress is prevented by a documented external or technical blocker
- `NEEDS_DECISION`: a bounded Program Manager decision is required
- `AWAITING_REVIEW`: implementation agent asserts the directive is ready for independent review
- `APPROVED`: Program Manager accepted the directive or milestone
- `FAILED`: acceptance was attempted and failed without a current recovery plan

Only the Program Manager may set a phase gate to `APPROVED`.

## Universal acceptance gates

Before review:

1. The work stays inside the current directive.
2. Relevant automated and manual tests are recorded.
3. Security and secret-handling checks pass.
4. Documentation, status, blockers, decisions, and test matrix agree.
5. Retry, cleanup, and failure behavior are tested when applicable.
6. Known limitations and skipped checks are explicit.
7. The branch and commit under review are identifiable.
8. No production content or credentials are present in the diff.

## PM-0001 acceptance

PM-0001 is accepted only when its criteria in `CURRENT_DIRECTIVE.md` pass and the reviewer can independently verify:

- placeholder skeleton only; no runnable application features
- actual-VM inventory for Python, FFmpeg/ffprobe, Tesseract, whisper.cpp, storage, permissions, and required networking
- safe smoke tests and available-model benchmarks using non-production fixtures
- explicit disposition of missing dependencies
- complete Phase 0.1 test-matrix evidence
- `STATUS.json` set to `AWAITING_REVIEW`

Approval authorizes no later roadmap work. The Program Manager must issue the next directive separately.
