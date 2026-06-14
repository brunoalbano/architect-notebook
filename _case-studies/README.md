# Architecture Case Studies — real decisions from the wild

Real-world architecture decisions from engineering blogs, talks, and news. Two flavors, both collected
here (rotate between them):
- **Reversals ★** — *a company dropping a popular pattern/tool and explaining why.* The exact "when NOT
  to use" knowledge that's hard to get from books — a team paying the cost of a tradeoff in public.
- **Successes** — *a company adopting a pattern/tool where the complexity genuinely paid off.* Teaches
  "when it IS worth it" and the preconditions that made it work — so you build balanced judgment, not a
  reflex that every pattern is a trap.

## Why this matters
- Books teach patterns; case studies teach **context** — why the same pattern was right here, wrong there.
- "We migrated *away* from X" stories expose the downside the original adopters didn't see coming;
  "X paid off for us" stories show the scale/team/preconditions where the complexity earned its keep.
- They're the best raw material for the **revisit** habit and for system-design **drills**.

## How to get a new one
Run **`/case-study`** (or say "give me an architecture case study"). Claude researches a recent or
notable decision, writes a brief here using `template.md`, and links it to the relevant `ROADMAP.md`
topics. Ask for a specific company/pattern, or let Claude pick a fresh one. Do this "from time to time" —
no schedule; pull one when you have appetite.

> **Every entry here is a real, sourced, publicly-documented decision — never invented.** If a story can't
> be backed by a citable primary source, it doesn't get written. Treat any brief lacking working source
> links as suspect and delete it.

## Reading lens (what to extract every time)
1. What did they have, and what **pain/quality-attribute failure** triggered the change?
2. What did they switch **to**, and what tradeoff did they knowingly accept?
3. What was **context-specific** (their scale/team/$) vs a **general** lesson?
4. Would the same call be right for a *smaller* team? (Usually the punchline.)

## Starter set
> Claude will verify details/dates against sources when writing each brief; treat these as prompts.

**Reversals ★ — "dropped/reversed the pattern/tool"**
- **Amazon Prime Video** — serverless/microservices (Step Functions + Lambda) → a consolidated
  "monolith" for audio/video quality monitoring; reported large cost cut. The microservices backlash poster child.
- **Segment** — microservices → monolith (operational complexity of hundreds of services).
- **37signals / DHH** — leaving the cloud back to owned hardware ("cloud exit"); also dropping TypeScript from Turbo.
- **Dropbox** — leaving AWS S3 for their own storage infra ("Magic Pocket").
- **Uber** — monolith → microservices → **DOMA** (domain-oriented microservice architecture) as a correction.
- **Khan Academy** — Python monolith → Go services.

**Successes — "adopted it and the complexity paid off"**
- **Netflix** — microservices + chaos engineering at global streaming scale; the canonical "it worked" story.
- **Shopify** — a deliberate **modular monolith** at massive scale (success of *not* fragmenting).
- **Stack Overflow** — famously monolithic, vertically-scaled, cheap-to-run at huge traffic.
- **Event sourcing / CQRS in banking & ledgers** — where the audit trail and replay genuinely justify the cost.
- **LinkedIn / Kafka** — building the log-as-backbone and why it paid off at their scale.
- **Discord** — Cassandra → ScyllaDB for trillions of messages (a tool swap that worked).
- (Add your own — both flavors — as you find them.)

## Index (newest on top)
| # | Date added | Flavor | Company | Decision (from → to) | Roadmap topics | Brief |
|---|-----------|--------|---------|----------------------|----------------|-------|
| _None yet — your first `/case-study` adds row 0001 here (pick from the starter set above or name one)._ |||||||
