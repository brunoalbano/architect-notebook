# 00 · Architecture Fundamentals & Quality Attributes

> The backbone topic. Every later pattern is judged against the vocabulary you build here.
> This topic is slightly different from the others: instead of one pattern, you're building the
> **measuring stick** — the quality attributes (architecture characteristics) you'll trade off forever.

## Goal of this topic
- Build a working vocabulary of ~7–8 **quality attributes** (a.k.a. architecture characteristics):
  scalability, availability, latency/performance, consistency, security, cost, evolvability, operability.
- For each, be able to (1) define it crisply, (2) name what it *trades against*, and (3) make it
  **measurable** via a fitness function — turn "it should be fast" into "p99 < 200ms under 1k rps".
- Internalize the central truth: **architecture is choosing which characteristics to optimize at the
  expense of others.** There is no "good" architecture, only one fit for its drivers.

## Why this first
You can't evaluate caching, CQRS, or microservices without a yardstick. This topic *is* the yardstick.

## Sources (Claude will pin exact chapters next session)
- FoSA ch.4–7 (architecture characteristics, identifying & measuring them).
- Azure Well-Architected Framework — its five pillars as a real-world cross-check.
- A postmortem of your choice where a *missing* characteristic (e.g. ignored back-pressure) caused an outage.
- 📄 Starter brief already written: [`_sources/00-architecture-fundamentals/quality-attributes.md`](../_sources/00-architecture-fundamentals/quality-attributes.md)
  — read it, then fill `decision-card.md` in your own words (the numbers in the brief are illustrative templates).

## Deliverables for this topic
- `decision-card.md` — adapted: a table of your core quality attributes with definition, how to
  measure, what it trades against, and an Azure example.
- `adr/0001-*.md` — practice: given a concrete app brief, rank the top 3 driving characteristics and justify.
- `_journal/tradeoffs.md` — at least one entry on a characteristic pair that surprised you.

See `NEXT.md` for the immediate next step.
