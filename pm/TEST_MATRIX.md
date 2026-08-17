# Test Matrix

Status values: `NOT_RUN`, `PASS`, `FAIL`, `BLOCKED`, `NOT_AVAILABLE`.

Every executed row must include the actual VM context, command or procedure, observed result, and evidence location. Never paste secrets or private sermon content.

## Phase 0.1 — PM-0001

| ID | Check | Expected | Status | Evidence |
|---|---|---|---|---|
| P01-01 | OS, architecture, CPU, memory, disk inventory | Target VM characteristics recorded | NOT_RUN | Pending |
| P01-02 | Python and package tooling | Paths and versions recorded; supported baseline proposed | NOT_RUN | Pending |
| P01-03 | FFmpeg and ffprobe inventory | Paths, versions, required codecs/filters recorded | NOT_RUN | Pending |
| P01-04 | FFmpeg/ffprobe smoke benchmark | Synthetic input processed; output metadata and wall time recorded | NOT_RUN | Pending |
| P01-05 | Tesseract inventory | Path, version, and language data recorded | NOT_RUN | Pending |
| P01-06 | Tesseract slide smoke benchmark | Representative non-production slide OCR and wall time recorded | NOT_RUN | Pending |
| P01-07 | whisper.cpp inventory | Executable, build/acceleration details, and available models recorded | NOT_RUN | Pending |
| P01-08 | whisper.cpp model benchmark | Available tiny/base/base.en results, wall time, real-time factor, and quality note recorded | NOT_RUN | Pending |
| P01-09 | Mounts and permissions | Intended input/output/state paths and safe disposable access checks recorded | NOT_RUN | Pending |
| P01-10 | Required network access | DNS/HTTPS checks to approved endpoints recorded without secrets | NOT_RUN | Pending |

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
