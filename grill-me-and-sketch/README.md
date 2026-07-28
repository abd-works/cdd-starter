# grill-me-and-sketch

> [See the skill in action →](https://www.youtube.com/watch?v=GZM7r8wbI6M)

**The problem.** Agents jump to answers too fast. You describe something loosely, the agent fills in the gaps with its own assumptions, and by the time you see the code you're realize the AI was misaligned on fundamentals — structure, scope, intent. Matt Pocock's [grill-with-docs](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md) skill is a huge helps — but freeform questions and answers in text often don't provide the right level of abstractions to reason deeply about your problem or solution. You oten want to *see* what the agent thinks it's going to build.

**Why grill-me-and-sketch?** AI grills you, then confirms it's understanding by sketching one or models representing a solution perspective, leading to an increase in *understanding* between human and agent. 

- **Higher-level abstractions first.** Work at the level of concepts, modules, and flows before touching code. Catch wrong shapes early, not after they're built.
- **Confirmed understanding through models.** The agent externalises its reasoning as sketches — stories, module boundaries, BDD scenarios, UX flows. You can see where it's right and where it's missing the point.
- **Fundamental gaps surface fast.** A scaffold across all four perspectives makes contradictions obvious. If the stories don't match the modules, you know before writing a line.
- **Scales to teams and larger problems.** Sketches are shareable artifacts, not just chat history. They give contributors a common reference point to reason from and challenge.

---

## When to use it

- Starting a new feature or system from scratch
- Exploring options before deciding on a design
- Aligning on shape across multiple perspectives before writing specs

---

## How to trigger it

Just ask the agent in natural language:

```
/grill-me-and-sketch [what you're designing]
```

Or describe what you're working on and the agent will pick it up.

---

## What happens

1. **Confirm perspectives** — Agent asks which lenses to apply (Stories, Modules, BDD, UX). All four are on by default; deselect any you don't need.
2. **Scaffold** — A skeleton of the whole design, shape without detail.
3. **Theme loop** — One theme at a time: the agent grills you on open questions, drafts a rough sketch, saves it, then moves to the next branch.
4. **Persist** — Every sketch is saved to `.context/{slug}-sketch.md` in your session folder.

---

## Files in this skill

| File | Purpose |
|---|---|
| `SKILL.md` | Full step-by-step instructions the agent follows |
| `grill-me.md` | Grilling protocol — how the agent finds and validates context |
| `templates/bdd-template.md` | BDD sketch format |
| `templates/modules-template.md` | Module boundary sketch format |
| `templates/stories-template.md` | Epic/story sketch format |
| `templates/ux-template.md` | Screen and navigation sketch format |

---

## Output

Sketches land in `.context/` next to your session folder. They evolve in-place — one file per design, overwritten on each refinement pass.

---

## Attribution

Inspired by and makes use of [grill-with-docs](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md) by [Matt Pocock](https://github.com/mattpocock) — a relentless interview skill that sharpens plans and designs while producing ADRs and a glossary as you go. Thanks, Matt.
