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

### The current system is a snapshot, not a ceiling

Code and production data are reality today, and every claim must be grounded in them — but they are a photograph of a growing thing. The user base grows, feature adoption grows, the data changes daily, and new requirements arrive with every onboarded cohort. So the snapshot **grounds and prioritizes; it never blocks.** A number is not wrong because today's data is thin, and a behaviour is not right because the code already does it. Design from what the operator needs; use the snapshot to size and sequence. When the snapshot and the user's need pull in different directions, that is a grill thread for the owner — not a silent judgement call in either direction.

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
- [ ] **Read the closed sibling sheets, not just the tracker.** Inherit the spine, the section names, and every settled ruling before writing a word. The close-out sibling check (Phase 6.75) only catches what inheritance missed — on the screen that introduced it, three sheets had three names for the Restricted section and two screens ruled opposite ways on the same duplicate-tile question, because each was drafted without reading the others.
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
- **Size every candidate number against production data, in both directions.** A hardened source can specify a case that never occurs — one screen's sources built a cleanup path for a field null on zero of 339,000 rows, and a permanent bucket for a fund used on 0.35% of records. But low is not automatically dead: judge a *new* number as a new feature ("would seeing this change what someone does"), not by behaviour under a product that never showed it — an 83%-missing figure turned out to be the reason to ship the number, not cut it. Record the measured figures in the doc so the reasoning can be re-checked.

## Phase 3.5 — Draw the behaviour grids (before drafting, not after)

Rules in prose are not a grid. One screen stated every time-filter rule correctly, grilled the biggest question on that axis, and still shipped a duplicate tile plus two more gaps that only surfaced when the grid was finally drawn — after close-out. Draw them now, so the draft embeds the answers instead of patching them in later:

- **Time grid:** every number down the side, every filter option across the top, every cell filled. The cells that are not simply "follows" are the work — fixed-window cards, chips with no previous period to compare against, trend exemptions.
- **Drill table:** every tappable thing → exactly what opens — and confirm the destination can actually express that filter. A promised drill onto a filter the list does not have is a number that can never open its own records.
- **Tap matrix, not a bare drill table:** `You tap | What opens | Arriving filtered to | Ready?`, one row per tappable element, grouped by card. The destination varies with the record's state, and the arrival filters are spelled out in plain words. Close it with the list-capability summary: what the lists filter by today, and every missing filter named with the numbers waiting on it.
- **The window-travel rule:** what a tap carries differs by window kind — live taps carry nothing, period taps carry the window and the list shows people as they are today naming any drift on arrival, forward-window taps re-route to where the records are today.
- **The behaviour contracts, one line each:** where Custom stops, what the filter remembers across drills and launches, return-from-drill state and freshness, the comparison basis for Custom windows, the day boundary. Every one of these was once a question a builder had to ask.
- **The zeros router:** one table telling every empty-looking situation apart — never-set-up, onboarding (something confirmed, nothing arrived), the emptied bad zero, the in-window zero, the good-news zero, the not-recorded gap, failure. Good zero versus bad zero is per number, never per screen.
- **Loading / empty / failed walk:** per card, but only for exceptions — the tracker's shared rules transfer wholesale.
- **View-toggle audit** (trap 7's three failure modes) wherever any toggle exists.

## Phase 4 — Draft

**Start from the project's canonical template, never from a blank page or from memory of the format** (RentOk analytics suite: `_meta/_Handoff Sheet Template.md` beside the tracker). Copy it, fill it, delete what the screen does not have. Drafting from memory is how spines drift; three screens drifted before this became a rule. Flag every ambiguity inline as an open question rather than guessing. A draft with ten honest open questions beats a confident draft with ten quiet assumptions.

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
- **"Think it through / you have the tools" is a graded answer, and the grade is failing.** When the owner returns a question instead of answering it, the question was answerable from evidence — code, production data, or the live product. Withdraw it, get the evidence, and come back with a decision to approve rather than a question to answer.
- **Push on edge cases.** "Does this apply to X too? What about the partial case?"
- **Don't accept vague.** "Probably" is not an answer. Ask again, narrower.
- Stop when both sides could write the same sentence and mean the same thing.

## Phase 6.5 — The final audit (mandatory; do not skip because the doc "survived review")

Run this once the doc feels finished. On its first outing it found ~30 real issues in a doc that had already survived eleven versions and five owner-review rounds — more than any single review round had found. The premise: **almost every error that survives review is a claim made from reasoning rather than from looking.** So every angle here re-looks.

Four checks, the first two delegated to sub-agents so they get done properly rather than skimmed:

1. **Design-file verification (sub-agent).** Every visual claim in the doc, re-verified against the actual frames, claim by claim, with a VERIFIED / WRONG / CANNOT-TELL verdict each. Also: "report anything visible in the design the doc never mentions." That second instruction is where half the value comes from.
2. **Source-document sweep (sub-agent).** Three lists: DROPPED (required by a source, absent from the doc), CONTRADICTED (doc and source disagree — mark deliberate vs accidental), STALE (source superseded). Quote sources verbatim. This was the largest angle: dropped items included a live bug the doc's own bugs section missed and an entire non-goals section that had evaporated across rewrites.
3. **Decision ledger (yourself).** List every decision the owner made across the whole project — all of them, from session notes, not memory — and mechanically check each appears in the doc. Catches silent drops during full rewrites.
4. **The doc's own rationale claims (yourself, or a code sub-agent).** Sentences justifying a decision ("because rent is due on the 1st") are claims too, and they're the least-checked text in the doc because they sound like explanation rather than fact. Verify the load-bearing ones against code or data.

Then two sweeps on the text itself:
- **One word, one meaning.** List the doc's load-carrying terms and check each means exactly one thing everywhere. A term meaning two things ("settled" = bill-dealt-with vs money-reached-bank) is a bug even when every individual sentence is true.
- **Section diff.** Compare the final section list against the version before the last full rewrite. Full rewrites silently drop sections; one vanished for three versions before anyone noticed.

**Two cautions:** audit agents are also wrong sometimes — one confidently reported a card sorted that wasn't; re-verify any agent finding that contradicts your own records before accepting it. And findings that supersede the source documents create debt: **mark the losing sections of the sources as superseded**, or the next reader of a doc still titled "source of truth" builds the wrong thing.

## Phase 6.75 — Check the screen against its siblings

**Only possible once a second screen exists, and mandatory from then on.** A handoff sheet can be internally flawless and still contradict the screen next to it. No check inside one document can see this, and users move between these screens in seconds.

Read the already-closed sheets and compare line by line. The axes that have actually caught something:

- **Change indicator direction and colour.** Is this screen a revenue view or a cost view, and does every sibling agree about itself?
- **What a card's own period control does when the global filter changes.** Persist or snap back.
- **Fixed-period cards that duplicate a filter option.** Same situation, same answer.
- **What the view-all sheet does** — drillable rows or read-only, and why.
- **Whether share-of-total is shown**, and whether it is a requirement or a maybe.
- **Permission wording, lock copy, and the section names themselves.**

First run of this, on the third screen of a project, found five disagreements. The worst was an **inverted change indicator on the first screen, signed off as "confirmed in scope"** — green when the bad number rose. It also found that screen still specifying a rule reversed two screens earlier.

**Where the sibling is wrong, fix it and say what changed at the top of its doc**, because someone may already have built from it. **Where it is a judgement call, the owner picks one answer for all screens** — differing is worse than either option.

## Phase 7 — Close out

- [ ] **Hoist cross-screen rules to the tracker.** Anything learned here that applies to other screens must leave this document and become shared. Otherwise every future screen re-asks it.
- [ ] Update the tracker: screen status, next action, session lineage.
- [ ] Append anything learned to the Learnings section of this skill.
- [ ] **Write the cover note** for whoever receives the doc: which sections are theirs, which open decisions are pre-answered with a recommendation, where the superseded sources sit. A self-contained doc still needs a reading map — the alternative is the reader reading everything or nothing.
- [ ] **Re-read the final artifact end to end after the last edit** (trap 11). Audit fixes are edits too; the final read comes after the final change, or the fixes ship their own contradictions.
- [ ] **Run the vocabulary sweep**: grep the sheet for the banned time words (follows, pinned, window-scoped, duration-scoped, period-scoped) and for any kind label outside Live / Time-scoped / Forecast. Mechanical, thirty seconds, and it is the only thing that has ever actually stopped this class of drift.
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
| **What this screen is not** | Non-goals, each with the reason it was ruled out — so nobody rebuilds the argument or adds one by accident. |
| **How this group is put together** | Local design notes only: how the group's own combination logic works, whether that's right for every option in it, any parent/child mismatch, whether the control type (checkbox, radio, multi-select) matches the underlying exclusivity. Not a redesign — a record, so nothing found along the way gets lost. The cross-group version of the same question lives in the tracker, not here; see the "Taxonomy and redesign" note below. |
| **Build guidance** | Numbered instructions for whoever implements — traps to avoid, patterns not to copy, things not to build. |
| **Open items** | Explicitly "none outstanding" when closed. Never leave this ambiguous. |
| **Measured figures appendix** | Every production figure a decision rests on, and what it decided — so the reasoning can be re-checked without re-deriving it. |

Number the sections end to end, and keep the design-only sections (empty-state copy, the design-fix list) separable — the cover note should be able to say "sections 1–11 and 14 are yours, skip 12 and 15" and have that be true.

**Writing rules:**
- Plain words. If a term only resolves for someone who shares your context, replace or gloss it. **The acceptance test, stated by the owner:** a developer or designer reads the sheet alone and builds from it without asking the PM. Check candidate words against the reader's own world, not just a jargon list: "partition" is clean English and still failed, because to a PG operator a partition is a plywood wall dividing a room.
- Domain vocabulary the team already owns (tenant, due, booking, vacant) stays — don't over-simplify their own words.
- Zero code references: no file paths, table names, column names, function names, line numbers. **Internal numeric codes count as code** — say "a bill cleared using the tenant's deposit," never the mode number behind it.
- **And never tell the reader their code is wrong.** No "the existing calculation counts wrongly", no "do not reuse X", no code cited as evidence. Where something today produces a wrong result, write it as **an outcome plus a test**: not *"it counts anyone present at any point as present throughout"* but *"a tenant present three days contributes three days — test: a property that emptied on the 5th and refilled on the 25th reports about a third full"*. A code claim goes stale the day someone edits; an outcome stays true for the life of the screen, is testable by QA, and costs nothing if the code already does it. **"Not built yet" stays** — that is scope, not critique.
- **Every "wrong today" finding closes with "Once fixed:", one plain sentence stating the target, unhedged.** The outcome-plus-test convention above proves it's broken; this states what right looks like, so the reader never has to reconstruct the target by re-reading the problem prose. Collect every "Once fixed" in the group into one small table at the end of the wrong-behaviour section, the same move the Measured Figures appendix already makes for numbers: state each once where it's relevant, then again, once more, all together, so the whole group's target is scannable without reading every bug.
- Tables for parallel content, prose for argument.
- Mark decisions as recommended-but-untested or open where they are — never blur that with what's settled.
- **State what should be, not how you decided it.** No "locked this session," no "checked against the dev sheet," no "an earlier pass assumed," no "independently confirmed by." The reader is building the screen, not auditing your reasoning. Every sentence of process archaeology is a sentence they have to skip.
- **No cross-document references.** Not "per the Build Sheet §4.2," not "the Formula Map says," not "see the Drill Map." If a fact matters, state the fact. A handoff sheet that only resolves when you have four other documents open isn't a handoff sheet.
- **Design-file problems go in side notes, not the body.** The body says what the number means. A short, clearly-marked side note says what's currently missing or mislabelled in the design. Keep them separable — the definitions outlive the Figma bugs. Collect them once more in a single list at the end so design has one place to work from.

---


## Document grammar (locked 2026-08-07)

The sheets are one product. A reader who learned one must be able to read them all without relearning anything. This is a hard rule, the same rank as no-code-references:

- **One spine.** Same section names, same order: Build status, Where this lives, What every number counts, How the screen behaves, the cards, What each number opens, Who can see this, the states, What this screen is not, Build guidance, Open items, Design file fixes, measured figures. A sheet may drop a section that does not apply; it never renames or reorders what it keeps.
- **Same words inside the cells, not just on the headers.** The behaviour grid's cells come from a closed phrase list ("As of today" · "Counted inside the window" · "Each tile keeps its own fixed window; the filter does not change it" · "From today onwards" · "—"), and kind labels are Live / Time-scoped / Forecast only. Banned and grep-swept at close-out: follows, pinned, window-scoped, duration-scoped, period-scoped. Cell text is written the way you would say it aloud to someone new, never compacted into a word only the author understands.
- **Same table schemas, same column headers.** Tap matrix: `You tap | What opens | Arriving filtered to | Ready?`, one row per tappable element, grouped by card; the destination varies with the record's state and the arrival filters are spelled out in plain words. Definitions and View all: `Row | Meaning`. Number kinds: `Live` / `Time-scoped`, the suite's own two words, never a new coinage. Never change a header another sheet already uses.
- **Same fixed phrases.** "Test it:", "View all", "as of today", "from today onwards", "hides at zero", "Couldn't load this", the Restricted copy. Word for word.
- **Tables wherever content is parallel**; prose only for argument. Layer rows lead with the dash marker inside a cell, as Inventory's View all does.
- **Plain punctuation.** No em dashes in prose (the dash survives as empty-cell placeholder, layer marker, and in titles). No AI-tell vocabulary, and none of the banned-jargon list (polarity, denominator, cohort, reconcile and the rest); say it in the team's words.
- **No scavenger hunts.** Keep every label and number the sheets use, F12, D26, S3, §4, item 7. Never leave one bare. Each reference carries a few words of its own meaning where it sits, so the sentence works without a trip somewhere else: "D26 (no-term tenants get their own group)", "§17's no-guesswork rule", "F52's one guard in the caller". Same for a term the glossary defines once and a card uses 200 lines later. Grep-swept at close-out alongside the banned words: `F\d+`, `D\d+`, `S\d+`, `§\d+`, "item N", "see above". A label is a pointer for finding more, never the only carrier of the meaning, because nobody reads a sheet in the order it was written.

---

## Traps (accumulating — add one every time something goes wrong)

1. **A cropped screenshot silently drops fields.** Cross-check bucket/field counts against the structure. *(Dropped an entire bucket from a 4-bucket widget by trusting one narrow render.)*
2. **Stale notes about the wrong module read exactly like current facts.** Verify which code path serves *this* screen before asserting anything is broken. *(Reported a real bug from an older, unrelated module as a live defect on a new screen.)*
3. **Bare questions waste a turn.** Lead with a recommendation every time, in prose as well as in formal question tools.
4. **Ambiguous input — ask, don't invent.** A garbled or unclear instruction is a question, not a puzzle to solve creatively.
5. **Two lists that differ aren't automatically inconsistent.** Check whether they're computed over different slices before flagging a mismatch. *(Flagged a "taxonomy inconsistency" that was actually two correct top-N lists over different data.)*
6. **A design file's hidden layer needs an explicit ruling** — deferred, dead, or forgotten. Don't assume deferred; a duplicate elsewhere usually means dead.
7. **Documenting a view toggle is not the same as auditing it — and there are three failure modes, not one.** If a screen has a toggle (Paid Date / Due Date, Bed / Room, Gross / Net), walk *every* card through *both* sides:
   - **Collapses** — the card splits data by the very dimension the toggle switches, so on one side it only restates the filter back at the operator (one row at 100%, the rest at zero). → Hide it.
   - **Goes dead or redundant** — rows become permanent zeros, or a tile becomes an exact duplicate of its neighbour. → Swap for something that answers the question *that* view is actually good at.
   - **Silently changes meaning** — the dangerous one. The card looks completely fine on both sides: no zeros, no duplicates, nothing visibly wrong. But it now measures a different thing and nothing on screen says so. Sibling cards can also stop agreeing with each other, because one can express the new meaning and another structurally can't.

   The first two announce themselves. The third never does, and no visual scan will catch it — the only method is to write out each card's definition in words on each side and compare the two sentences. **"Unaffected" is a claim that has to be earned per card, not a default for anything that doesn't look broken.** *(Missed all three on one screen. Documented a toggle's mechanics cleanly but missed a collapsed card and four dead tiles — owner caught those. Then claimed every remaining card was unaffected; owner pushed again, and two of them turned out to silently change meaning.)*
8. **A "not locatable" informal source is a question for the owner, not a closed finding.** Searching and reporting "couldn't find it" is honest but incomplete — the owner usually has the link. Ask before drafting, not after publishing. *(A dev sheet declared unfindable turned up instantly when asked for, after the doc was already published, and it changed two decisions.)*
9. **A rationale is a claim.** The sentences that *justify* decisions are the least-checked text in the doc, because they read as explanation rather than assertion. One justified a whole chart mode with "billing lands in one week, rent is due on the 1st" — the billing day turned out to be a per-property setting with a mode that spreads it across the month. Verify load-bearing rationales the same way as any other claim.
10. **One word doing two jobs survives every sentence-level review.** "Settled" meant bill-dealt-with in half a doc and money-reached-the-bank in the other half; every individual sentence was true and the doc was still wrong. Only a deliberate term sweep catches it.
11. **Patch edits create the next round of contradictions.** Audit findings get fixed by targeted edits, and each edit is written against the finding, not against the whole document — so a fix lands cleanly and contradicts unpatched prose three paragraphs away. One doc accumulated seven of these across five waves of fixes: an empty-state string reused from a source doc that re-imported a rule already overridden, a paragraph denying the existence of a row another fix had added above it, two scripted-edit mechanical breaks (a split table, a duplicated list number). **The final read happens after the last edit, not after the last audit.** Re-read end to end once everything has landed, checking list numbering, table integrity, and every paragraph adjacent to a patch.
12. **Your own sections are not safe during rewrites.** A full rewrite dropped an entire section (Copy Fixes) and three subsequent versions shipped without it. Diff section lists across any full rewrite.
13. **Sub-agent audit findings need the same verification as your own claims.** An auditor confidently reported a card sorted highest-to-lowest; the frame shows it isn't. When an agent contradicts something you personally verified, re-look before conceding — the point of the audit is looking, not deference in either direction.
14. **Reading a prior doc once, early, is not reading it.** Prior documentation gets skimmed in Phase 0 for orientation, then never re-read — so whatever didn't seem relevant on first pass stays lost. Re-read the definitions doc *after* drafting, line by line, against the draft. *(A hardened formula doc specified a five-part card; the design drew four; the draft documented four. The missing part — what's still unpaid — was half the card's value, and it sat in a doc that had already been read.)*
15. **Drafting from the design file inherits the design file's omissions.** If a section exists in the definitions but not in the mockup, working from the mockup silently drops it and nothing flags the gap. Walk the definitions doc's section list against the draft's section list, both directions, before closing out.

16. **A card can be a duplicate of a screen that already exists.** A fully designed analytics card was about to be specced onto a screen before anyone checked whether the product already had that surface. It did, as a live screen with actions a read-only card can never offer, and the card's one useful next step was banned on a diagnosis screen anyway. *Check whether the product already owns the subject before specifying a card for it.* Where half a card feels wrong for the screen, usually the whole card is. The fix is one number that drills into the real screen. **Second occurrence, one screen later:** a "settlements list to build" was recommended before checking whether the product owned settlement tracking. It did (the FlexiPe screen), and the owner caught it, not the check, because the check was never run for *drill destinations*, only for cards. The trap covers both: any time the spec is about to name a surface as new, whether a card on this screen or a destination on another.
17. **Measuring a candidate number against production is necessary and not sufficient.** Querying live data correctly killed three specced numbers that described cases occurring zero, twenty-one and 0.35 percent of the time. The same query said 83% of expenses had no bill, and the number was cut as meaningless. That was wrong: nothing had ever shown anyone that figure, which was *why* it was 83%. **Current data measures behaviour under a product that never asked.** A new number's test is "would seeing this change what someone does", not "is this common today". Low counts can still be dead cases, but that has to be argued, not assumed from the count.
18. **A filter option that is also a fixed card produces a duplicate nobody looks for.** Where a screen offers a period on its global filter *and* carries a card pinned to that same period, selecting it puts two identical numbers side by side. This is the known "tile becomes a duplicate of its neighbour" failure arriving through the filter rather than through a view toggle, so a toggle audit will not find it. Check every fixed-period card against every filter option. **Then check how the sibling screens ruled on it** — on one project two adjacent screens reached opposite answers, months apart, and neither knew.
19. **A named destination is not a reachable one.** Three whole families of drill were specced onto a list that has no such filter, so the numbers could never open their own records. Enumerate the destination's real filters and match them against the drill list one by one. "The list supports filtering" is not the check; "the list supports *this* filter" is.
20. **Info icons are a content deliverable and nobody owns them.** Every card carried one, on filled and empty states alike, and no screen in the project had written a single word of what they say. An icon that opens nothing is worse than no icon.
21. **The one-word-one-meaning sweep has to run across screens, not just within a document.** A word can be correct in two screens and mean two different things, with a manager moving between them in seconds. A within-document sweep passes both.
22. **Pay the superseded-sources debt during close-out, not as an open item.** One screen logged it as an open item and it was never paid; its sources still call themselves the source of truth. Writing the banner takes minutes and is the difference between a correction landing and a correction being overwritten by whoever reads the old doc next.

23. **The first matching artifact is not the governing artifact.** Asked whether expense payment modes had labels, a search found *a* mode map — the collection-side one — and produced a confident, wrong conclusion ("no expense-side list exists, the null default is mislabelled"). The real map was one trace away, reachable from the path that actually writes the value. When claiming something does not exist, trace from where the system writes or reads it, not from the first keyword hit — and treat the owner's "it exists, check properly" as evidence, because it usually is.


24. **A config value is not a constraint.** A nav config declaring a screen "live only" was read as the screen's shape, and used to recommend against the time dimension that was the screen's entire reason to exist. A config line is the easiest thing in the system to change and the easiest to mistake for a fixed fact. Same session, same shape twice: a 1% adoption figure was read as permanent when a full migration to it was already planned. **Before recommending against something because the system does not do it today, ask what it would take to change and who is already changing it.**
25. **A recommendation that names a table, a field or a lookup has stopped being a requirement.** Corrected by the owner mid-session: *"Why are you getting into how it should be calculated? Let the engineers decide. We only want what we want."* Say what the user must see. Engineering picks the shape. This is easy to violate precisely when the grounding work has gone well, because the mechanism is fresh in mind and feels like part of the answer.
26. **When the drill check fails, ask whether the fact is recorded at all.** The established check — enumerate the destination's real filters, match one by one — assumes the data exists and a filter is missing. On one screen the two most valuable numbers had no destination because **nothing recorded the underlying event**. That is a build prerequisite blocking whole cards, not a filter to add, and it only surfaced because the check ran before the drill table was drafted rather than after.
27. **An empty card is not always bad news.** Every screen in one suite treated empty as a gap until a screen arrived where empty meant success — no vacant rooms means the property is full. Its copy congratulated a full property on having no data. **Ask per screen whether zero is good or bad**, and where it is good, that is a distinct state from "nothing here yet", not a wording tweak.
28. **A screen-wide rule can be a per-number property on the next screen.** Change-chip polarity was settled once per screen for three screens running ("up is bad here"). The fourth needed three directions in one row, including a neutral state the shared component did not have. When a screen breaks a settled rule's *shape* rather than its value, that is component work, and saying so is part of the finding.

29. **Saying what is broken in the code is the same mistake as saying how to compute it.** Both hand the reader a mechanism instead of a requirement, and both go stale the moment someone edits. One sheet accumulated seven of these — "do not reuse the existing calculation", "do not copy the live percentage", "the existing count wrongly sweeps in bookings" — each written in good faith to stop a real bug being inherited. **The fix is the same every time: state the outcome, add a test.** The bug still gets caught, by a criterion QA can run, and the claim survives the next refactor. The owner named this after the requirements-not-derivations correction, and it is the same lens applied to defects rather than to formulas.

30. **A state describing the future is a layer on a present state, not a peer of it.** A "booked" slice was specced beside "occupied" and "vacant" — and it double-counted every bed where a replacement sat behind a leaving tenant, broke the donut's arithmetic, and made the vacancy drill unreachable, because the destination list could not express "vacant but not booked". The owner's correction was one sentence: a booking is a promise, not a person, so a booked-empty bed is *still vacant* — booked is a layer inside it. Three separate defects dissolved at once. **When a candidate state answers "what will happen" rather than "what is true now", make it a layer and check what the totals do.**

31. **A text search with a size window is trap 1 wearing different clothes.** Trap 1 is about cropped
    screenshots dropping fields; the same loss happens when structured content is read with a pattern that
    has a fixed character limit. Two switches whose entries carried one extra block ran past the window and
    vanished with no error — and a production report shipped analysing 81 of 83 items. Both missing ones
    turned out to belong in almost every recommendation, and for three cases they *reversed* the finding.
    **Read structured content by matching brackets, never by a size window, and cross-check the count
    against an independent source** — here the stored-field list would have caught it in one line.

32. **"No design file" is not "no design."** This pipeline was ruled out twice for a feature with no Figma,
    from its own one-line description. But the skill accepts a *built screen*, and the feature had two —
    live on web and mobile. The sheet that resulted found five copy errors, two platform divergences on
    identical data, and an access that is enforced but cannot be granted anywhere. **Before ruling this
    pipeline out, ask whether the surface the work touches already ships.**

31. **The sheet absorbs the session unless it is rewritten, not patched.** Across ~30 grill-round patches, one sheet accreted the reasoning behind every ruling: 54 warning blocks, 81 platform statistics woven into card definitions, coined vocabulary ("today number") where the suite already owned the word (Live), a renamed concept one tab apart from its sibling's name for the same people, and a section type no sibling has. Every patch was locally correct; the drift ran two screens before the owner caught it, and none of the audit angles measures density or voice. The fix is a full rewrite to the leanest sibling's shape, then re-running the decision ledger against the new text, because full rewrites drop rulings. Budget from that rewrite: warnings under ten, platform numbers only in the measured-figures appendix, reasoning lives in the tracker, definitions in one or two lines.

33. **Two entry points that share one underlying mechanism read as two separate findings if each is traced
    in isolation.** A default-select-all behaviour in a shared property picker was first written up as a
    risk specific to one entry point (a dedicated "copy" button), then separately as a risk specific to a
    different entry point (an ordinary "save" button) on the sibling sheet — before the sibling check
    noticed both write-ups described the same widget, fed the same list, from two different buttons. Written
    up twice, the two sheets gave conflicting scope for what looked like one behaviour and read as a
    contradiction. Written up once with both entry points named, it was one fact. **When a newly-found
    default or behaviour sounds like something already documented under a different name, trace whether it
    is the same root mechanism before writing it up as new or as scoped to just the screen in front of you.**

34. **A "the write only touches X" claim needs checking against what X defaults to, not just against the
    write logic in isolation.** A draft claimed editing a field during a multi-target copy doesn't change
    the source, because the write only reaches targets named in a list — true of the write, and still
    misleading, because that same list defaulted to including the source itself, pre-selected. A code
    verification pass caught it; a narrower claim checked only against the write function would not have,
    because the write logic and the default-selection logic lived in different files entirely. Same family:
    a positional match between two lists that can be shaped differently (built by conditionally appending
    items per record) is a write-correctness risk, not just a missing-item risk, if the diff matches by
    position rather than by name — confirm which one it does before trusting it, per any two lists whose
    composition can vary. **Check a narrow mechanism claim against the default state or list shape that
    determines whether it's actually true in practice, not just against the isolated function you traced.**

35. **A composite option contradicts the components of its own inputs, not just its own named
    opposite.** A filter built as "A verified AND B signed" was checked for a contradiction against its own
    opposite label and passed. It was never checked against the *opposite of A* or *the opposite of B*
    individually, and it silently contradicts both — nothing on screen says so, and the result looks
    identical to a real zero-match. Drawing the full grid (every filter against every other filter, not just
    labeled pairs) surfaced it before drafting; a live check on the built screen then confirmed the same
    empty result with no explanation. **Any filter defined as the AND of two or more other filters must be
    checked against the named opposite of every one of its inputs, not only against a same-sounding label of
    its own.**

36. **Cross-document references leak from your own drafting, not only from source material.** The
    no-cross-document-references rule is usually framed as a defence against citing an engineering doc or a
    prior brief. It also catches a sibling sheet in the same project — two sentences in a fresh sheet named
    "the KYC Status sheet" while stating a fact that belonged in this sheet on its own. Neither was wrong,
    both were style violations that would have broken if the sibling sheet were ever renamed, split, or read
    on its own. Caught only by the sibling check (Phase 6.75), not by any earlier pass, because a
    single-document read never notices a sentence pointing outward. **When running the sibling check, grep
    your own draft for the other sheet's title, not only for contradictions in content.**

37. **A "has something saved" check is not a "has real content" check, and the gap between them can
    swallow almost the whole passing set.** A completeness filter counted a field as filled in the moment
    anything was saved against it, including an empty string typed and saved with nothing in it. Of the
    tenants marked complete under this rule, only 4% actually had non-blank answers in every field checked;
    the other 96% passed on records that were empty strings, not real data. This is a different failure from
    the already-known "checks the wrong list of fields" bug (trap-worthy on its own) — a screen can fix which
    fields it checks and still pass this trap, because the flaw is in what counts as an answer, not in which
    questions get asked. **Whenever a filter's definition is "field is not empty/not null," check what
    fraction of the passing set holds real content versus a saved blank — the two numbers are not close by
    default.**

36. **A declared default is not the live default.** A verification agent read the stored shape and reported
    that three accesses are on for a newly created record. Measuring records actually created in the last
    thirty days gave **two**: the third was on for 1 record in 538. Where a schema is not applied to the live
    system automatically, the declared default and the real one drift silently, and the declared one reads
    like a fact. **Measure the outcome on real recent records; never read a setting and call it behaviour.**
    The same query also proved the remaining two, which turned a "the code says" claim into a "here is the
    population" claim.

37. **A finding sent to engineering can be wrong, and fixing the document does not fix the flag.** Two
    findings were sent the same day they were found, per the urgency rule. The later audit narrowed both:
    one had missed a second trigger path, so a fix following it would not have closed the problem; the other
    described a display override as a real grant of access, because the claim was built from where the value
    is *shown* rather than where it is *enforced*. Both were withdrawn and re-sent. **Something urgent enough
    to send the same day is urgent enough to correct the same day, at the source.** Send early anyway — the
    alternative is a docs repo holding a production risk — but re-verify every flag against the audit and
    replace it, rather than only correcting the sheet.

38. **Two counts of the same thing on the same date can both be right.** Two sheets recorded 71,080 and
    71,105 for one figure, hours apart, and an audit reported it as a counting failure. The table is written
    to continuously. **Before treating a moving number as an error, ask whether the thing being counted
    changes during the day** — then say when the figure was taken instead of reconciling it.

32. **Post-close owner questions expose missing contracts, not missing content.** After a sheet survived every audit, four consecutive owner questions each found the same species of gap: the tap contract (what opens, filtered how), the window contract (what travels with a tap), the list-capability contract (which filters exist, which must be built), and the zero contract (which empty state is which). None of the audit angles caught them, because audits verify claims that exist; a contract that was never written produces no claim to check. The fix is the Phase 3.5 checklist above: contracts are enumerated up front, not discovered by question. Corollary from the same stretch: **read the verification output before committing** — twice an edit was pushed and only then found to have missed its anchor.

39. **A confirmed bug on one screen makes the same-shaped read on the next screen more convincing, not less risky.** A prior sheet's headline finding was two labels whose logic ran backwards — inverted enum names sitting right beside the query they governed. The next filter group's constants file showed the exact same shape: inverted names, inverted labels, right next to the WHERE clause. It read as confirmation before it was traced, and would have shipped as a second confirmed swap. Tracing the actual frontend consumers (not the constants file) and a live property check overturned it: the labels and the logic agreed everywhere that mattered, the inversion was in a dead, unused constant nothing imports. **The more a new read resembles a bug you already confirmed elsewhere, the more it deserves the full trace, not less** — pattern recognition is doing the opposite of its job when the pattern is "I've seen this shape before."

40. **A yes/no filter pair's two options can silently fail to cover everyone, and no amount of reading the code shows it.** Both options tested a nullable field for exact equality against `true` and `false`; a tenant whose value was never set matched neither and vanished from both, invisible until measured. **For every yes/no filter pair, add a production check that the two options' counts sum to the group's total** — a gap there is a third state hiding in plain sight, and it can be large: here it was roughly 1 in 10 active tenants, sitewide, and not shrinking.

42. **A summary widget and the results list underneath it can silently use different scopes, and a sum-based test built on both will fail even when nothing is wrong.** A screen showed one property's name and stat cards at the top, and a filtered results list underneath that looked like it belonged to the same property. It didn't: the stat cards were scoped to that one property, the results list searched everywhere the account could see and grouped rows by property below the fold. A "tick this filter, tick that filter, add the two counts, compare to the stat-card total" test, already published, failed this exact way, not because the filter was broken but because the two numbers being compared were never measuring the same population. Caught by a clean coincidence (a filtered count matched a wider Metabase scope exactly, not the narrow one expected) rather than by inspection — nothing about reading the screen suggested the mismatch existed. **Before writing any test that sums or compares counts across a summary widget and a results list, confirm both use the same scope**, and prefer a test that stays entirely within one side (compare a filtered count to the unfiltered count on the same list) over one that reaches across to a different widget.

41. **"Should this be redesigned" is a different question from "what does this mean today," and answering it early, one group at a time, produces an answer you'll have to redo.** Three groups into a 13-group project, the owner asked whether the groups should be regrouped, which options should be exclusive versus combinable, what the parent/child structure should be. Two groups in, a cross-group coupling (one switch shown under two different group names) had already surfaced — proof that a taxonomy call made after group 1 alone would have missed it, and a call made after group 3 will likely miss whatever groups 4 through 13 turn up. The fix that keeps both goals alive: document per group exactly as before, but every sheet also gets a short, local "how this group is put together" note, and everything cross-group gets parked in one running tracker table, undecided, until enough groups exist that new couplings stop appearing on every new one. Only then does the full redesign pass happen, as its own deliverable — the same shape a per-group redesign brief already takes for the one group complex enough to need it going in, scaled to cover how all the groups relate. Answering "what's broken" and "what should the whole thing look like" in the same breath, per group, is how you get a taxonomy decided on incomplete information and then have to walk it back.

42. **Two sections written minutes apart, both true in isolation, can say opposite things about the same
    mechanism.** One section correctly described a bulk "clear everything" action as also clearing a set of
    person-level switches; a later section, drafted the same session, described the more common single-item
    version of "remove" as leaving those same switches untouched, and generalized that to the bulk action
    too, because both actions share the word "remove." Neither sentence was wrong about the action it meant.
    The document was still self-contradicting, and the final read caught it, not the drafting. **When two
    actions share a name but not a mechanism, name the mechanism every time either one is mentioned again,
    not just the first time.**
43. **A decision that has been locked but not shipped is a different fact from a decision that has landed,
    and writing "this screen follows decision N automatically" collapses the two.** A sheet described its own
    defaults as already tracking a sibling sheet's not-yet-built decision, because both write through the
    same code path today. That is true of the code path and says nothing about whether the decision has
    shipped, and the measured figures in the same document showed the old, undecided default still live.
    This is the declared-default trap's mirror image: instead of reading a setting and calling it behaviour,
    it read a plan and called it behaviour. State what is true today, and name the decision that will change
    it without asserting the change has already propagated.
44. **When a second finding shares a mechanism with a prior one, match the prior finding's own account of
    itself, not just the mechanism.** A prior sheet was explicit that its own finding and an earlier sheet's
    finding were one gap, not two, because they ran through the same code. A later sheet found a third,
    separately coded instance of the same class of gap and described it as "two earlier findings, this is a
    third," arithmetically defensible, but restating history the earlier sheet had already settled as one
    finding. A reader taking the count at face value would think engineering owes three fixes where the
    documents actually describe two: one shared fix already flagged, and one new fix for a route that does
    not share code with it. Say what the prior sheet said about itself before recounting it.

39. **A grammar that locks headers but not cells regrows jargon one layer down.** Section names and column headers were locked; the words inside grid cells were not, and "Follows", "pinned window" and a third kind label all appeared on one sheet, each individually reasonable, jointly a private dialect — on the very axis where the rule "never a new coinage" already existed. Recall does not hold a vocabulary; a closed phrase list plus a grep at close-out does. When locking any format, ask what the next unlocked layer inside it is, because that is where the drift moves.

45. **A proxy for "nobody has touched this yet" is validated against one population, and silently invalid on
    the next.** On one screen, filtering records to "sign-in still off" cleanly isolated untouched rows,
    because the person was brand new and nobody had set anything up. The sibling screen reused the same
    filter for records belonging to people who *already existed* and were already active elsewhere, where it
    means almost nothing: the person is live at other properties, and the screen itself opens the editor
    immediately after creating the record, so most rows are edited within seconds. The query ran, returned
    plausible numbers, and those numbers appeared to contradict a correct code-level finding. **Before
    reusing a freshness proxy on a new population, state what it is standing in for and check that still
    holds** — and where it doesn't, say the measurement cannot be made rather than reporting the muddy
    number. A write-time claim verified from code is legitimate on its own; it does not need a steady-state
    measurement to prop it up, and pairing it with one that measures something else is worse than leaving it
    unmeasured.

40. **A scripted find-and-replace pass is not a rewrite, and it needs its own read.** Thirty-six edits went in across two turns via find-and-replace, each individually correct, and greps afterwards came back clean. But a grep proves a banned word is absent; it cannot see that one idea has been given five different phrasings. Replacing "partition" in five places produced five wordings for one concept, on the very axis where a one-word-one-meaning rule had just been locked. **After any scripted pass, read the changed sentences side by side, not just the sweep output.** `grep -n` on the replacement phrase, all hits printed together, is enough to see it.

41. **Sweep every deliverable in the folder, not the main artifact alone.** The banned-word sweep ran on the handoff sheet and passed; the cover notes sitting beside it still carried a banned word, because they were written before the rule and never re-checked. The reader receives the folder, not the file. Glob the folder at close-out.

42. **The confident wrong sentence describes the endpoint the screen *looks like* it should call, not the
    one it does.** One sheet's audit found four substantive errors and every one had this exact shape: a
    verification flow written up as recording nothing, when the screen calls a different path that does
    record and displays the result on that very page; an activity list described as showing what was done
    *to* a person when it shows what they *did*; a deletion said to clear sign-in sessions when it clears
    push-notification registrations; and that same deletion said to leave a log entry against the deleted
    person when the entry names whoever performed it. None came from careless reading. Each came from
    tracing the plausible mechanism instead of the actual one, then writing with the confidence the trace
    earned. **For any claim about what a screen records, sends or leaves behind, follow the call the screen
    actually makes before writing the sentence** — and treat a dead or unreachable endpoint that matches
    your expectation as the trap it is, because finding one feels like confirmation.

43. **A corpus-wide punctuation sweep fragments any phrase that appears in more than one file, and the
    fragmentation is invisible in the diff.** Stripping 732 em dashes across twenty documents was
    content-safe by every mechanical check: zero words, numbers, links or identifiers changed. But one
    quoted product label appearing in eight files came out the other side in five different wordings,
    because different editors resolved the same dash to a comma, a colon, or a deletion. Each choice was
    locally right; collectively they invented four new variants of a string the product will ship. **After
    any sweep, grep every phrase that appears in more than one file and confirm it still has one form.**
    The sweep also exposed a three-way split in that label which pre-dated it, so the check earns its keep
    twice: it catches what the sweep broke and what it merely revealed.

44. **An established population phrase can silently drift wider than the screen it describes, and nothing
    catches it except checking the screen again.** An early sheet correctly ruled a not-yet-moved-in booking
    out of a tenant list's non-goals section, in words. Four sheets later, the project's own measured-figures
    convention had grown to "active tenants and bookings," and every session since had queried that broader
    population without re-deriving it, because it read as an established, safe default. It was never
    re-checked against the live screen. It took searching for a specific booking's name, with zero filters
    applied, and getting no results, to notice the drift — a fact already sitting in a sibling sheet's own
    non-goals table did not stop the phrase from spreading past it. The numeric damage was small (the
    excluded population was about 1% of the combined one, never enough to flip a finding's direction), but
    the phrase itself was flatly false: that population cannot appear on the screen, under any filter, full
    stop. **A phrase that has become "how we always say it" is not exempt from verification — it is the
    thing most likely to have drifted, because nobody re-checks what feels settled.** When a user gate asks
    "are you satisfied," treat it as permission to re-open something that already shipped, not just a
    prompt to restate confidence in it.

31. **A rewrite can resurrect a fixed defect, not just drop a ruling.** The decision ledger exists for drops; on the Inventory uplift it also caught the rewrite reinstating behaviour (forward cards narrowing on the forward setting) that a v1 audit round had explicitly removed. The author writes the rewrite from their sense of the design, and their sense includes the pre-fix version. **Give the ledger agent the audit-round corrections as first-class rulings, not just the final text.**
32. **`open(f,'w').write(fn_that_reads(f))` truncates the file before the function reads it.** Python evaluates the `open` first; the function reads back zero bytes, crashes, and leaves the file empty. This zeroed a closed sibling sheet mid-session. Write scripted edits as read-modify-write with the write LAST on its own line — and know where the backup is before running: the repo mirror restored the sheet in one copy.
33. **A sibling's sheet can fix your open item for you.** An Inventory open item tracked a wrong toggle description on the Expense sheet; Expense's own uplift had already corrected it, and only the sibling check noticed. Before carrying an open item about another sheet forward, re-read that sheet — the suite moves between your sessions.

46. **A rewrite can resurrect fixed defects, not only drop rulings.** The decision ledger exists because full rewrites drop things. On one uplift it caught the opposite: the rewriter reintroduced forward-card behaviour that a v1 audit round had explicitly removed, because the rewriter's memory of the card predated the fix. **The ledger must check both directions: v1 rulings present in v2, and v1's *corrections* not undone in v2.** Anything an audit round changed is exactly what a rewrite from memory will change back.
47. **`open(f,'w').write(fn_that_reads(f))` truncates the file before the function reads it.** Python evaluates the `open` first. This zeroed a closed sibling sheet mid-session; it was restored from the repo clone within a minute. Two rules: never nest a same-file read inside a write call's arguments, and **the repo mirror is also the backup** — sync it at every close, not only at the end.

## Learnings (accumulating — split into a separate file if this passes ~30 entries)

- **Prior art check is the highest-ROI pre-flight step.** One screen had three existing hardened documents; the job collapsed from "derive everything" to "reconcile and reformat."
- **Cross-screen rules discovered on screen #1 make screens #2–N faster.** Time-filter behaviour, permission grouping, drill-down rules, empty-state tone, and document format all transfer. Hoist them or lose them.
- **The operator-first audit finds a different class of problem than correctness review** — redundant numbers, wrong copy, undefined interactions. Correctness review finds none of these.
- **Existing system precedent usually beats an invented model.** Reusing permission groupings that already exist avoided inventing a third permission scheme and avoided re-assignment work for everyone already holding the old ones.
- **Informal developer sheets are worth reading.** Rough, undocumented, often ~70% right, and frequently keyed to the same design nodes. Better than that reputation suggests, in fact: on one screen the dev sheet independently confirmed five decisions and overturned two — but only because it was read at all. Its value collapses if read late; read it in Phase 0, beside the hardened docs.
- **Where the informal source and the hardened doc disagree, the informal one is usually loose wording, not a different decision.** Three conflicts on one screen all resolved as shorthand ("Today" meaning no-date-limit, "remaining" meaning collected, "paid invoices" meaning received payments). Check whether the two readings would produce a different *build* before treating a conflict as real — but never resolve it silently either way.
- **Interactive controls hide more work than static sections do.** A toggle, tab set, or mode switch multiplies the screen: every card now has to be correct in N states, and the pipeline's phases naturally walk the default state only. Budget an explicit pass per control.
- **Two owner pushbacks in one session, both right, both reversing a recommendation.** One caught a card being specced without checking whether the product already had that screen. One caught a number being cut on measurement alone. The pattern in both: a confident recommendation made from one source of evidence, where a second cheap check would have flipped it. The stance section says verify before asserting; these were verified against the *wrong* thing.
- **The final audit found roughly forty issues in a document that had already survived a deliberate term sweep and an operator-first pass.** Same result as its first outing. It is not correlated with how careful the drafting was, because it looks at a different thing: claims made from reasoning rather than from looking.
- **A screen can be internally perfect and still contradict its neighbour.** Every check in this pipeline before Phase 6.75 looks inside one document or between that document and its own sources. None of them can see that the screen one tab over answers the same question differently. On its first run the sibling check found an inverted change chip that had survived a full close-out, five review rounds and a final audit on that screen, because nothing had ever compared it to anything.
- **A finding can be genuinely new and still already decided elsewhere.** The fixed-period duplicate was found by a fresh audit, ruled on confidently, and turned out to have been ruled the opposite way on a sibling screen months earlier. Neither ruling was wrong; having both was.
- **Sub-agent audits mis-frame spec-versus-drawn.** A design auditor reported the document "wrong" for specifying four tiles where the file draws five. The document was the specification, not a description of the frame. But it was right that the removal list did not account for all five. **When an audit calls a spec wrong for differing from the design, the finding is usually about the change list, not the spec.**
- **Being asked "what happens in state X?" is a signal the audit was too shallow, not a request for reassurance.** The right response is to walk every component through state X and report what breaks — not to answer only for the component named in the question. The one card the owner asked about turned out to be the smaller half of the problem.

- **Two design versions in one file is not one live and one dead.** The newer version was named "done" and was the correct build target — and the older one was still a live requirements source, because five things had been *lost* between them rather than cut. The tell was cheap and specific: **the newer version's empty states still carried the older version's card titles**, and included an empty state for a card the newer version did not have. Drift leaves fingerprints in the parts nobody re-reads. When two versions exist, diff them and ask which differences were decided.
- **Run the pre-flight fan-out in parallel and it costs one round trip.** Tracker, sibling sheets, prior art, backend state, informal source and design index all went out at once. Everything after that was grounded from the first message rather than discovered late — and two of those agents independently corrected facts that the prior documentation asserted confidently.
- **Confirmed absence and failed search read identically unless you make them different.** The informal dev sheet had no tab for this screen. Rather than reporting "could not find it", the check enumerated the workbook's own internal sheet list and cross-validated it against a known tab id. That turns a soft finding into a hard one — and the pipeline already carries a trap about exactly this failure going the other way.
- **The sibling check found a self-contradiction created by my own patch edit an hour earlier.** A rule was corrected in the body and left uncorrected in the design-fix list, where the designer would actually read it. Patch edits do not just contradict old prose, they contradict *the other place the same rule lives*. Grep every restatement of a rule when changing it, not just the definition.
- **The audits are not redundant with each other, and the split is real.** The sibling check found 16 issues, the source sweep 36 dropped items and 16 contradictions, the operator pass 4, the decision ledger 3, the term sweep 2. Almost no overlap. Each looks at a different thing; dropping any one of them loses its whole category.

- **The strongest argument for the no-code-critique rule is self-protection, not politeness.** Stating a requirement and being told "we already do that" costs nothing. Stating "your code is broken" and having misread it sends someone to rewrite working code — and in the same session that produced this rule, a source document's confident code claim ("this needs a new table and a nightly job") had already been repeated as fact before a code check overturned it. Write what must be true; let the reader discover whether their code already does it.

- **The owner's simplifications repeatedly beat my elaborations.** Three times in one session a rule I had built up with exceptions and caveats was replaced by a shorter one that covered more: booked-is-a-layer replaced a slice plus two overlap rules; "if the tenant were taking the whole flat they would be recorded against the whole flat" replaced my three-branch unit test; and "state the outcome, add a test" replaced seven separate warnings about existing code. **When a rule needs exceptions to survive contact with edge cases, the rule is usually wrong — take it to the owner rather than adding the exception.**

- **Density is a measurable sibling-check axis.** Words, warning count, statistics-in-body, bold count, long paragraphs: one script over the five sheets showed the drift instantly (54 warns vs the exemplar's 4) and then proved the rewrite landed leaner than the exemplar. Run it at close-out; feelings about density argue, numbers settle.
- **The owner's writing bans are hard rules, not style preferences: no em dashes, no AI-tell phrasing.** Plain commas, colons and full stops. The dash survives only as an empty-table-cell placeholder and in document titles. A mechanical sweep leaves comma splices where the dash was doing real work, so sweep, then read the swept lines and repair by hand.

- **A widget's read on the built screen and its code path are two different lies that can point the same wrong direction.** A "Joining Requests" count read zero on every property, and the filter meant to surface those same tenants also returned nothing when selected. They looked like one bug from the outside. They were two: an unrelated hardcoded placeholder behind the count, and a type mismatch (the filter code sent as text, checked against a number) behind the filter. Confirming both independently in source before writing either up avoided presenting a coincidence as a single root cause.
- **The second sheet in a project is where the sibling check earns its keep, and it can catch the drafting session's own slips, not just old contradictions.** On the very first sheet the sibling check had a second document to compare against, it caught two sentences citing the sibling sheet by name — a rule violation invisible from inside either document alone.

- **A tiny filter group (two options) still earns the full pipeline, and finishes leaner because of it, not despite it.** Two options meant one combination row, one measured-figures table, no toggle audit, no drill matrix — the doc landed at a third of its siblings' length. The value didn't come from padding it to match; the live check, the production sum-check, and the creation-path trace still found a real, sitewide, ongoing defect that a code read alone had first mis-diagnosed as something else entirely (trap 39).

- **Before concluding a destination is missing, run the filter-first ladder.** (a) Does an existing screen already own the subject (trap 16)? (b) Can a filter on an existing list express the slice? (c) Can the row carry the content the number promises (a discount drill must show discount amounts, and whether it can is a payload question, not a screen question)? Only if all three fail is a new surface worth proposing. On Collection this ladder resolved every candidate gap to filters and one existing screen; the "new screens needed" list came out empty.

- **A "not built" claim must name which path is missing: recording, filtering, or display.** One sheet said caution-money adjustments had "no working path in the system today". The list-filter path was missing; the recording path was live, taking real payments within weeks of the sheet being written. The over-general claim was wrong the day it shipped and read as authoritative for two more days. Recording, filtering and display are three different paths that die and ship separately.

- **Your own verification needs the same audit as an agent's.** One grounding round produced three measurement errors in a row — a case-sensitive grep that missed 31 capitalised hits, the wrong literal for a fixed phrase, a path that silently pointed at nothing — and each error read as "the source document is stale" when the source was right every time. The trap about verifying sub-agent findings runs both ways: when your checks contradict a grounded claim, audit the checks before the claim, starting with case, exact strings, and whether the file you measured exists.
- **A suite rule needs its threshold stated, or the next screen re-litigates it.** "Every screen whose records carry future dates gets the forward setting" was true and still wrong for a screen holding 45 stray future rows out of 340,524. The test is whether records carry future dates *by design*, not whether any exist — and the rule only became usable once the carve-out was written into it.
- **Reachability verdicts come from the filter surface the apps share, never from reading backend request handling alone.** The backend accepts more than any UI exposes, and a partial read of the backend misses codes the UI does expose; both errors happened on one screen, and a check against the app's own filter definitions flipped nine verdicts in both directions (eviction states, leaving-date windows and the already-expired agreement window all existed; a "confirmed" backend-only ✅ was fine). The app's filter file is the truth for "can a manager reach this"; the backend is the truth for "could it be built".

- **An uplift is cheapest run by the session that wrote v1 — and that is also its risk.** The author holds every ruling, which made the ledger fast to verify; the author also holds the pre-fix drafts, which is exactly how the resurrected-defect trap fired. The guard is mechanical, not mental: draft from the published file on disk, never from recall, and run the ledger against the file even when you are sure.

- **Audit restorations outweigh the word budget.** An uplift landed under the sibling ceiling, then the ledger and final read restored ~300 words of dropped rulings, putting it 3% over. The right call was to keep the rulings and correct the tracker's claim, not to trim other content to protect a number — a budget is an instrument for killing noise, and restored rulings are the opposite of noise. Say the overage plainly rather than silently re-trimming.

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

48. **A banned-word sweep that names one inflection misses the others, and the miss is invisible because the sweep reports clean.** The close-out grep for time coinages listed `follows`; the Complaints draft carried `follow the filter` four times, all the banned sense, and every sweep passed. Caught only by reading the cards aloud. **Sweep the word family, not the word:** `follow(s|ed|ing)?`, `reconcil(e|es|ed|ing)`. A grep that names a stem and lets the regex carry the endings cannot be defeated by a tense change.

49. **A rule inherited from recall of a sibling sheet can be the opposite of what the sheet says, and it reads as settled fact.** "Inventory's empty states carry no button" was recalled, stated as the suite rule, and used to argue against an Add Complaint button. Inventory's *healthy* states carry no button; its *not-set-up* state carries one, with the wording quoted in the sheet. Re-reading the source before citing it turned a claimed exception into no exception at all. **Before writing "the sibling rules X", read the sibling's own sentence for X, that session, not the memory of it.** Same shape as the frame-dismissal rule (a claim about a design file needs a live check): a claim about a sibling sheet needs a live read.

50. **A whole-session-scale first report is the wrong shape for an owner who decides card by card, and the cost is a rejected turn.** The first consolidated Phase 0 to 3 report was rejected as overwhelming; the same content delivered one card and one question at a time closed eleven cards and every cross-cutting section in one sitting. The gate the owner set, "proceed only if 100% satisfied being user first", fired eleven times and caught a real gap on the majority of them (a filter behaviour never settled, a state trigger too wide, a tap that would lie, a rule recalled wrong). **When the owner asks for one thing at a time, that is the method, not a preference to be worked around; and a "sure?" gate is a request to re-open, never a prompt to restate confidence.**

- **The greenfield screen with the least going in produced the most rulings and the most measured figures**, and needed the info-icon copy written where no sibling had. Complaints landed at 9,100 words against a 8,100 ceiling, over by the info-icon block that no other sheet carries. When a screen is the first to carry a layer, say the overage and keep the layer; do not trim rulings to protect a number set before the layer existed.
- **The suite's own house voice for info icons was measurable and had already been decided.** 46 shipped hints, median seven words, zero you/your, four calc blocks, a locked glossary with banned synonyms. Writing the eleventh screen's icons meant measuring that voice first, not inventing one; the first draft (two to four sentences, second person, ending in advice) was wrong on all three counts and would have shipped a second dialect beside the first.
