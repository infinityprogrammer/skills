# Skills

[![skills.sh](https://skills.sh/b/infinityprogrammer/skills)](https://skills.sh/infinityprogrammer/skills)

Agent skills by RAFI.

## Install

### As a Claude Code plugin

Inside Claude Code, add this repo as a marketplace and install the plugin:

```
/plugin marketplace add infinityprogrammer/skills
/plugin install rf-skills@infinityprogrammer
```

Skills are then available as `rf-skills:<skill-name>`. To get new versions later, run `/plugin marketplace update infinityprogrammer`.

### With skills.sh

```bash
npx skills add infinityprogrammer/skills
```

## Skills

- [tracer-bullet-development](skills/tracer-bullet-development/SKILL.md) — build a tiny end-to-end slice of a feature first, then expand in phases.
- [to-spec-slice](skills/to-spec-slice/SKILL.md) — plan features as tracer-bullet spec slices: a thin end-to-end slice first, then phased expansion.
- [claude-code-requirement-writer](skills/claude-code-requirement-writer/SKILL.md) — turn rough ideas, screenshots and bug reports into copy-ready Claude Code requirements and end-to-end QA test plans.

## Develop locally

```bash
claude --plugin-dir .
claude plugin validate .
```
