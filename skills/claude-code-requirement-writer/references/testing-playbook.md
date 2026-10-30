# Real-AI SaaS test-prompt playbook

Read when generating a comprehensive testing phase, 360-degree audit, regression suite, or multiple-company conversation test prompt. Customize to the requested count and context; do **not** automatically impose 10 companies or three customers on every request.

## 1. Design a balanced, explicit matrix

- Select genuinely distinct company/service domains, preferably not repeated from earlier rounds if the user requests new ones.
- Define business type, realistic offering, pricing rule, goals, realistic policies, assigned existing plan, uniquely named assistant, and test-only company admins/staff.
- Exactly honor company/customer count. Each customer is independent with distinct identity and persisted conversation.
- Default customer personas when three per company: **A conversion**, **B enquiry only**, **C negotiation/difficult/changed requirements**. With two, use A conversion + B challenging/enquiry unless user directs otherwise.
- Use a language distribution table, not loose language bullets. For five languages and 30 conversations, allocate six each; for 10, two each. Do not claim equal coverage unless table arithmetic confirms it. A company should use distinct languages across personas where practical.
- Include domain-specific scenarios rather than merely duplicating generic scripts.

## 2. Execute honestly and preserve UI evidence

- Require actual configured AI provider invocation per primary conversation; execute multi-turn cases based on **real prior response**, not canned model replies.
- Route messages through actual BuyChat AI processing, store customer + model messages, tool/transaction references, timestamps, language, voice links, test marker, and reply counts in **development**.
- Verify visibility in Company Admin → Conversations and Platform Admin → Company Details → Conversations (read-only); attach direct dev URLs or exact IDs when available.
- Distinguish simulation transport from real model inference. If real AI provider calls are blocked, state **Blocked** and do not fabricate content.
- Never send to real customer phone numbers or perform real banking transactions. Use fake/isolated contact identities and **synthetic, prominently tagged** QR/media/evidence. An internal test-mode staff action may exercise state machine but must not masquerade as bank verification.
- Protect existing data; seed idempotently with run tag; preserve chats after completion. Respect "testing only" instruction even if defects are found.

## 3. E2E workflows

### Conversion
Customer intent → discovery → accurate recommendation → price/quote → availability/capacity → customer approval → order/booking creation → correct payment request → submitted evidence → pending review → authorized decision → proper confirmed status/notification, if preconditions met → visibility in UI and backend.

### Enquiry only
Questions → comparison → missing knowledge → no order/payment/booking → correct lead classification → eligible follow-up rules.

### Difficult customer
Complaints/emotion → negotiation/changes → no invented discounts/guarantees → escalations and badges, still AI enabled unless manually OFF/Take Over → proper quote revisions, cancellation/rescheduling if appropriate.

### Service-only round
Stress availability, staff/resource constraints, overlapping time slots, multiple bookings, cancellations, quotation approvals, service request vs confirmed booking, lifecycle and sidebar sync. Never claim a valid confirmed booking without real backend evidence.

## 4. Cross-cutting test catalogue

Use IDs for tests and list actual expected behavior. Include relevant subsets, not every test for every conversation.

- **AI:** correct business instructions, multilingual scripts, empathy, answer-directly exceptions, no first-message catalogue dump, context retention, hallucination and duplicate reply prevention.
- **Voice:** STT transcripts, normal voice → voice; voice product/service list or summary → text; TTS fallback and plan entitlement; language naturalness.
- **Buttons:** Malayalam/Manglish English headings/action labels, stable internal IDs; leave Hindi/Arabic behavior untouched except normal localization; RTL check for Arabic.
- **Transactions:** accurate pricing, discounts/tax, quote vs fixed price, COD if applicable, UPI active account, evidence never auto-approved, no duplicate payments, proper booking/order transition.
- **Staff workflow:** Need Human, Need Attention, Payment Verification Required as separate flags; Take Over/manual AI OFF suppression; issue resolution and audit.
- **Notifications:** admin recipient quotas/permissions, approved WhatsApp templates, configurable events, payment verification actions, deduplication, no spurious customer AI chats.
- **Follow-up:** single scheduled item, time boundary, no premature/duplicate sends, eligibility and suppression.
- **Security:** RBAC, Platform Admin read-only enforced server side, cross-company isolation, webhook authenticity, unauthorized interactive payloads, race/retry idempotency.
- **Resilience:** API 429/timeouts/empty responses, failed media, no active UPI, no availability, expired plan, stale queued jobs, retries with controlled injection.
- **UI:** chats persist, sidebar Orders/Bookings/Payments match DB, accurate counts, unread and read-only view have no side effects.

## 5. Scoring without inventing results

Suggest clearly labeled **proposed evaluation rubrics** (not observed scores). If relevant, use prior BuyChat rubrics:

| Financial integrity | Max |
| --- | ---: |
| Pricing accuracy | 25 |
| Payment request accuracy | 25 |
| UPI account accuracy | 20 |
| Verification and confirmation safety | 30 |

| Service booking integrity | Max |
| --- | ---: |
| Service selection and requirements | 20 |
| Date/time and availability | 20 |
| Booking creation/data | 25 |
| Lifecycle and status | 20 |
| Sidebar and notifications | 15 |

| Language quality | Max |
| --- | ---: |
| Language detection | 20 |
| Script accuracy | 20 |
| Consistency | 20 |
| Grammar/naturalness | 20 |
| Contextual adaptation | 10 |
| Business terminology | 10 |

For untested criteria, show **N/A/Blocked** and coverage; do not automatically award full points. Any cross-tenant financial leak, unauthorized payment confirmation or fabricated booking confirmation can be **Critical** irrespective of score. Include AI conversation/empathy quality separately when requested.

## 6. Audit report contract

Ask Claude Code to create a report filename reflecting exact phase (e.g., `BUYCHAT_PHASE7_SERVICE_REAL_AI_AUDIT.md`) with:

1. Executive summary and executed vs planned coverage.
2. Company setup and test-run ID.
3. Per-company/persona/language matrix with actual conversation IDs, dev URLs and pass/fail/blocked.
4. Actual transcripts, including native scripts, timestamps and provider trace references.
5. Case-by-case test table: ID, precondition, actions, expected, actual, status, evidence.
6. Order/booking/quote/payment state and notification audit.
7. Applicable verified scoring rubrics and deductions.
8. Defects by severity, reproducible steps, screenshots/log references, suspected cause clearly marked as hypothesis, recommended remediation.
9. Performance/tokens/AI provider costs where actually available; never fabricate.
10. Regression findings, missing features, improvement roadmap, release readiness and explicit blockers.

The prompt must require **executed evidence**, not a pre-filled success report. If execution stops, report partial completion honestly.
