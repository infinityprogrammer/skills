# `notes sync` command

## Goal
Users of the `notes` CLI can sync their local notes folder to a remote server with one command.

## Assumptions
- Go CLI; the server exposes a REST API with token auth; notes are Markdown files.

## Layers touched
| Layer | Location | Notes |
|---|---|---|
| CLI | `cmd/sync.go` | New subcommand |
| Config | `internal/config/` | Read server URL and token |
| Logic | `internal/sync/` | New; diff local vs remote |
| Storage | local notes folder | Read files |
| External | Notes server API | `PUT /notes/{id}` |

## Phase 1 — Tracer bullet
**User action:** Run `notes sync` and see one local note appear on the server.

**Path:** `notes sync` → read config → read one file from the notes folder → `PUT` it to the server → print `synced 1 note`.

**Shortcuts allowed:** uploads only the first file found, no diffing, no conflict handling.

**Acceptance criteria:**
- [ ] Running the command against a dev server creates the note remotely
- [ ] End-to-end test: run the binary against a local test server and assert the note was received

## Phase 2 — Full upload
**Adds:** upload all changed files using modification time; progress output.

**Acceptance criteria:**
- [ ] Only changed files are uploaded on a second run
- [ ] Test: modify one of three files and assert exactly one upload

## Phase 3 — Two-way sync and conflicts
**Adds:** download remote changes; detect conflicts and write `.conflict` copies.

**Acceptance criteria:**
- [ ] Remote edits appear locally; simultaneous edits produce a conflict file
- [ ] Test: conflict scenario against the test server

## Phase 4 — Robustness
**Adds:** retries, `--dry-run`, clear errors for bad token or offline server.

## Deferred
- Background/watch mode
- Encryption at rest

## Risks and open questions
- Auth flow and token storage (Phase 1 exposes this)
- Clock skew affecting modification-time diffing
