# 01 · Architecture Decision Records (ADRs)

> A **practice** topic, not a pattern. The ADR is the artifact you produce in the "Decide" step of every
> other topic — so owning the format and the *thinking* behind it pays off across the whole program.

## Goal of this topic
- Know what an ADR is (and isn't): a dated record of **one decision**, the **context that forced it**, the
  **alternatives rejected**, and the **consequences accepted** — captured at decision time.
- Internalize that an ADR's value is **mostly in Context + Alternatives + Consequences**, not the decision.
- Be able to write a one-page ADR where the decision feels almost obvious *because* the context is so well stated.
- Understand the lifecycle: **immutable, append-only, superseded** (never edited) — the history is the point.

## Why study it early
Every roadmap topic ends with you writing a real ADR. Learn the format now and the dozen you'll write
during tiers 1–2 become deliberate practice instead of busywork.

## Sources (Claude will pin exact links when you start)
- Michael Nygard's original "Documenting Architecture Decisions" post (the canonical source).
- adr.github.io — the ADR organization, tooling (e.g. `adr-tools`), and template variants (MADR, etc.).
- FoSA — the architecture-decisions chapter.
- A real public ADR set (many OSS projects keep `docs/adr/`) to see the format in the wild.

## Deliverables for this topic
- `decision-card.md` — adapted: instead of a pattern's tradeoffs, capture the ADR *practice* — anatomy,
  golden rules, common failure modes, when an ADR is/ isn't warranted, template variants compared.
- `adr/0001-*.md` — **meta-practice:** write a real ADR for a given scenario (e.g. "choose the orders
  datastore"), then have Claude critique it against the rubric — especially your Context and Alternatives.
  Write a second ADR that **supersedes** the first to practice the lifecycle.
- `_journal/tradeoffs.md` — note anything that surprised you about the format.

## When you start
Ask Claude (or run `/topic`) — it will curate the sources and then **teach the ADR method**, then give you
a scenario to draft so you learn by writing, not by reading a summary.

See `NEXT.md` for the immediate next step.
