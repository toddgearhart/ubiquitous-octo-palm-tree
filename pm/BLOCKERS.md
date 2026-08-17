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

No open blockers. Two decisions are requested in `pm/STATUS.json` (VM identity
confirmation and the proposed D-008/D-009/D-010 entries in `pm/DECISIONS.md`);
these are review questions, not blockers — the PM-0001 evidence work itself is
complete.

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
