# Revisit Queue

> Revisit is the **"no-rush advantage"** — re-deciding old decisions with new knowledge is how
> architectural judgment matures (METHODOLOGY step 6). Most people skip it because nothing *tracks* it.
> This file is that tracker. `/wrap` adds a row when a topic completes; `/topic` revisit-mode works the
> top of the queue and logs the outcome in the topic's ADR **Revisit log**.

## How it works
- When a topic finishes, add it here with a **trigger** — the later topic whose knowledge should make you
  re-attack this one (e.g. "revisit *Caching* after *Consistency/CAP*").
- A revisit is **due** once its trigger topic is completed. Work due rows first.
- When done, set Status to `revisited <date>`, write one line on what changed (or "held — still sound"),
  and ensure the topic's `adr/*.md` **Revisit log** has the dated entry.

## Queue
Newest on top. Status: `queued` · `due` · `revisited <date>` · `held <date>`.

| Topic | Queued | Revisit trigger (what should change my mind) | Status | What changed on revisit |
|-------|--------|----------------------------------------------|--------|--------------------------|
| 00 · Architecture fundamentals | 2026-06-14 | After Tier 1–2 (CAP, replication, caching): are my consistency/latency fitness functions still right? | queued | |
