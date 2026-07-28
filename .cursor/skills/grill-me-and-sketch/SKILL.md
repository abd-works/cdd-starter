---
name: grill-me-and-sketch
description: >
  Run an interactive grill-and-sketch loop to surface design shape early across BDD, Modules, Stories, and UX perspectives.
  Use when starting a new design, exploring solution options, resolving design branches interactively, or sharpening understanding
  through a particular lens (behavior, structure, interaction, or experience) before committing to formal generation.
---

# Sketch Skill

Sketch rough, multi-perspective artifacts before generating formal deliverables. Work **theme by theme**; check that perspectives agree before moving on. Surface design shape early, reason in the open, and establish shared understanding cheaply.

---

## Step 0 — Analyze context and pick perspective(s)

Read the ask. Scan `.context/` for existing work. Name the specific open question each perspective will close.

- **Stories** — who does what, in what sequence
- **Modules** — module boundaries, structure, connections
- **BDD** — which real-world situations the logic must handle
- **UX** — what the user sees and how they move between screens

> **HARD RULE:** Pick a perspective only if you can name the open question it will answer. If you can't, don't add it.

**Confirm with the user before proceeding.** Use the `AskQuestion` tool — `allow_multiple: true`, one call — listing all four perspectives as options, with your recommended set pre-selected. Default all four when the ask is greenfield or broad. Wait for the answer before moving to Step 0.5.

> **HARD RULE:** Do not scaffold or sketch until the user has confirmed which perspectives are active.

---

## Step 0.5 — Scaffold (when required)

Scaffold when: greenfield, ask spans multiple themes/modules, no scaffold in `.context/`, or large / complex delta. Otherwise skip to Step 1.

A scaffold is a minimum-resolution skeleton of the **whole** design — shape without detail. Scaffold **every active perspective**, not just Stories.

| Perspective | Scaffold contains | NOT scaffold |
|---|---|---|
| **Stories** | Epics, sub-epics, story spine (names only) | Scenarios, Given/When/Then |
| **BDD** | Top-level `describe` blocks only | `that…` / `with…` / `it should…` |
| **Modules** | Module folder names only | Operations, properties, interactions |
| **UX** | Site map / navigation only | Screens, controls, layouts |

Ask up to 3 overarching questions before sketching — **one at a time** using the `AskQuestion` tool. Wait for the user's answer before asking the next. After the last answer is received, sketch the full scaffold immediately. Mark every scaffold line with `< scaffold`.

> **HARD RULE:** One question per turn via `AskQuestion`. Never batch two questions in one message or one tool call.
> **HARD RULE:** Questions come before the scaffold. Never sketch and ask at the same time.

> **HARD RULE:** Never scaffold and detail in the same pass. Scaffold the whole first, then go deep on one theme.
>
> **HARD RULE:** Never write `I{Type}` interfaces in a Modules sketch — modules and concrete class names only.

---

## Step 1 — Choose one theme and one template

A theme is any cohesive thing you're solving: a feature, user need, behavior, screen, module, or a formal artifact (epic, sub-epic, increment). Match theme size to app size — top-level epic for medium apps, sub-epic for large.

Templates live in `templates/`: `bdd-template.md`, `modules-template.md`, `stories-template.md`, `ux-template.md`. If the user brings their own format, save it to `templates/` under a chosen name.

> **HARD RULE:** One theme per loop. Never work multiple themes at once.

> **HARD RULE:** Read every active perspective's template file in full before writing a single sketch line. The template IS the format law — notation, structure, and fidelity rules all come from there. Never invent alternative formats (no "As a / I want / so that", no "describe:" prefixes, no YAML comment blocks, no plain ASCII boxes without the required `Stories (~N):` / `key:` annotations).

---

## Step 2 — Grill inside the sketch loop

Follow `grill-me.md`: find context, read it, cite it, then validate or ask.

> **HARD RULE:** After 1–2 resolved questions, stop and sketch. Three questions with no sketch is already too many. A sketch with `?` placeholders beats more questions.

---

## Step 3 — Draft

Draft a rough shape from the template. Use `?` for unresolved parts. For every active perspective, expand only the parts this theme touches — all sibling entries across Stories, BDD, Modules, and UX stay `< scaffold`.

> **HARD RULE:** No fidelity tags (`<-i`, `<-m`, etc.) on sketch lines. Scaffold lines get `< scaffold`; everything else is plain.

> **HARD RULE:** The scaffold IS the sketch — one file, one structure. When detailing a theme, replace its `< scaffold` entries in-place with real content. Never create separate `## THEME N` sections below the scaffold. There is no "scaffold block" plus "theme blocks" — only the scaffold, evolving.

---

## Step 4 — Present and persist

Show the sketch in chat with 1–2 sentences per decision.

> **HARD RULE:** Save immediately to `{destination}/.context/{slug}-sketch.md` — create `.context/` if missing. A sketch that lives only in chat is a defect. Overwrite on every regeneration.

Engagement sketches → `{session}/.context/{slug}-sketch.md`. Module sketches → `{session}/{module}/.context/{slug}-sketch.md`.

---

## Step 5 — Extend the next branch

Pick one unresolved branch. Sketch your recommended shape, explain why in 1–2 lines, wait for feedback.

---

## Step 6 — Check agreement across perspectives

Does the current sketch raise questions another lens would answer? If yes, sketch the same theme from that lens in the same file.

> **HARD RULE:** Do not move on while active perspectives conflict. Refine until they agree.

---

## Step 7 — Refine and repeat

Regenerate the sketch, save again, re-check agreement. Repeat Steps 3–7 until the theme is stable and all active views agree. Then pick the next theme or deepen fidelity.
