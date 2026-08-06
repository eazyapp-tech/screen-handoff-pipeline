# screen-handoff-pipeline

A reusable Claude Code **skill**: turn an existing design (a Figma frame, mockup, or already-built screen) into a plain-language developer handoff document — every number, field, and state defined, **zero code references**.

Built from the RentOk Manager App Analytics project (the Dues screen, DA-01) and generalized.

## What it does

Runs an ordered pipeline: **pre-flight → index the design → capture visuals → verify logic against the real system → draft → operator-first audit → grill open threads → close out.**

Two ideas carry the weight:
- **Never blindly agree.** Every claim gets checked before it's repeated back; every question leads with a recommendation, not a blank menu.
- **The learning loop is the point.** Mistakes become pre-flight checklist items (a `Traps` section); rules that apply beyond one screen get called out to hoist into shared project state. Both sections accumulate — append to them every run.

## Install

```bash
cp -R screen-handoff-pipeline ~/.claude/skills/
```

Restart Claude Code (or start a new session). It appears in the skill list and auto-routes on asks like "write a handoff for this screen", "turn this Figma into a spec", "define what each number means".

## Sibling skills

| Situation | Skill |
|---|---|
| Feature doesn't exist yet | `feature-design-pipeline` |
| Existing module needs a visual/craft overhaul | `module-redesign-pipeline` |
| Design exists, developers need to know what it means | **this skill** |

## Adapting it

The bottom of `SKILL.md` has a project-specific profile (RentOk's Manager App Analytics conventions). Swap it for your own team's paths, backend notes, and terminology — the phases and stance above it are project-agnostic.
