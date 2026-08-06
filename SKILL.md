---
name: screen-handoff-pipeline
description: Use when a design already exists (Figma frame, mockup, or built screen) and developers need a plain-language handoff doc defining every number, field, state, and behaviour on it — with zero code references. Trigger on "write a handoff for this screen", "turn this Figma into a spec", "define what each number means", "developer handoff sheet", "one-pager for this screen", or any multi-session project of documenting screens one at a time. NOT for features that don't exist yet (use feature-design-pipeline) or craft/visual overhauls of existing modules (use module-redesign-pipeline).
---

# Screen Handoff Pipeline

Turn an existing design into a handoff document a developer can build from and a non-technical reader can follow — plain language, no code references, every ambiguity closed before it ships.

## When to use which sibling skill

| Situation | Skill |
|---|---|
| Feature doesn't exist yet; the WHY isn't written down | `feature-design-pipeline` |
| Existing module needs a visual/craft/IA overhaul | `module-redesign-pipeline` |
| **Design exists; developers need to know what every number means** | **this skill** |

---

## Stance — how to behave (read this before the phases)

The phases below are mechanical. The value is in the stance. A run of this pipeline that follows every step and skips the stance produces a tidy document that is quietly wrong.

### Never blindly agree

When the owner proposes something, check it before agreeing. Agreement that costs nothing is worth nothing to them.

- Agreed **because you verified it** → say what you checked.
- Agreed **because their reasoning is better than yours** → say what changed your mind.
- Wrong → say "you're right, I was wrong," plainly, and explain what you got wrong and why. No hedging, no burying it in a paragraph of qualifications.

The owner is testing whether you're a second brain or a mirror. Be the second brain.

### Lead with your recommendation — always

Never ask a bare question. Every question carries your own answer first, with the reasoning, then the question. This applies in plain conversation, not just formal question tools.

> Wrong: "Should Received be duration-scoped or live?"
> Right: "I'd make Received duration-scoped — as an all-time number it answers a question nobody's asking. Agree?"

If you genuinely have no preference, say why the call is theirs. That's still a position.

### Verify before asserting — especially "this is broken"

The most damaging output of this pipeline is a confident, specific, wrong claim. They read exactly like true ones.

Before writing "this is broken / confirmed / still failing":
- Confirm **which** code path actually serves **this** screen. Matching keywords are not proof. A real bug in the wrong module reads identically to a real bug in the right one.
- Confirm the claim is current, not inherited from months-old notes.
- If something is scaffolded-but-not-built, say that — "not built yet" and "broken" are different facts with different consequences.

### Operator-first, not spec-checker

Correctness review asks "does this match the spec." Operator review asks "what would confuse the person using this at 9am." Only the second one finds:

- The same number shown three times under three labels
- A widget whose empty state was copy-pasted from a different module
- A chart with no defined tap behaviour
- Two adjacent lists that look inconsistent but aren't

Run this pass on every screen, unprompted. It is not optional and not a nice-to-have — it is where most real findings come from.

### Be proactive about what you surface

Don't wait to be asked "anything else?" Actively hunt and report:

- Hidden or switched-off layers in the design file — is that deferred, dead, or forgotten?
- Copy that belongs to a different feature
- Naming drift between design, backend, and prior docs
- Numbers that duplicate each other
- States that exist in the file but nobody mentioned (restricted, empty, error, loading)

When something is adjacent but out of scope, name it and move on. Don't fix it silently and don't pretend you didn't see it.

### Spar before you settle

For any non-obvious decision, argue both sides before recommending one. If you can't state the strongest case against your own recommendation, you haven't thought about it enough. Then commit to a side — a survey of options is not a recommendation.

### Say what you don't know

- Read a cropped or partial view? Say so, and cross-check before treating it as fact.
- Inferred something visually rather than confirming it? Mark it as inferred.
- Assumed a default? Flag the assumption rather than burying it.

---

## Phase 0 — Pre-flight (mandatory, cheap, saves the most time)

Run every check before writing anything. Each one exists because skipping it cost a rework.

- [ ] **Read the project tracker first.** Cross-screen rules already decided must be inherited, not re-litigated. Re-asking a settled question is the most annoying failure mode of a multi-session project.
- [ ] **Does prior documentation already exist for this screen?** Search the docs folder before deriving anything. Reconciling an existing hardened doc is a fraction of the work of writing from scratch — and re-deriving it silently contradicts decisions someone already made.
- [ ] **Which backend actually serves this screen?** Verify. Do not assume the module with matching keywords is the right one. Note whether it's real logic or a scaffold returning placeholder data.
- [ ] **Is there an informal source?** A developer's rough sheet, a QA doc, a slide. Often 70% correct and worth reading before starting.
- [ ] **What states exist in the design file beyond the happy path?** Empty, restricted, error, loading, hidden layers. Enumerate them now so they don't surface at the end.

## Phase 1 — Index the design

Get the structure before the detail.

- Pull the frame's node tree. If the dump is large, compress it through a subagent — never read a huge structure dump into the main conversation.
- Produce a compact index: what each frame/section/component is, grouped, with repeated variants collapsed to a count.
- Explicitly list: anything hidden, anything that looks like an annotation or dev note, anything named as a state variant.
- Read any designer notes embedded in the file as text — they often contain the real formulas.

## Phase 2 — Capture the visuals

- Batch screenshot requests for all relevant nodes in one go.
- Download to scratch, then read as images.
- **Cross-check field counts against the node structure, never trust a screenshot alone.** A narrow or cropped render silently drops fields. If a section has a small fixed set of buckets, confirm the count structurally.

## Phase 3 — Verify the logic

- Check each definition against how the system actually behaves today.
- Translate every finding into plain language. The verification happens against code; **no code reference survives into the document.**
- Where design intent and current behaviour disagree, surface it as a decision for the owner — never silently pick one.
- Distinguish clearly: *built and working* / *built and wrong* / *scaffolded, not built* / *doesn't exist*.

## Phase 4 — Draft

Write the document (format below). Flag every ambiguity inline as an open question rather than guessing. A draft with ten honest open questions beats a confident draft with ten quiet assumptions.

## Phase 5 — Operator-first audit

Walk the finished draft as the person who uses this screen daily. Look for:

- Numbers that repeat under different names
- Undefined tap/drill behaviour
- Sections whose relationship to each other isn't stated
- Copy that doesn't belong
- Edge states never mentioned
- Anything that would prompt "wait, which one is right?"

Report findings with a recommendation for each. This pass is mandatory.

## Phase 6 — Grill the open threads

**Use the `grilling` skill for any thread still ambiguous after owner review.** This is the highest-value phase — it converts "probably" into "locked," and ambiguity that survives into a build becomes a bug or a rework.

How to run it well:

- **One question at a time.** Batched questions get batched, shallow answers.
- **Search for real precedent before each question.** How does the system already handle this elsewhere? An answer grounded in existing precedent is usually right and always cheaper than inventing a new model.
- **Lead every question with your recommendation and its reasoning.**
- **Push on edge cases.** "Does this apply to X too? What about the partial case?"
- **Don't accept vague.** "Probably" is not an answer. Ask again, narrower.
- Stop when both sides could write the same sentence and mean the same thing.

## Phase 7 — Close out

- [ ] **Hoist cross-screen rules to the tracker.** Anything learned here that applies to other screens must leave this document and become shared. Otherwise every future screen re-asks it.
- [ ] Update the tracker: screen status, next action, session lineage.
- [ ] Append anything learned to the Learnings section of this skill.
- [ ] Confirm the doc has zero code references, zero jargon requiring your dictionary, and no open items left unmarked.

---

## Document format

One document per screen. Sections in this order, skipping any that don't apply:

| Section | Contents |
|---|---|
| **Copy fixes** (only if wrong copy found) | Table: widget / current wrong copy / recommended title+subtitle / CTA. Put at the very top — it's the most actionable thing in the doc. |
| **Build status** | Is the backend real, scaffolded, or absent? Prevents readers treating "not built" as "broken." |
| **Where this lives** | Navigation path to the screen. |
| **Cross-cutting rules** | Behaviours applying to every section — time filters, drill-down, permissions. State them once, up top. |
| **Live sections** | Each with a definitions table: field → plain meaning. |
| **Time-scoped sections** | Same, noting what window each follows. |
| **Detail sheets / drill-downs** | What opens from where, and what it shows. |
| **Restricted / permission states** | Who sees what, and what's locked together vs independently. |
| **Empty states** | Covered by the copy table if there are issues. |
| **Build guidance** | Numbered instructions for whoever implements — traps to avoid, patterns not to copy, things not to build. |
| **Open items** | Explicitly "none outstanding" when closed. Never leave this ambiguous. |

**Writing rules:**
- Plain words. If a term only resolves for someone who shares your context, replace or gloss it.
- Domain vocabulary the team already owns (tenant, due, booking, vacant) stays — don't over-simplify their own words.
- Zero code references: no file paths, table names, column names, function names, line numbers.
- Tables for parallel content, prose for argument.
- Mark decisions as locked, recommended-but-untested, or open — never blur the three.

---

## Traps (accumulating — add one every time something goes wrong)

1. **A cropped screenshot silently drops fields.** Cross-check bucket/field counts against the structure. *(Dropped an entire bucket from a 4-bucket widget by trusting one narrow render.)*
2. **Stale notes about the wrong module read exactly like current facts.** Verify which code path serves *this* screen before asserting anything is broken. *(Reported a real bug from an older, unrelated module as a live defect on a new screen.)*
3. **Bare questions waste a turn.** Lead with a recommendation every time, in prose as well as in formal question tools.
4. **Ambiguous input — ask, don't invent.** A garbled or unclear instruction is a question, not a puzzle to solve creatively.
5. **Two lists that differ aren't automatically inconsistent.** Check whether they're computed over different slices before flagging a mismatch. *(Flagged a "taxonomy inconsistency" that was actually two correct top-N lists over different data.)*
6. **A design file's hidden layer needs an explicit ruling** — deferred, dead, or forgotten. Don't assume deferred; a duplicate elsewhere usually means dead.

## Learnings (accumulating — split into a separate file if this passes ~30 entries)

- **Prior art check is the highest-ROI pre-flight step.** One screen had three existing hardened documents; the job collapsed from "derive everything" to "reconcile and reformat."
- **Cross-screen rules discovered on screen #1 make screens #2–N faster.** Time-filter behaviour, permission grouping, drill-down rules, empty-state tone, and document format all transfer. Hoist them or lose them.
- **The operator-first audit finds a different class of problem than correctness review** — redundant numbers, wrong copy, undefined interactions. Correctness review finds none of these.
- **Existing system precedent usually beats an invented model.** Reusing permission groupings that already exist avoided inventing a third permission scheme and avoided re-assignment work for everyone already holding the old ones.
- **Informal developer sheets are worth reading.** Rough, undocumented, often ~70% right, and frequently keyed to the same design nodes.

---

## Profile: RentOk Manager App Analytics

Stable facts for this project. Evolving state (decisions, screen status) lives in the tracker, not here.

- **Docs live in:** Obsidian → `RentOk/PRDs/Homescreen Detailed Analytics/`, one folder per screen, `DA-XX` numbering.
- **Tracker:** `00 — Manager App Analytics Tracker.md` in that same folder. **Read it first, every session.**
- **Backend:** a single analytics module serves every screen in this project. As of 2026-08-06 every block is wired and reachable but returns placeholder data — scaffolded, not built. Re-check per screen; don't assume it's still true.
- **An older, separate list-screen module** serves the legacy widgets. It is *not* what these screens are built on. Don't cite its behaviour as fact about the new screens.
- **Informal source:** a developer-maintained spreadsheet with a tab per screen, keyed to design node URLs. Rough but useful.
- **Cadence:** one screen per session. The tracker is the handoff artifact — a fresh session reads it and continues. Full handoff ceremony only for unusual sessions.
- **Team:** backend, fullstack, mobile, QA, PM, CTO. Write for a smart, plain-vocabulary, largely non-native-English audience.
