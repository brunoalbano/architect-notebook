---
name: podcast
description: Assemble a NotebookLM-ready "audio pack" for a study topic so the learner can generate a free two-host podcast (NotebookLM Audio Overview) to study/review while listening. Produces a consolidated source markdown + a tuned customization prompt. No TTS, no cost, no install. Use when the user says "/podcast", "make a podcast for topic NN", or wants an audio version of a topic to listen to.
---

# Podcast pack — prep a topic for NotebookLM Audio Overview

The learner studies while commuting via **Google NotebookLM** (free; generates a two-host audio overview
from uploaded sources). Your job is NOT to make audio — it's to assemble the best possible *source pack*
and a *focus prompt* so the generated audio targets a senior engineer (tradeoffs, when-NOT-to-use,
failure modes), not basics. Zero cost, zero setup.

## Steps
1. **Pick the topic.** Use the named topic, or the active one from `ROADMAP.md` / `NEXT.md`. Works best on
   a topic that's at least partly studied (has a filled `decision-card.md`), but can also produce a
   "preview" pack from the README + curated sources for listening *before* studying.
2. **Assemble the pack.** Write `NN-topic/podcast-source.md` — a single clean, self-contained document
   combining, in this order:
   - A one-paragraph framing of the topic and why it matters.
   - The topic's `decision-card.md` content (or its README if the card isn't filled yet), lightly edited
     into flowing prose that reads aloud well (expand terse bullets; spell out acronyms on first use).
   - Key tradeoffs, the "when to use / when NOT to use" contrast, and failure modes — emphasized.
   - Any `_sources/<topic>/*.md` briefs, folded in.
   - 3–5 "questions a senior architect should be able to answer after this" at the end (gives the hosts
     something to explore).
   - Keep it accurate to what's in the repo — **do not invent** facts, numbers, or sources to pad it.
3. **Write the focus prompt.** Output a ready-to-paste NotebookLM customization prompt, e.g.:
   *"Audience: a senior engineer who already knows the basics and wants architect-level judgment. Focus on
   tradeoffs, when NOT to use this, and failure modes — not introductory definitions. Compare against the
   main alternatives. Keep it ~12–15 minutes, conversational but technically precise. Assume .NET/Azure context."*
   Tune it to the specific topic.
4. **Give the learner the workflow** (concise, see below).

## The NotebookLM workflow (tell the learner)
1. Go to **notebooklm.google.com** → **New notebook**.
2. **Add source** → upload `NN-topic/podcast-source.md` (or copy-paste its contents; optionally add the
   real source URLs as additional sources for richer audio).
3. Click **Audio Overview** → **Customize** → paste the focus prompt → **Generate**.
4. When ready, **download** the audio for offline listening. (Optionally drop the file path note in the
   topic's README so you remember it exists.)
5. For review later, the same notebook also lets you ask follow-up questions of the sources.

## Notes
- This pairs with the `/wrap` topic-completion nudge: a finished topic is a great one to turn into a
  review podcast.
- `podcast-source.md` is a **point-in-time snapshot** of the decision card / sources — it does *not*
  auto-update. If you regenerate after the topic changes, rebuild the pack first so the audio isn't stale.
  Treat it as a derived artifact, not a source of truth.
- NotebookLM features/limits change; if the UI differs, the principle holds — upload the pack, steer with
  the prompt, generate, download.
- If the learner ever wants *fully automated* local audio instead (no manual NotebookLM step), the
  fallback is a TTS engine like `edge-tts` (free) — but NotebookLM's quality/effort tradeoff is better for now.
