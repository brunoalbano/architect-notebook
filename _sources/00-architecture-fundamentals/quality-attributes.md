# Source brief: Quality Attributes (Architecture Characteristics) — the core vocabulary

- **Source:** Synthesis of *Fundamentals of Software Architecture* (Richards & Ford) ch. 4–7 + Microsoft
  **Azure Well-Architected Framework** (the five pillars). Cross-check against the live docs.
- **Date summarized:** 2026-06-14
- **Reliability:** canonical book + official vendor framework — high for *concepts*. The **example
  numbers below are illustrative templates, not benchmarks.**
- ⚠️ **Verify before relying:** every threshold (p99, 99.95%, $/req) is a placeholder showing the *shape*
  of a fitness function — replace with numbers from your real system's NFRs. Do not cite these as facts.

## What this source argues / covers
There is **no "good" architecture, only one fit for its drivers.** Architecture is the activity of
choosing which **quality attributes** (FoSA calls them *architecture characteristics*) to optimize at the
expense of others. FoSA's contribution: characteristics should be **measurable, testable, and few** —
expressed as *fitness functions* so "evolvability" or "scalability" stops being an adjective. Azure's WAF
gives the same idea an operational spine: five pillars (Reliability, Security, Cost Optimization,
Operational Excellence, Performance Efficiency) you can run a structured review against.

## Key points for an architect
- **Problem it addresses:** without a shared, measurable vocabulary you can't compare designs, justify a
  decision, or test that the system still meets its goals over time. Decisions become opinion.
- **How it works (brief):** name the system's *driving* characteristics (FoSA: keep it to a handful — too
  many means you've optimized nothing), make each **measurable** via a fitness function (an automated or
  manual test of "are we still within budget?"), and design by trading them explicitly.
- **Benefits:** decisions become defensible and revisitable; NFRs become testable; tradeoffs become
  visible instead of accidental.
- **Costs / downsides / failure modes:**
  - *Adjective-driven design* — unmeasurable goals ("fast", "scalable") that can't be tested or traded.
  - *Optimizing everything* — over-engineered, costly, fit for nothing. Choosing is the job.
  - *Ignoring implicit characteristics* — security/operability nobody explicitly "requested".
  - *Stale ranking* — drivers shift as the business scales; an un-revisited ranking misleads.
- **When to use / when NOT to use:** use for any non-trivial design, ADR, or drill. Overkill for a
  throwaway script or single-user CRUD with no SLA.
- **Alternatives it compares against:** ISO/IEC 25010 quality model (more exhaustive, heavier); ATAM /
  risk-storming as *evaluation methods* that consume this vocabulary (topic 61). The vocabulary is the
  input; those are processes over it.
- **Quality attributes affected:** this *is* the set.

## The working set (own these ~8)
| Attribute | One-line definition | Fitness-function *shape* (replace numbers) | Pulls against |
|-----------|---------------------|--------------------------------------------|----------------|
| Scalability | throughput holds as load grows | rps sustained with p99 flat from N→10N | cost, consistency |
| Availability | % of time serving correctly | error-budget vs target (e.g. 99.95%/mo) via synthetic probe | cost, consistency |
| Latency / performance | response time under load | p99 < X ms at Y rps | cost, consistency |
| Consistency | how current/agreed reads are | read-your-writes window; 0 lost updates under partition | availability, latency |
| Security | confidentiality/integrity/auth | 0 plaintext secrets; encrypted in transit+rest; patch MTTR | latency, dev-speed |
| Cost | $ to build/run/scale | $/1k req; idle $/mo; sub-linear cost vs traffic | scalability, availability |
| Evolvability | ease/safety of change | lead-time for change; change-failure rate; coupling fitness fn | performance |
| Operability | ease of run/observe/recover | MTTD/MTTR; % traced; one-command deploy+rollback | cost, dev-speed |

## The tensions worth memorizing
- **CAP** — under a network partition you choose Consistency *or* Availability, not both.
- **PACELC** — *Else* (no partition) you still trade **L**atency vs **C**onsistency. The everyday tax.
- **Scalability ↔ Cost** — headroom/redundancy cost money; autoscale bounds are the dial.
- **Security ↔ Dev-speed** — controls add friction; paved-road automation mediates.
- **Implicit vs driving** — security & operability are usually *implicit* (always required); each system
  has a few *driving* characteristics that define it. Rank the top ~3 drivers; design for those.

## Notable specifics
- FoSA: characteristics are **explicit** (in requirements) or **implicit** (assumed — the dangerous ones);
  keep the driving set **small** and **measurable**. *(verify exact phrasing against ch. 4.)*
- Azure WAF five pillars map onto the attributes above (Reliability→availability/consistency;
  Performance Efficiency→scalability/latency; plus Cost, Security, Operational Excellence). AWS
  Well-Architected is the one-line equivalent (six pillars; adds Sustainability). *(verify pillar lists.)*

## Open questions this source leaves
- Where's the line between "driving" and "implicit" for a given system — who decides, and how often re-decided?
- How many fitness functions can you realistically automate before the test suite itself becomes a cost?
