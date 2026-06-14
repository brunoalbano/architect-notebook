---
name: case-study
description: Research and write a real-world architecture case study from engineering blogs/news — both "company dropped a popular pattern/tool and why" (reversals) and "company adopted a pattern/tool and it paid off" (successes). Saves a brief to _case-studies/ and links it to roadmap topics. Use when the user says "give me an architecture case study", "/case-study", or wants a real-world example of an architecture decision/migration.
---

# Architecture case study — research a real-world decision

## ⛔ HARD RULE — never invent a case study
Every case study MUST be a **real, verifiable, publicly documented** decision backed by **at least one
citable primary source** (the company's own engineering blog, conference talk, paper, or a reputable
publication). NON-NEGOTIABLE:
- **Do NOT fabricate** companies, decisions, quotes, numbers, dates, or outcomes — not even
  "representative" or "illustrative" examples. No composites, no "a typical company would…".
- **You MUST use WebSearch/WebFetch to locate and read the actual source(s) before writing.** Do not write
  from memory — memory of these stories is unreliable and dates/numbers drift.
- **If you cannot find and verify a real source, STOP and say so** — propose a different subject you *can*
  source, or ask the user to pick one. Producing nothing is correct; inventing one is a serious failure.
- Every brief leads with **working source links**. Any specific claim you couldn't confirm in a source is
  marked **"(unverified)"** explicitly — never smoothed over.
- Distinguish what a source *measured* from what it *marketed*; don't launder PR claims into facts.

Produce one well-sourced case study of a real architecture decision for a senior engineer becoming an
architect. Two flavors, both valuable — **rotate between them** (lean reversals, but mix in successes
regularly so the learner gets balanced judgment, not pattern-cynicism):
- **Reversal ★** — a company *dropping/reversing* a popular pattern/tool (microservices→monolith, leaving
  the cloud, serverless→containers, dropping a framework). This is where "when NOT to use" judgment lives.
- **Success** — a company *adopting* a pattern/tool where the complexity genuinely **paid off** (event
  sourcing, CQRS, microservices at the right scale, a CDC pipeline). Teaches "when it IS worth it" and
  what preconditions made it work — equally important so the learner doesn't conclude "patterns are traps".

Context in `_case-studies/README.md`.

## Steps
1. **Pick the subject.** If the user named a company/pattern, use it. Otherwise choose one that's notable
   or recent and that they haven't covered yet (check the index in `_case-studies/README.md`; the starter
   set there is a fallback). **Vary the flavor** — if the last few entries were reversals, pick a success
   story (and vice versa). Prefer subjects that connect to topics on `ROADMAP.md`.
2. **Research with real sources.** Use WebSearch/WebFetch to find **primary sources** (the company's own
   engineering blog or conference talk) plus one critical/independent take. Architecture news moves and
   memory is unreliable — **verify dates, numbers, and the actual decision against sources; do not assert
   specifics from memory.** If you can't verify a claim, mark it "(unverified)".
3. **Write the brief** from `_case-studies/template.md`, saved as `_case-studies/NNNN-<company>-<slug>.md`.
   Fill the **Architect's read ★** section properly — separate context-specific factors from the general
   lesson, and always answer "would this be right for a smaller team / for the learner's context?".
   Be skeptical of PR spin: distinguish *measured outcomes* from *marketing*.
4. **Link it back.** Map to specific `ROADMAP.md` topics. Note if it confirms/challenges any existing
   decision card or ADR in the repo, and whether it should seed a `_journal/tradeoffs.md` entry,
   a `_journal/revisit.md` trigger, or a good `/drill` prompt.
5. **Update the index** table in `_case-studies/README.md` (newest on top).

## Output to the learner
A tight verbal summary (don't just dump the file): the decision, the real reason, the one transferable
lesson, and the provocative question — *would you make the same call?* Offer to go deeper or to turn it
into a drill prompt.

## Optional
For a deep, heavily-cited investigation, consider delegating the research to the `deep-research` skill,
then format its output into the case-study template.
