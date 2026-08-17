# Test Matrix

Status values: `NOT_RUN`, `PASS`, `FAIL`, `BLOCKED`, `NOT_AVAILABLE`.

Every executed row must include the actual VM context, command or procedure, observed result, and evidence location. Never paste secrets or private sermon content.

## Phase 0.1 — PM-0001

**Environment under test (authoritative run):** the Debian 13 KVM guest
(`debian`, user `hillside`) running on the Unraid host on the operator’s
LAN (private LAN/Tailscale identifiers are intentionally redacted from
this public repository; host identity was evidenced by guest DMI
`QEMU / Standard PC (Q35 + ICH9)` and the Unraid webGUI `/Main` redirect
on the host — full identifiers are on record with the Program Manager).
Run date: 2026-08-17, branch `feature/pm-0001-skeleton`. All evidence was
collected **inside the guest**; the Program Manager confirmed this guest
as the designated runtime VM on 2026-08-17 (PR #1 decision comment).

**Provisioning status (post-decision, 2026-08-17T01:54Z):** the operator
has begun the approved D-008 installs — `python3.13-venv` is now present
and `python3 -m venv` succeeds. Still pending: tesseract packages,
build tools for whisper.cpp, the D-009 share passthrough mounts/state
directory, and the D-010 resize (guest still 1 vCPU / 962 MiB at the
timestamp above). Rows below reflect the pre-provisioning run; the PM’s
required rerun batch (P01-01, P01-02, P01-05..P01-09) executes after
provisioning completes.

Executed row summary: **4 PASS, 2 FAIL, 5 NOT_AVAILABLE** (matches
`pm/STATUS.json`).

| ID | Check | Expected | Status | Evidence |
|---|---|---|---|---|
| P01-01 | OS, architecture, CPU, memory, disk inventory | Target VM characteristics recorded | PASS | P01-01 record below |
| P01-02 | Python and package tooling | Paths and versions recorded; supported baseline proposed | FAIL | P01-02 record below — venv/ensurepip missing |
| P01-03 | FFmpeg and ffprobe inventory | Paths, versions, required codecs/filters recorded | PASS | P01-03 record below (limitation: user-space static builds) |
| P01-04 | FFmpeg/ffprobe smoke benchmark | Synthetic input processed; output metadata and wall time recorded | PASS | P01-04 record below |
| P01-05 | Tesseract inventory | Path, version, and language data recorded | NOT_AVAILABLE | Absent from the VM — P01-05 record below |
| P01-06 | Tesseract slide smoke benchmark | Representative non-production slide OCR and wall time recorded | NOT_AVAILABLE | Depends on P01-05 |
| P01-07 | whisper.cpp inventory | Executable, build/acceleration details, and available models recorded | NOT_AVAILABLE | Absent; no compiler toolchain — P01-07 record below |
| P01-08 | whisper.cpp model benchmark | Available tiny/base/base.en results, wall time, real-time factor, and quality note recorded | NOT_AVAILABLE | Depends on P01-07 |
| P01-09 | Mounts and permissions | Intended input/output/state paths and safe disposable access checks recorded | FAIL | Intended paths absent — P01-09 record below |
| P01-10 | Required network access | DNS/HTTPS checks to approved endpoints recorded without secrets | PASS | P01-10 record below |
| P01-11 | Docker / Docker Compose (deployment option per ARCHITECTURE) | Presence recorded if deployment intends it | NOT_AVAILABLE | Absent from the guest — P01-11 record below |

---

## Phase 0.1 evidence records

### P01-01 — OS, architecture, CPU, memory, disk

- Date/time: 2026-08-17 ~20:59 UTC (guest local)
- Directive: PM-0001
- Branch/commit: `feature/pm-0001-skeleton` (evidence commit; see STATUS)
- VM/environment: Debian 13 (trixie) KVM guest on the Unraid host
  (identifiers redacted; see environment note above)
- Command or procedure: `uname -srm`; `/etc/os-release`; `lscpu`; `free -h`; `df -h /`; `uptime`; `/proc/cpuinfo` flags
- Expected: target VM characteristics recorded
- Observed: Linux 6.12.101+deb13-amd64 x86_64; **1 vCPU** (Xeon E5-2650 v2
  @ 2.60GHz, 1 thread — Ivy Bridge class); CPU flags include `avx`, `f16c`;
  **no `avx2`, no `fma`**; **962 MiB RAM**; virtio disk `vda` 20 G with 15 G
  free on `/`; uptime 2:39, load 0.07
- Status: PASS (recorded; sizing concern raised as proposed decision D-010)
- Evidence location: this record; commands re-runnable verbatim
- Cleanup: n/a
- Limitations: CPU/RAM are small for whisper.cpp (see D-010); resize is a
  host-side VM configuration change requiring the operator

### P01-02 — Python and package tooling

- Date/time: 2026-08-17 ~20:59 UTC
- Command or procedure: `which python3 pip3`; `python3 --version`;
  `python3 -m pip --version`; `python3 -c "import sqlite3, venv,
  ensurepip"`; `python3 -m venv /tmp/venv-smoke`
- Expected: paths and versions recorded; supported baseline proposed
- Observed: `/usr/bin/python3` **3.13.5**; pip **26.2.1** (user install at
  `~/.local`) functioning with `--user --break-system-packages`;
  `sqlite3` module OK (SQLite 3.40.1-era system build);
  **`venv`/`ensurepip` BROKEN** — `python3 -m venv` fails:
  “No module named ‘ensurepip’ … apt install python3.13-venv”
- Status: FAIL — core interpreter and pip are healthy; the venv gap blocks
  the Phase 1 packaging baseline (isolated virtualenvs)
- Proposed baseline: Python 3.13 + `python3.13-venv` + pip, per proposed
  decision D-008
- Evidence location: this record
- Cleanup: `/tmp/venv-smoke` removed
- Limitations: remediation requires an operator `apt install` (no
  passwordless sudo for the agent — by design)

### P01-03 — FFmpeg and ffprobe inventory

- Date/time: 2026-08-17 ~21:00 UTC
- Command or procedure: `~/.local/bin/ffmpeg -version`;
  `-hide_banner -encoders | grep libmp3lame`; `-hide_banner -filters |
  grep loudnorm`; `-hide_banner -hwaccels`
- Expected: paths, versions, required codecs/filters recorded
- Observed: ffmpeg/ffprobe **N-126175-g0056dd32fd-20260816** static
  user-space builds at `~/.local/bin/` (not distro packages); encoders
  include **libmp3lame**, aac, libopus; filters include **loudnorm**
  (EBU R128 two-pass), dynaudnorm, silenceremove, speechnorm, aresample;
  `eburr128` filter absent from this build (loudnorm covers the
  ARCHITECTURE requirement); hwaccels listed (cpu, cuda, vaapi, …) but
  the guest has no GPU — CPU-only processing
- Status: PASS
- Evidence location: this record
- Cleanup: n/a
- Limitations: (1) static builds, not system packages — PATH must include
  `~/.local/bin` (non-login shells, including the agent harness, do not
  inherit it); (2) provenance is a manual download; Phase 1 should decide
  between distro `ffmpeg` packages and these builds (folded into D-008)

### P01-04 — FFmpeg/ffprobe smoke benchmark

- Date/time: 2026-08-17 ~21:01 UTC
- Fixture: fully **synthetic** 30 s stereo WAV (440 Hz + 659 Hz sines,
  tremolo) — no sermon media
- Command or procedure: lavfi synthesis → `-c:a libmp3lame -b:a 128k -ar
  44100 -ac 2` encode → ffprobe JSON validation; wall times via
  `date +%s.%N` deltas
- Expected: synthetic input processed; output metadata and wall time recorded
- Observed: WAV synthesis **0.29 s**; MP3 encode **1.19 s** (~25×
  realtime on 1 vCPU); output verified: `codec_name=mp3`,
  `sample_rate=44100`, `channels=2`, `duration=30.000000`,
  `bit_rate=128297`, size 481114 B
- Status: PASS
- Evidence location: this record
- Cleanup: `/tmp/p01.wav`, `/tmp/p01.mp3` removed (verified)
- Limitations: sine-wave input does not exercise speech dynamics or the
  loudnorm two-pass path; a speech-like fixture belongs to Phase 0.2

### P01-05 — Tesseract inventory

- Command or procedure: `which tesseract`; package check
- Observed: **not installed**; no language data present
- Status: NOT_AVAILABLE
- Disposition: recorded as evidence, not permission to install —
  proposed decision D-008 includes `tesseract-ocr` + `tesseract-ocr-eng`
  for operator approval

### P01-06 — Tesseract slide smoke benchmark

- Status: NOT_AVAILABLE — depends on P01-05
- Disposition: to be executed after D-008 approval, using a synthetic
  slide fixture (Phase 0.2 fixture set)

### P01-07 — whisper.cpp inventory

- Command or procedure: `which whisper-cli whisper-server whisper main`;
  compiler check `which gcc g++ cc make cmake`; model dirs checked
- Observed: **no whisper.cpp executable**; **no compiler toolchain**
  (gcc/g++/make/cmake all absent) so a source build is not currently
  possible in the guest; no model files (`ggml-*.bin`) anywhere
- Relevant hardware: 1 vCPU, AVX/F16C only (no AVX2/FMA) — expect modest
  throughput; quantized tiny/base models are the realistic candidates
  (see proposed D-010 sizing)
- Status: NOT_AVAILABLE
- Disposition: recorded as evidence, not permission to install/build —
  proposed decision D-008 covers build tools vs prebuilt binary choice

### P01-08 — whisper.cpp model benchmark

- Status: NOT_AVAILABLE — depends on P01-07
- Disposition: after D-008/D-010 approval, benchmark tiny / tiny.en /
  base / base.en (availability permitting) with wall time, real-time
  factor, and a qualitative transcript note

### P01-09 — Mounts and permissions

- Date/time: 2026-08-17 ~21:02 UTC
- Command or procedure: `ls -la /mnt`; `ls -d /mnt/sermons /mnt/podcast
  /var/lib/podcast-publisher`; `test -w` probes on `/mnt`, `/tmp`, home
- Expected: intended input/output/state paths and safe disposable access
  checks recorded
- Observed: `/mnt` is **empty** and root-owned (not writable by
  `hillside`); `/mnt/sermons`, `/mnt/podcast`,
  `/var/lib/podcast-publisher` **do not exist**; `/tmp` and `$HOME` are
  writable (disposable-file probes created and removed cleanly). No
  Unraid share is passed through to this guest (no virtiofs/9p mounts in
  `/proc/mounts` beyond system filesystems).
- Status: FAIL — the intended storage boundaries from `.env.example`
  (placeholders) are not provisioned; this requires a host-side VM
  configuration change (virtiofs/9p passthrough of Unraid shares, or SMB
  mounts, plus an admin-created state directory) — proposed decision D-009
- Evidence location: this record
- Cleanup: probe files removed
- Limitations: read-only-boundary verification is meaningless until the
  mounts exist; deferred to post-D-009

### P01-10 — Required network access

- Date/time: 2026-08-17 ~21:02 UTC
- Command or procedure: per-host DNS resolution (`socket.gethostbyname`)
  + `curl -s -o /dev/null -w '%{http_code}' https://<host>/`
- Endpoints tested: `github.com` (control plane / git), `pypi.org` and
  `files.pythonhosted.org` (future Python dependencies)
- Observed: all three resolve and answer **HTTPS 200**; no secrets
  transmitted
- Status: PASS
- Limitations: WordPress and AI-provider endpoints are intentionally
  untested — none are approved yet (Phase 0.3 / D-006 fail-closed policy)

### P01-11 — Docker / Docker Compose (supplementary)

- Command or procedure: `which docker docker-compose`; `docker compose version`
- Observed: **not installed** in the guest
- Status: NOT_AVAILABLE
- Disposition: ARCHITECTURE lists Compose as the *current deployment
  preference, not approved*. Evidence: a systemd-service deployment
  directly in this VM is equally viable (Python runtime present); the
  Compose-vs-systemd choice is folded into proposed decision D-008/D-009
  review rather than pre-decided here

## Superseded context

The draft-PR “dev sandbox” observations (same machine, informal run) are
superseded by the authoritative records above; no sandbox numbers are
relied upon for decisions.

## Evidence record template

### TEST-ID — Title

- Date/time:
- Directive:
- Branch/commit:
- VM/environment:
- Command or procedure:
- Fixture:
- Expected:
- Observed:
- Status:
- Evidence location:
- Cleanup:
- Limitations:
