# estudo — Becoming a Software Architect

A long-term, AI-guided, self-study program. From Senior Full-Stack Engineer → Software Architect,
optimized for **deep mastery** and learning by **reading + coding** (not video).

## Start here
1. Read `METHODOLOGY.md` — the **Read → Model → Build → Decide → Write → Revisit** loop.
2. Open `ROADMAP.md` — the tiered topic backlog and canonical sources. Active topic is marked there.
3. Each session, run **`/topic`** (or say *"continue my architecture study"*) — Claude reads the active
   topic's `NEXT.md` and drives the next step.

## Commands (skills) — how to drive this repo with Claude
> Skills are loaded at session start. After adding/changing skills, **restart Claude Code** (or reload
> the window) so they register. Invoke with the slash command or the natural-language trigger.

| Command | What it does | When to use |
|---------|--------------|-------------|
| **`/topic`** | Starts/continues a topic through the full learning loop: orients from `NEXT.md`, curates 2–3 disagreeing sources, summarizes dense docs into `_sources/`, scaffolds the decision card / ADR / spike, and challenges your reasoning. Also handles **revisit** mode (devil's advocate on old decisions). | Every study session — the daily driver. |
| **`/drill`** | Runs a ~45-min system design **mock interview**: Claude plays interviewer (vague prompt → scoping → estimation → design → deep dive → curveballs), then scores you on the 8-dimension rubric and logs it to `_drills/`. | Checkpoint every ~4–6 completed topics. |
| **`/wrap`** | Closes out a session for resumability: updates `NEXT.md`, ticks `ROADMAP.md` progress, prompts a tradeoff-journal entry, and nudges capstone/drill when due. | End of every session. |
| **`/case-study`** | Researches a real-world architecture decision (biased toward "company dropped a popular pattern/tool and why"), writes a sourced brief to `_case-studies/`, and links it to roadmap topics. | From time to time, for real-world judgment. |
| **`/podcast`** | Assembles a NotebookLM-ready audio pack (`NN-topic/podcast-source.md` + a tuned focus prompt) so you can generate a free two-host podcast of a topic to study/review while listening. No TTS, no cost, no install. | To learn/review a topic on the go. |

### Subagents (Claude spawns these — you don't call them directly)
| Agent | Role | Invoked by |
|-------|------|-----------|
| `source-summarizer` | Compresses one dense doc into a focused brief in `_sources/`. Runs several in parallel. | `/topic` Read phase |
| `design-evaluator` | Grades a drill transcript **blind**; 3 run in parallel and are averaged to remove bias. | `/drill` scoring (milestone drills) |

**Typical session:** `/topic` → study & build → `/wrap`.  Every few topics: `/drill`.

## Layout
```
CLAUDE.md          AI study-guide instructions (Claude reads this every session)
METHODOLOGY.md     The learning loop and habits
ROADMAP.md         Tiered topic backlog + canonical sources + progress
_templates/        decision-card.md · adr-template.md · NEXT.md  (copy per topic)
_sources/          AI-generated summaries of dense docs, per topic
_journal/          tradeoffs.md (surprising tradeoffs) · revisit.md (queue of decisions to re-attack)
_drills/           system design mock interviews — protocol, rubric, tooling guide, logs
_case-studies/     real-world architecture decisions (esp. "dropped a popular pattern/tool")
_capstone/         the evolving system design that ties topics together
NN-topic/          one folder per topic: README · decision-card · adr/ · spike/ · NEXT
```

## Environment
Windows 11 · VS Code · .NET/C# default spikes · Docker Desktop for infra · Azure-first ecosystem.

## How a topic flows
`decision-card.md` (the tradeoff analysis) + `adr/0001-*.md` (a practiced decision) are the durable
artifacts. The spike exists to make a downside *felt*. `README.md` is the teach-it-back. `NEXT.md`
keeps it resumable after long gaps.
