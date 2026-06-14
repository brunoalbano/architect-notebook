# METHODOLOGY — How I study to become an architect

The job of an architect is **judgment under tradeoffs**, not recall. This program trains
decision-making. Every topic runs the same loop. Optimize for *owning the tradeoffs*, not
for finishing fast. Budget roughly **60% reading/writing, 40% coding** — coding exists to
make tradeoffs *felt*, not to build products.

## The loop: Read → Model → Build → Decide → Write → Revisit

### 1. Read (diverge)
Read 2–3 sources that **disagree** — a canonical book chapter, an official doc, and a
real-world blog post or postmortem. Disagreement is where the tradeoffs live. If a source
only lists benefits, it's marketing, not study material. (Claude curates these per topic and
can summarize dense docs into `_sources/<topic>/`.)

### 2. Model (compress)
Before coding, fill `decision-card.md`. Articulating **when / when-not** is the actual skill.
~30 min. If you can't write the "When NOT to use" section, you haven't read enough yet.

### 3. Build (concretize)
Write the **smallest runnable spike that makes the downside hurt** — not a demo of the happy path.
Examples:
- Caching → introduce a deliberate invalidation bug; feel staleness.
- Saga vs 2PC → kill a service mid-flow; watch compensation (or its absence).
- CQRS → measure read/write model sync lag.

Default stack: **.NET / C# on Windows**, Docker for infra. The *pattern* is the subject, so use
familiar tech to keep the tooling out of the way.

### 4. Decide (apply judgment)
Write an **ADR** (`adr/0001-*.md`) for a realistic scenario: *"Given these requirements, I'd
choose X over Y because…"*. ADRs are the architect's daily artifact. This step is the point of
the whole loop.

### 5. Write (teach it back)
A short `README.md` you could hand to a mid-level engineer. If you can't explain when **not** to
use the thing, you don't own it yet.

### 6. Revisit (mature the judgment) — the "no rush" advantage
Every few topics, reopen an **old** decision card / ADR and attack your past self: *"Would I still
decide this? What did I miss now that I know more?"* Re-deciding old decisions with new knowledge
is how architectural judgment actually matures. Most people skip this. Don't.

## Per-topic durable artifacts
| File | Purpose |
|------|---------|
| `decision-card.md` | Compressed tradeoff analysis (the core artifact) |
| `adr/0001-*.md` | At least one practiced decision on a realistic scenario |
| `README.md` | Teach-it-back summary |
| `spike/` | Smallest runnable code that makes the tradeoff felt |
| `NEXT.md` | One line: where I stopped / what's next (resumability) |

## Cross-cutting habits
- **Always compare against the alternative.** "REST vs gRPC vs GraphQL," never "gRPC" alone.
- **Tie every topic to quality attributes** (scalability, availability, latency, consistency,
  security, cost, evolvability, operability). Architecture is trading these against each other.
- **Tradeoff journal** (`_journal/tradeoffs.md`): log every surprising tradeoff. This becomes your
  personal whiteboard/interview prep and your evidence of growth.
- **Capstone** (`_capstone/`): every few topics, fold what you learned into one evolving system
  design so patterns interact instead of living in isolation. Integration is where architecture lives.
- **Resumability:** sessions are irregular. Always leave `NEXT.md` current so returning costs minutes.
