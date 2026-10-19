# Spec template

Copy this structure into `specs/<feature-name>.md`. Keep each section short — the spec is a plan, not documentation.

```markdown
# <Feature name>

## Goal
One or two sentences: who it's for and what they can do when it's done.

## Assumptions
- Anything you assumed instead of asking (stack, scope, users, data).

## Layers touched
| Layer | Location | Notes |
|---|---|---|
| UI | `path/to/component` | |
| API | `path/to/route` | |
| Logic | `path/to/service` | |
| Storage | `path/to/schema-or-migration` | |
| External | service name | |

## Phase 1 — Tracer bullet
**User action:** the single smallest action that crosses every layer.

**Path:** UI → API → logic → storage → back to UI (describe the concrete flow).

**Shortcuts allowed:** hardcoded values, single happy path, minimal styling.

**What this proves:** the assumptions this slice tests (query shape, integration, dev wiring).

**Demo:** the exact commands or clicks that show it working.

**Acceptance criteria:**
- [ ] <observable result someone can demo>
- [ ] End-to-end test: <what the test does and asserts>

## Phase 2 — <capability>
**Adds:** what this phase adds across the stack.

**Why now:** one line on why this phase comes in this position (risk, dependency).

**Acceptance criteria:**
- [ ] ...
- [ ] Test: ...

## Phase 3 — <capability>
...

## Deferred
- Things explicitly out of scope for now, so nothing is silently dropped.

## Risks and open questions
- Unknowns the tracer bullet should expose early (integrations, performance, permissions).
```

## Checklist before handing the spec over

- [ ] Phase 1 touches every layer in the table
- [ ] No phase is a single layer ("build the database", "build the API")
- [ ] Every phase is demoable and has at least one test
- [ ] Riskiest work comes earliest, and each phase says why it is in that position
- [ ] Phase 1's shape lets later phases widen it rather than rewrite it
- [ ] Deferred list exists, even if short
