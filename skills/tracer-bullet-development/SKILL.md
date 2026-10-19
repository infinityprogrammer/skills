---
name: tracer-bullet-development
description: Plan and build features as tracer bullets - one thin, real, end-to-end slice through every layer first, then phased expansion. Use when the user asks to plan, spec, scope, or start a new feature, build an MVP or prototype, break work into phases or milestones, asks for a vertical slice, or says "where do I start".
---

# Tracer Bullet Development

Tracer bullets come from *The Pragmatic Programmer*. Instead of building a system layer by layer, you fire one thin slice of real functionality through **every** layer first. It gives immediate feedback on whether the architecture, integrations, and assumptions hold — then you expand outward from working code.

## Core rules

1. **Phase 1 touches every layer with real wiring.** UI → API → logic → storage → external services. Hardcoded values and a single happy path are fine; mocking or skipping a layer is not.
2. **Every phase ends runnable and demoable.** Someone can execute it and see the result.
3. **Grow outward, not downward.** Later phases add capability (more cases, validation, error handling, edge cases, polish) across the stack — never "finish the database first".
4. **Tracer code is kept, not thrown away.** Unlike a prototype, it is production-quality skeleton code that later phases build on. Keep it simple, but write it properly.
5. **Defer explicitly.** Anything not in a phase goes in the Deferred list, so nothing is silently dropped.

## Workflow

### 1. Understand the feature
Identify the goal, the user, and what "done" looks like. If something genuinely blocks planning, ask at most 3 focused questions; otherwise state your assumptions and continue.

### 2. Map the layers
Inspect the codebase and list every layer the feature crosses, using real file and module paths (e.g. `src/app/reports/page.tsx`, `api/routes/reports.py`, `db/migrations/`). In an empty project, choose the minimal stack and list the layers you will create.

### 3. Define the tracer bullet
Pick the **single smallest user action** that passes through all the layers. Test it against this checklist:
- [ ] Touches every layer listed in step 2
- [ ] Uses real wiring between layers (no mocked layers)
- [ ] Is demoable end to end
- [ ] Can be built in roughly a day or less
- [ ] Could be covered by one end-to-end test

If it fails any check, shrink it or re-scope it.

Write down **what the tracer proves** — the specific assumptions it tests (e.g. "the join-table query shape works", "the dev proxy reaches Postgres"). If it proves nothing uncertain, it is aimed at the wrong target.

Shape Phase 1 so later phases **widen it instead of rewriting it**. Stub the expensive parts rather than leaving room for them to be bolted on later: for example, keep a `user_id` column with a hardcoded user instead of building auth now, or create the final table shape even if only one column is used.

### 4. Plan the phases
Usually 3–5 phases after the tracer bullet. Each phase:
- Adds one coherent capability across the stack
- Has concrete acceptance criteria that map to tests, and a short demo (the commands or clicks that show it working)
- Leaves the system runnable

Order phases by risk. Product-defining unknowns (tricky domain logic, integrations, data shape) go first. Well-understood plumbing (auth, admin screens, polish) can come later, as long as Phase 1's shape makes adding it a widening. Say why the order is what it is in one line.

Deployment is a layer too. For new projects, consider deploying the tracer bullet early, or at least list hosting as a risk.

### 5. Write the spec
Save the spec to `specs/<feature-name>.md` (or the project's existing spec/docs folder) using the template in [references/spec-template.md](references/spec-template.md). For large plans (a whole MVP, 5+ phases), split it into a `specs/<feature>/` folder with an overview file plus one file per phase.

### 6. Confirm, then build one phase at a time
Show the user the tracer bullet and the phase list, and confirm before implementing. When building:
- Implement Phase 1 only, then run it and its end-to-end test
- Report what worked and what the tracer revealed (surprises, wrong assumptions)
- Revise remaining phases if needed before moving on

## Anti-patterns

| Don't | Do instead |
|---|---|
| "Phase 1: design the database schema" | Phase 1 creates only the one table/column the tracer needs |
| Mocking the backend so the UI "works" | Call the real endpoint, even if it returns hardcoded data |
| Building all API endpoints before any UI | One endpoint, wired to one UI action |
| A phase that ends in "foundation" no one can run | Every phase ends in something demoable |
| Handling every edge case in Phase 1 | Happy path only; edge cases are a later phase |
| Throwing the tracer code away afterwards | Build on it — it is the skeleton |

## Examples

See [examples/](examples/) for worked specs:
- [web-app-export-report.md](examples/web-app-export-report.md) — full-stack web feature
- [cli-sync-command.md](examples/cli-sync-command.md) — command-line tool
- [data-pipeline-ingest.md](examples/data-pipeline-ingest.md) — data pipeline
