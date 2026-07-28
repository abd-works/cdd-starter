# BDD Sketch Template

**Context perspective**: Observable behavior and verification

## Scaffold Level (use this first for new or large asks)

Scaffold = top-level subject blocks only. Mark every scaffold line with `< scaffold`.
Do NOT write `that …` / `with …` / `it should …` at scaffold level.

```
# {domain subject or state}                               < scaffold
# {domain subject or state}                               < scaffold
# {domain subject or state}                               < scaffold
```

Pick ONE subject as the active theme. Remove its `< scaffold` marker and fill in the full tree below.

---

## Template Structure

Use this for sketching behaviors for the active theme only.

```
# {subject — domain thing, state, or observable condition}
  ## that {event or condition that sets the subject up}
    with {narrower condition}
      it should {observable outcome}
```

**Read top-down as a usage/storytelling sequence**: what the user or system does first, then what is true, then what is observed.

## Example

```
# an action that is annotated with log
  ## that is invoked
    it should record a run event on the session trail
  ## that has been logged
    with no session name given
      it should use the default session
    with a given session name
      it should keep events under that session
    with verbose off
      it should write a summary line and keep the last payload
      with full logging requested
        it should flush the last payload
    with verbose on
      it should write payload files for later events

# an action that is not annotated
  ## that is invoked
    it should leave the session trail empty
```

## Critical BDD Rules

- **`observable-behavior`** — Prove what a stakeholder can verify without reading code (return value, state, public effect). Never internals.
- **`domain-practice-alignment`** — Subject names must match domain language exactly.
- **`usage-order-behaviors`** — Order as a usage story (what happens first → next). Not by implementation layer.
- **`subject-is-not-internal`** — A `#` subject is a domain thing, state, or condition — never a manager, hub, runner, service.
- **`subject-is-plain-english`** — Full English phrases (e.g. "an action that is annotated with log"). Never symbol names (`"@log marker"`).
- **`state-not-when`** — Never use `when` for state. Use `## that …` for events/conditions and `with …` for standing conditions.
- **`nest-by-enabling-events`** — Each nested `that`/`with` must be a real precondition required for the nested `it should`.

## BDD Sketch Anti-Patterns (Never Do This)

❌ `@log marker` — mechanism/symbol, not a subject  
❌ `ToolsetRunner` — manager/internal  
❌ `when no session name is given` — never "when" for state; use "with …"  
❌ Splitting the same subject into multiple `#` blocks

## Line Naming Guide

| Line | Names | Never names |
| --- | --- | --- |
| **`#` subject** | Subject under observation in plain English (thing, state, condition) | Manager / hub / runner / service / internal class; decorator symbol (`@log`); marker name |
| **`## that …`** | Past or present event/condition on that subject (`that has been logged`, `that is invoked`) | `when …` |
| **`with …`** | Narrower standing condition (`with no session name given`, `with verbose off`) | `when …`; implementation knobs phrased as API flags |
| **`it should …`** | One stakeholder-visible outcome | Internals, private fields, call counts on mocks of the subject |
