# Export report as PDF

## Goal
Account owners can download their monthly usage report as a PDF from the Reports page.

## Assumptions
- Next.js frontend, Python API, Postgres; reports data already exists in the `usage_events` table.
- PDF generation happens server-side.

## Layers touched
| Layer | Location | Notes |
|---|---|---|
| UI | `web/app/reports/page.tsx` | Add "Export PDF" button |
| API | `api/routes/reports.py` | New `GET /reports/{month}/pdf` |
| Logic | `api/services/report_pdf.py` | New; builds the PDF |
| Storage | `usage_events` table | Read-only |
| External | none | |

## Phase 1 — Tracer bullet
**User action:** Click "Export PDF" and receive a downloaded PDF.

**Path:** Button → `GET /reports/2026-10/pdf` → service returns a one-page PDF containing the account name and the total event count from one real SQL query → browser downloads `report-2026-10.pdf`.

**Shortcuts allowed:** month hardcoded to the current month, no charts, default fonts.

**Acceptance criteria:**
- [ ] Clicking the button downloads a valid PDF showing the real account name and total
- [ ] End-to-end test: request the endpoint as a seeded user, assert `200`, `application/pdf`, and that the total appears in the extracted text

## Phase 2 — Real report contents
**Adds:** month picker in the UI, month parameter validated in the API, daily breakdown table in the PDF.

**Acceptance criteria:**
- [ ] Any past month can be exported
- [ ] Test: invalid month returns `400`; the table row count matches the days with events

## Phase 3 — Permissions and errors
**Adds:** only owners can export; empty months show a friendly message; UI shows errors.

**Acceptance criteria:**
- [ ] Non-owners get `403` and the button is hidden for them
- [ ] Test: permission and empty-month cases

## Phase 4 — Polish and performance
**Adds:** branding, charts, generation under 2s for the largest account (cache if needed).

**Acceptance criteria:**
- [ ] Load test on the largest seeded account under 2s

## Deferred
- Scheduled email delivery of reports
- CSV export

## Risks and open questions
- PDF library rendering on the production server image (Phase 1 exposes this)
- Query cost for large accounts (measured in Phase 4)
