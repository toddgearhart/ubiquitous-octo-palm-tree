# Standing Instructions for Coding Agents

## Authority and order of work

1. Read `pm/CURRENT_DIRECTIVE.md`, `pm/STATUS.json`, `pm/ACCEPTANCE.md`, and `pm/TEST_MATRIX.md` before changing the repository.
2. Work only on the active directive. Do not begin another major phase without written Program Manager approval.
3. Treat GitHub as the source of truth. Important status, evidence, decisions, blockers, and scope changes belong in the repository.
4. Explicit instructions in the current directive override general guidance in this file. If two controlled documents conflict, stop and record a question in `pm/STATUS.json`.

## Implementation rules

- Use a focused feature, fix, or chore branch and a pull request when the repository workflow permits it.
- Keep changes small enough to review against one directive.
- Do not silently change an architectural decision. Add a proposal to `pm/DECISIONS.md` and set status to `NEEDS_DECISION` when approval is required.
- Do not implement work listed under “Do Not Implement” in the current directive.
- Add tests appropriate to every behavior change.
- Prefer reversible, idempotent operations and explicit failure states.
- Do not add Redis, Celery, a vector database, or another infrastructure dependency without approval.
- Long-running processing must not execute inside a web request. The planned web and worker processes remain separate.
- Preserve raw inputs and source-derived data; generated or cleaned forms must be stored separately.
- User-entered sermon title and date remain authoritative.

## Status protocol

Update `pm/STATUS.json` after a meaningful milestone, test run, blocker, scope question, or review request. Allowed states are:

- `IMPLEMENTING`
- `TESTING`
- `BLOCKED`
- `NEEDS_DECISION`
- `AWAITING_REVIEW`
- `APPROVED`
- `FAILED`

Use `pm/BLOCKERS.md` for detailed blocker evidence. If blocked after one reasonable implementation attempt, document the failure and options instead of repeatedly redesigning the system.

Before requesting review:

1. Run the relevant automated and manual checks.
2. Update `pm/TEST_MATRIX.md` with commands, results, environment, and evidence.
3. Update documentation and `pm/STATUS.json`.
4. Commit the changes and create or update the pull request.
5. Set state to `AWAITING_REVIEW`.

A milestone is not complete until the Program Manager records approval.

## Security and repository hygiene

Never commit:

- credentials, tokens, API keys, application passwords, or populated `.env` files
- production sermon WAV/MP3 files, slides, transcripts, notes, or WordPress content
- model binaries, generated artifacts, databases, logs containing secrets, or personal data

Use synthetic or explicitly approved fixtures. Redact secrets and private content from test evidence. Do not make destructive changes to the VM, WordPress, or production storage without explicit approval.
