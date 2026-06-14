# Decision Card — Quality Attributes (Architecture Characteristics)

> **This topic's card is different on purpose.** Every other topic studies one *pattern* against its
> alternatives. Topic 00 builds the **yardstick** you'll judge all of them with: a working vocabulary of
> ~7–8 quality attributes, each made *measurable* (a fitness function), and — the real skill — each one's
> **tension** with the others. Architecture is choosing which to optimize at the expense of which.
>
> Fill the table in your own words. The reference brief is
> [`_sources/00-architecture-fundamentals/quality-attributes.md`](../_sources/00-architecture-fundamentals/quality-attributes.md);
> don't copy it — articulating these yourself *is* the exercise.

## The core attribute table  ★ (this is the deliverable)

| Attribute | Crisp definition (your words) | How I'd **measure** it (fitness function — a number, not an adjective) | Trades **against** | Azure lever / example (AWS in one line) |
|-----------|-------------------------------|------------------------------------------------------------------------|--------------------|------------------------------------------|
| **Scalability** | | e.g. *sustains 1k→10k rps with p99 latency flat* | cost, consistency, simplicity | |
| **Availability** | | e.g. *99.95%/mo = ≤ 21.9 min downtime; measured by synthetic probe* | cost, consistency, latency | |
| **Latency / performance** | | e.g. *p99 < 200 ms read under 1k rps* | cost, consistency (caching), scalability | |
| **Consistency** | | e.g. *read-your-writes within X ms; 0 lost updates under partition* | availability, latency (CAP/PACELC) | |
| **Security** | | e.g. *0 secrets in code; all data encrypted at rest/in transit; MTTR-to-patch < N days* | latency, cost, evolvability/dev-speed | |
| **Cost** | | e.g. *$/1k requests; idle $/mo; cost stays sub-linear to traffic* | scalability, availability, performance | |
| **Evolvability** | | e.g. *change-failure rate; lead-time for a typical change; cyclomatic/coupling fitness fn* | performance, simplicity | |
| **Operability** | | e.g. *MTTD/MTTR; % requests traced; one-command deploy & rollback* | cost, dev-speed | |

> Pick the **7–8 you'll actually use as your working set** (the open question in `NEXT.md`). It's fine to
> add domain ones later (e.g. *auditability*, *portability*, *accessibility*) — but own these first.

## The central tensions  ★ (the part that makes you an architect)
Name the pair, the conflict, and the dial that mediates it. Add your own as you study later topics.

| Tension pair | Why they pull apart | The dial that mediates it |
|--------------|---------------------|---------------------------|
| Consistency ↔ Availability | a network partition forces a choice (CAP) | sync vs async replication; quorum size |
| Latency ↔ Consistency | strong reads cost coordination/round-trips (PACELC's "else") | caching, read replicas, staleness budget |
| Scalability ↔ Cost | headroom and redundancy aren't free | autoscale floors/ceilings, spot/reserved capacity |
| Security ↔ Dev-speed/evolvability | controls add friction and latency | shift-left automation, paved-road platform |
| Evolvability ↔ Performance | abstraction/indirection costs cycles | measure before abstracting; hot-path carve-outs |

## How to *elicit* the driving attributes for a given system
You won't optimize all eight — you rank the **top ~3 drivers** from the business context, then design for
those and accept the rest as "good enough." Practice this ranking; it's the move the ADR below rehearses.
- Source them from: non-functional requirements, the business model, regulatory context, the cost of an
  outage, the team's size/skill, and expected change rate.
- Distinguish **implicit** (always-on: security, operability) from **driving** (this system's defining few).

## Failure modes of the vocabulary itself  ★
- **Adjective-driven design** — "it should be fast/scalable" with no number. If it can't be a fitness
  function, it can't be traded or tested. This is the #1 failure.
- **Optimizing all attributes at once** — yields an over-engineered, expensive system fit for nothing.
  Not choosing *is* a choice (usually the wrong one).
- **Ignoring implicit attributes** — security/operability skipped because nobody "asked" for them.
- **Frozen ranking** — drivers change as the business scales; a never-revisited ranking goes stale
  (that's what the Revisit loop is for).

## When this vocabulary pays off / when it's overkill
- ✅ Any non-trivial design decision, any ADR, any drill — this is the lingua franca.
- ⛔ A throwaway script or a CRUD app with one user and no SLA — naming eight attributes is ceremony.

## Spike notes
What I built to make a tension *felt* (e.g. force a CAP choice: kill the network between two replicas and
watch a strongly-consistent read fail vs an eventually-consistent read serve stale). Link to `spike/`.

## Open questions / to revisit
- Which exact 7–8 are my working set, and is each one expressed as a real fitness function?
- (Revisit after Tier 1–2: did studying CAP/replication/caching change how I'd measure consistency & latency?)
