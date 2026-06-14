# System Design Drills — Mock Interviews

Periodic, timed, interview-style system design exercises. Claude plays the **interviewer**: poses a
prompt, drives the session with realistic probing and curveballs, then scores you against a rubric
and logs the result so you can see judgment improving over time.

## When to run one
Roughly every 4–6 completed topics, or any time you want a checkpoint. A drill *integrates* topics —
it reveals whether you can actually wield the patterns under pressure, not just describe them.

## How to start
Tell Claude: **"run a system design drill"** (optionally name a difficulty or theme), or run **`/drill`**.
The `/drill` skill (`.claude/skills/drill/`) carries the interviewer protocol; the protocol below is the
human-readable reference for it.

## The interview protocol (Claude drives this)
A real interview is ~45 min. Claude paces you through these phases and won't let you skip ahead:

1. **Requirements & scoping (5–8 min)** — Claude gives a deliberately vague prompt
   ("design a ride-sharing backend"). *You* must ask clarifying questions, pin functional + non-functional
   requirements, and state assumptions. Claude answers as the interviewer (sometimes ambiguously, on purpose).
2. **Estimation (3–5 min)** — back-of-envelope: users, QPS, storage, bandwidth. Claude checks your math.
3. **High-level design (10–15 min)** — you sketch the architecture (in your diagram tool of choice, see
   `_drills/tooling.md`) and narrate components, data flow, APIs.
4. **Deep dive (10–15 min)** — Claude picks 1–2 components and drills: data model, partitioning,
   consistency, the failure modes. Expect *"what happens when this node dies?"*
5. **Curveballs & scaling (5–10 min)** — Claude changes a requirement ("now 100× traffic", "now strongly
   consistent", "now multi-region") to test how you adapt and whether you know your tradeoffs.
6. **Wrap-up** — you summarize tradeoffs and what you'd do with more time.

Then Claude scores you (rubric below), gives specific feedback, and saves a record from
`_drills/template.md` as `_drills/NNNN-<slug>.md`.

## Scoring rubric — THE SINGLE SOURCE OF TRUTH (0–4 each; senior-architect bar is 3+)
> Both the `/drill` skill and the `design-evaluator` subagent defer to **this table**. If you tune a
> dimension or its bar, change it **here only** — the skill and agent carry just the dimension names and
> point back to this file, so they don't drift.

Bar: 0 = absent/wrong · 1 = weak · 2 = adequate · 3 = strong (senior-architect bar) · 4 = exceptional.
Most real candidates score 2s with a few 3s; reserve 4s for genuinely exceptional reasoning. Penalize
hand-waving ("we'll just add a cache") that isn't justified.

| Dimension | What "strong" looks like | Quality attribute(s) it really tests |
|-----------|--------------------------|--------------------------------------|
| **Requirements & scoping** | Drives out functional + non-functional reqs; states assumptions; clarifies before designing | *all — it's where the driving attributes get named* |
| **Estimation** | Sane back-of-envelope; uses numbers to justify design choices | scalability, cost, latency |
| **High-level design** | Clean component decomposition; clear data flow; sensible API boundaries | evolvability, operability |
| **Data modeling & storage** | Right store for the job; schema/partitioning thought through | consistency, scalability, latency |
| **Tradeoff reasoning** ★ | Names alternatives and *why this one here*; ties choices to quality attributes | *all — explicitly* |
| **Scalability & bottlenecks** | Finds the real bottleneck; scales the right tier; back-pressure aware | scalability, latency, cost |
| **Failure modes & resilience** ★ | Anticipates partial failure, the 3am page, blast radius, recovery | availability, operability |
| **Communication & structure** | Leads the session, manages time, clear diagram, thinks out loud | *meta — how the architect sells the tradeoff* |

★ = the dimensions that most separate architects from senior engineers — Claude weights feedback here.
The right-hand column ties each dimension back to the quality-attribute vocabulary from
[topic 00](../00-architecture-fundamentals/README.md) / [`_sources/00…/quality-attributes.md`](../_sources/00-architecture-fundamentals/quality-attributes.md) — the same yardstick the decision cards use.

## Drill log
Newest on top. Track scores to see the trend.

| # | Date | Prompt | Avg score | Weakest dimension |
|---|------|--------|-----------|-------------------|
| _No drills run yet — your first `/drill` adds row 0001 here._ |||||
