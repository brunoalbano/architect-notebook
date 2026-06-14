# Diagramming & Whiteboarding Tools for Design Exercises

The tools you'll actually use in interviews and design sessions. Pick a **primary** for speed under
pressure and learn its shortcuts cold — fumbling with a tool wastes interview minutes and looks junior.
Windows + VS Code friendly throughout.

## Real-time / interview whiteboarding (fast, low-friction)
| Tool | Why / when | Notes for you |
|------|-----------|---------------|
| **Excalidraw** ★ | The de-facto remote-interview whiteboard. Hand-drawn feel, zero setup, instant boxes/arrows. | **Recommended primary.** Free, browser-based, has a **VS Code extension** (`.excalidraw` files). Learn it cold. |
| **draw.io / diagrams.net** ★ | Most common for "real" architecture diagrams; huge shape libraries (Azure/AWS icons). | Free, has an official **VS Code extension** (`.drawio`/`.drawio.svg`). Great for capstone + decision cards. |
| **Whimsical / tldraw** | Fast, pretty; some interviewers' platforms use them. | Browser. Good fallbacks; less critical to master. |
| Interviewer's own tool (CoderPad, Miro, etc.) | You don't choose it — but they're all box-and-arrow. | Mastering Excalidraw transfers directly. |

## Diagrams-as-code (for the repo, capstone, ADRs — versionable in git)
| Tool | Why / when | Notes for you |
|------|-----------|---------------|
| **Mermaid** ★ | Renders directly in Markdown/GitHub; fast for sequence + flow + C4-ish diagrams. | **Recommended for this repo.** VS Code preview extension; embed straight in `README`/`decision-card`. |
| **PlantUML** | More powerful/uglier; strong for sequence & component diagrams. | Needs Java; VS Code extension exists. Use if Mermaid hits limits. |
| **Structurizr DSL** ★ | Purpose-built for the **C4 model**; one model → multiple consistent views. | Pairs with topic 02 (C4) and topic 60. Best for the capstone's C4 levels. |
| **Python `diagrams`** | Cloud-architecture diagrams with real provider icons, as code. | Optional; nice for polished Azure/AWS diagrams. |

## Recommended setup for you (Windows + VS Code)
- **Interview reflex tool:** Excalidraw (VS Code extension) — practice every drill in it.
- **Repo / capstone diagrams:** Mermaid for inline, Structurizr DSL for the C4 capstone, draw.io for
  anything needing Azure icons.
- Install the VS Code extensions once; Claude can scaffold starter `.excalidraw` / `.drawio` / `.mmd`
  files per drill or topic on request.

## Skill, not just tooling
The tool is 10%; the skill is **what to draw and in what order**. Strong sequence:
client → API/gateway → services → data stores → async/messaging → cross-cutting (cache, CDN, auth) →
then annotate the bottleneck and the failure points. Drills train this ordering until it's automatic.
