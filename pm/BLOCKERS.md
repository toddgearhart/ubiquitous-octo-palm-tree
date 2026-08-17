# Blockers

Open blockers are listed below; resolved entries move to the bottom archive.

## Entry format

### B-NNN — Short title

Status: OPEN | RESOLVED | ACCEPTED_RISK  
Severity: LOW | MEDIUM | HIGH | CRITICAL  
Directive: PM-NNNN  
Owner: name or role

#### Problem

A precise description of what prevents acceptance.

#### Evidence

Commands, observations, errors, environment, and relevant artifact links.

#### Attempts

Bounded steps already tried and their results.

#### Options

A. First option with cost and risk.  
B. Second option with cost and risk.

#### Recommendation

The implementation agent’s recommendation, without treating it as approval.

#### Program Manager decision

Pending, or the dated decision and reference.

#### Resolution

When resolved, record the branch, commit, test, and evidence that closed the blocker.

---

## B-003 — Claimed D-008/D-009/D-010 provisioning not present in the designated VM

Status: OPEN  
Severity: HIGH  
Directive: PM-0001  
Owner: operator / Program Manager

#### Problem

On 2026-08-17 the operator reported D-008, D-009, and D-010
provisioning complete and directed execution of the authoritative rerun
batch (P01-01/02/05/06/07/08/09). Agent verification of the designated
runtime VM (the PM-confirmed Debian 13 KVM guest) found **none of the
approved provisioning present**, including a regression of a previously
verified item. The rerun batch cannot be executed truthfully in this
state, and PM-0001 acceptance cannot be claimed.

#### Evidence

Snapshot `2026-08-17T11:24:30Z` (guest uptime 13:04 — no reboot since
2026-08-16T22:20Z):

- **D-010 (resize): FAIL** — `nproc` = 1; RAM = 962 MiB; CPU unchanged
  (Xeon E5-2650 v2). Required: ≥4 vCPU / 8 GiB before whisper benchmarking.
- **D-008 (tesseract): ABSENT** — no `tesseract` binary; dpkg has neither
  `tesseract-ocr` nor `tesseract-ocr-eng`.
- **D-008 (build tools): ABSENT** — `gcc`, `g++`, `make`, `cmake` all
  missing; no `build-essential` package. whisper.cpp cannot be built, and
  no prebuilt whisper binary or `~/whisper.cpp` checkout exists.
- **D-008 (venv): REGRESSION** — `python3 -m venv` fails again with “No
  module named ‘ensurepip’”. It succeeded at 2026-08-17T01:54Z after the
  operator installed `python3.13-venv`; that package is no longer
  effective in the current boot state.
- **D-009 (storage): ABSENT** — `/mnt/sermons/incoming`, `/mnt/podcast`,
  and `/var/lib/podcast-publisher` do not exist; zero virtiofs/9p mounts.
- **apt history:** last entries are 2026-08-16 20:41 local time
  (`apt-get install -y git`, then `gh`) — no package activity since,
  consistent with the other observations.

#### Attempts

- Agent re-checked user-space paths (`~/.local/bin`), alternate names,
  dpkg database, apt logs, mount table, and reboot state before
  concluding the provisioning is absent (the harness shell previously
  masked `~/.local/bin`, so PATH issues were ruled out first).
- No rerun rows were executed: every row in the batch depends on the
  missing provisioning (P01-01 on the resize, P01-02 on venv, P01-05/06
  on tesseract, P01-07/08 on the whisper build, P01-09 on the mounts),
  and re-recording absence would duplicate existing FAIL/NOT_AVAILABLE
  evidence.
- No installation was attempted by the agent: no passwordless sudo by
  design, and D-008 bounds assign installation to the operator.

#### Options

A. Operator re-applies the approved provisioning **to this designated
guest** (packages, pinned whisper.cpp build tools, share passthrough,
state directory, resize) and confirms completion; agent re-verifies and
   runs the batch.  
B. If the provisioning was applied to a different machine (e.g., the
   Unraid host or another guest), the PM identifies the correct designated
   VM and provides the agent an access path; the evidence rows are then
   re-run there.  
C. PM invokes the operator-assisted procedure (per the B-001 decision
   fallback) to execute the batch with the operator at the console and the
   agent transcribing verified outputs.

#### Recommendation

A first (cheapest, matches the PM-confirmed designated VM); B or C if the
provisioning was in fact applied elsewhere.

#### Program Manager decision

Pending.

# Archive

## B-001 — Phase 0.1 VM inventory cannot be executed from the dev sandbox

Status: RESOLVED  
Severity: HIGH  
Directive: PM-0001  
Owner: implementation agent

#### Program Manager decision

2026-08-17, PR #1 checkpoint: **Option A** — execute PM-0001 on the actual
Unraid VM; sandbox results remain non-authoritative; if direct execution were
impossible, stop and request the Option B operator-assisted procedure.

#### Resolution

Evidence gathered 2026-08-17 and committed on `feature/pm-0001-skeleton`
(see `pm/TEST_MATRIX.md` P01-01..P01-11): the execution environment was
determined to be a Debian 13 KVM guest **on** the Unraid host (host
identity evidenced by the Unraid webGUI `/Main` redirect; private
identifiers redacted from this public repository and on record with the
PM), and the full inventory ran inside that guest. The PM confirmed the
guest as the designated runtime VM on 2026-08-17 (PR #1 decision
comment). Direct host access remains unavailable (SSH requires operator
credentials), which is acceptable because all PM-0001 checks execute in
the guest.

## B-002 — Prior implementation overshoot must be dispositioned (not part of PM-0001)

Status: RESOLVED  
Severity: MEDIUM  
Directive: PM-0001  
Owner: implementation agent

#### Program Manager decision

2026-08-17, PR #1 checkpoint: **Option A** — retain the overshoot material
in the sandbox attic for possible Phase 1 consideration; do not push, merge,
copy into the PR, or treat it as approved architecture; note that the attic
may be ephemeral.

#### Resolution

Disposition recorded here. The material remains only at
`~/pp-attic/pm-0001-overshoot/` on the guest (host `debian`, user
`hillside`) and is excluded from the repository. **Ephemerality note:** the
attic is not backed up and not in Git; it may disappear with the VM. The PM
may direct an archive branch later if preservation is wanted.
