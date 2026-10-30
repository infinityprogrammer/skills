---
name: claude-code-requirement-writer
description: Convert rough feature ideas, screenshots, bug descriptions, change requests, or prior chat decisions into clear, copy-ready Claude Code requirements and implementation prompts. Also create thorough real-AI, end-to-end QA test plans and audit prompts for SaaS applications. Use when the user asks to "rewrite for Claude Code", "write a requirement", "create a change request", "give test cases", or "prepare the next testing phase". Supports BuyChat's established rules when that project is mentioned, without imposing them on other projects.
---

# Claude Code Requirement Writer

Turn informal requests into precise prompts another coding agent can execute, **without executing the work yourself unless separately asked**. Preserve the user's intent and project decisions, while adding the necessary technical, functional, and verification detail.

## 1. Understand and route the request

1. Read the latest user message, relevant prior turns, and any supplied screenshots. Treat screenshots as evidence of observed UI/behavior, not proof of a backend root cause.
2. Identify the requested deliverable:
   - **New feature:** implementation requirement with end-to-end workflow.
   - **Change to existing feature:** current vs expected behavior, compatibility, migration, and regression.
   - **Bug fix:** reproduction, observed vs expected, likely investigation points, fix criteria.
   - **Test / QA phase:** executable test plan with scenario matrix, records, observations, and audit report.
   - **Planning only:** investigation and proposed design, explicitly no implementation.
   - **Short rewrite:** a compact, polished instruction rather than a long specification.
3. Extract project, actors, permissions, triggers, states, negative cases, UI changes, and success definition. Reuse stated details; do not ask for details already available.
4. Follow **the newest explicit requirement** when older decisions conflict. Maintain existing functionality unless the user expressly changes it.
5. For BuyChat tasks, consult [references/buychat-rules.md](references/buychat-rules.md). For test phases and extensive audits, also consult [references/testing-playbook.md](references/testing-playbook.md).

## 2. Output defaults

- Produce a **self-contained, copy-paste-ready Markdown prompt** addressed to Claude Code. Use a clear `# [Project] — [Feature / Phase]` title.
- Write in direct, imperative, actionable language: "Implement", "Validate", "Do not", "Test"; avoid conversational filler and vague wishes.
- Match requested depth. For "short" or "don't expand", use roughly 1–4 concise sections. For a normal change, cover 6–10 meaningful sections. For "complete", "360", or a testing phase, deliver a detailed scenario-driven plan.
- Distinguish **mandatory requirements**, **exceptions**, **recommendations**, and **out-of-scope** items. Do not silently turn optional suggestions into requirements.
- Use tables where they improve comparisons, states, event mappings, or test cases. Do not pad a simple request with boilerplate.
- Preserve meaningful examples in the user's language (including Malayalam/Manglish), with short illustrative expected responses where useful.
- Do not assert that changes have been implemented, tests have run, APIs work, or results exist. The output is a prompt *for Claude Code*.
- Do not invent schemas, API endpoints, provider capabilities, prices, permissions, or existing modules. Instruct Claude Code to inspect and reuse the actual codebase. Flag uncertain functionality as "where supported" or "verify current implementation".
- Include "Do not commit or push unless explicitly authorized" when code modification is requested; don't make it a precondition to running local tests.

## 3. New feature / change request structure

Adapt this template; omit irrelevant sections:

```markdown
# [Project] — [New Feature / Change Request]: [Specific Title]

## Objective
[User goal and measurable result]

## Current Behavior / Problem
[Observed situation; screenshot evidence when present]

## Required Behavior
[Precise rules, trigger, state transitions, examples, exceptions]

## UX and Roles
[Menus, buttons, admin vs staff privileges, visibility]

## Backend and Integration
[Existing services to inspect, events, persistence, queues, security]

## Edge Cases and Safety
[Validation, concurrency, duplicates, failures, tenant isolation]

## Test Scenarios
[Positive, negative, permissions, regression]

## Acceptance Criteria
[Observable, verifiable outcomes]

## Final Instruction to Claude Code
[Review architecture → plan → implement → test; project-specific constraints]
```

**Make business transitions explicit:** differentiate request/pending/reviewed/verified/confirmed/completed/cancelled. Define who is authorized to cause each transition, what side effects happen, and what must *not* happen. For AI-generated messages, distinguish the text from the authoritative backend record.

**For bugs shown in screenshots:** quote only clearly visible behavior; explain why it is wrong; supply expected before/after examples; request root-cause analysis across system prompt, domain rules, UI, persistence, and queue if relevant. Avoid claiming a certain module is at fault without evidence.

## 4. Testing phase structure

Follow [references/testing-playbook.md](references/testing-playbook.md). At minimum specify:

1. **Scope and constraints:** company count, domain mix, number of independent customers, languages, real AI/provider usage, dev environment, safe transport and payments.
2. **Test company matrix:** realistic distinct industries, products or services, goals, assistant instructions, pricing types, plans, and seeded staff.
3. **Customer scenario matrix:** each company/customer/language; conversion, enquiry, negotiation or domain-specific difficult case. Explicit expected workflow, not only labels.
4. **End-to-end execution:** inputs → real AI responses → backend state → UI visibility. Persist all primary chats in the actual development Conversations page with IDs and test-run markers.
5. **Functional and negative test IDs:** pricing, orders/bookings, payment, human attention, follow-ups, voice, language, errors, concurrency, permissions, tenant isolation, notifications.
6. **Evidence and scores:** report factual pass/fail/blocked results; score only executed/evidenced criteria and show coverage; distinguish critical safety failures from aggregate score.
7. **Final report:** transcripts, links/IDs, transaction records, provider metrics, bug reproduction and severity, regression, improvements and readiness.

If a user asks for **testing only**, prohibit code edits, automatic fixes, destructive cleanup, production messages, real-money transfers, and unauthorized commits. The test agent may seed **isolated, tagged dev test records**, which is different from modifying source code. Explicitly preserve test conversations after execution if the user wants to inspect them.

For real-AI tests, do not describe mocks as real inference. Simulated WhatsApp transport, synthetic payment media, and controlled clock/fault injection are acceptable when clearly identified, while actual AI replies must originate from configured providers. Unavailable modules are **Blocked**, not fictitiously passed.

## 5. Requirement quality checks before responding

Review the draft against this checklist:

- Does it faithfully capture all explicit changes and the latest project rules?
- Are triggers, actors, states, exceptions, permissions, and UI outcomes testable?
- Does it avoid contradictory instructions (e.g., badges suppressing AI when they should not)?
- Does it prevent fabricated payment approval, booking confirmation, and cross-tenant data access?
- Are there concrete acceptance criteria and essential regression cases?
- Is the output sized appropriately, free from repeated boilerplate, and ready to paste into Claude Code?
- Does it clearly distinguish **a prompt to do work** from **work already completed**?

Finish with the standalone prompt rather than an offer to do additional work, unless the user requests follow-up options.
