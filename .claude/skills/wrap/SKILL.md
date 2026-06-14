---
name: wrap
description: Close out a study session so the repo stays resumable after long gaps. Updates the active topic's NEXT.md, ticks progress in ROADMAP.md, and prompts a tradeoff-journal entry. Use when the user says "wrap up", "I'm done for today", "/wrap", or is ending a study session.
---

# Wrap up the study session

The learner's schedule is irregular — leaving the repo cleanly resumable is the whole point. Do this
quickly and concretely; don't pad it.

## Steps
1. **Identify the active topic** from `ROADMAP.md` / the most recently touched topic folder.
2. **Update that topic's `NEXT.md`** — set the loop stage, "last did", the next concrete step, and any
   open question. Keep it to 1–3 lines so returning costs minutes. This is the most important step.
3. **Tick `ROADMAP.md`** — update the topic's loop-stage checkboxes and the "Progress at a glance" block
   (active topic, completed list). If a topic just finished, mark it and note the next suggested topic.
4. **Tradeoff journal** — ask if anything surprised them this session; if so, add a one-line dated entry
   to `_journal/tradeoffs.md` (newest on top).
4b. **Revisit queue** — if a topic *completed* this session, add a row to `_journal/revisit.md` with a
   **trigger** (the later topic whose knowledge should make them re-attack this decision). Also scan the
   queue: if any row's trigger topic is now done, flip it to `due` and mention it in the recap.
5. **Capstone nudge** — if several topics have completed since the last capstone update, gently suggest
   folding the new pattern into `_capstone/` next time (don't force it now).
6. **Drill nudge** — if ~4–6 topics have completed since the last drill, suggest a `/drill` checkpoint.
7. **Topic-completion nudges** — if a topic *completed* this session, offer (don't force) the two
   reinforcement options:
   - **`/case-study`** — a real-world decision related to the topic (reversal or success; see
     `_case-studies/`). Name a fitting subject if obvious (e.g. microservices → Prime Video or Netflix).
   - **`/podcast`** — assemble a NotebookLM audio pack so they can *review* the finished topic on the go.
   - **A video/conference talk or podcast episode** — for *review only* (not as a primary source; see
     CLAUDE.md). A good recorded talk is fine to reinforce a finished topic. If you suggest one, name a
     specific, reputable talk/episode (speaker + venue or show + episode) and one line on why it fits.
   Closing a topic is the best moment for a real example, an audio recap, or a talk to cement it.

## Output
End with a 3-line recap: what got done, where `NEXT.md` now points, and the single suggested next action
for the next session. Confirm the repo is left resumable.
