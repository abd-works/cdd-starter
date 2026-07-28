# UX Sketch Template

**Context perspective**: Navigation, visual layout, and user perception

## Scaffold Level (use this first for new or large asks)

Scaffold = site map / navigation only. Mark every scaffold line with `< scaffold`.
Do NOT draw screen boxes, controls, or layouts at scaffold level.

```
{Screen name}                                             < scaffold
  ├─ [{nav_type}] {action} ──→ {Destination screen}      < scaffold
{Screen name}                                             < scaffold
  └─ [{nav_type}] {action} ──→ {Destination screen}      < scaffold
```

Pick ONE screen as the active theme. Remove its `< scaffold` marker and draw the full screen box below.

---

## Template Structure

Use this for sketching navigation and screen layouts.

```
Fidelity: ia | mockup | specification | code

═══════════════════════════════════════════════════════════
  SITE MAP
═══════════════════════════════════════════════════════════

{Screen name}
  ├─ [{nav_type}] {action} ──────────→ {Destination screen}
  └─ [{nav_type}] {action} ──────────→ {Destination screen}

{Screen name}
  └─ [action] {action} ──────────────→ {Destination screen}

Nav tags: [Quick Action] · [top nav] · [drawer nav] · [secondary nav] · [action] · [system]

═══════════════════════════════════════════════════════════
  SCREENS
═══════════════════════════════════════════════════════════

[ {screen name} ]                                    {layout}
  ┌─────────────────────────────┐
  │ {region}                    │
  │ {field} · {field}           │  — representative row
  │ [ Create ] [ Delete ]       │  — verb row
  │ name [____________]         │
  │ kind [ Model      ▾ ]       │
  │ [x] active   [ ] default    │
  │ › selected row ‹            │
  │ (dim) disabled action       │
  │ ! validation feedback       │
  └─────────────────────────────┘
  Stories (~N): {Story} · {Story}
  Domain terms: {term} · {term}
  key:
    [____] text · [▾] dropdown · [x]/[ ] check · [ btn ] button
    ›sel‹ selected · (dim) disabled · ! error
    on [ Edit ] → {destination or effect}
    // stub/brand notes (specification only)
```

## Example

```
Fidelity: ia

═══════════════════════════════════════════════════════════
  SITE MAP
═══════════════════════════════════════════════════════════

character sheet — abilities
  ├─ [action] edit ──────────────────→ ability editor
  ├─ [action] selects Identities tab → character sheet — identities
  └─ [action] selects Movements tab ─→ character sheet — movements

ability editor
  └─ [action] save ──────────────────→ character sheet — abilities

═══════════════════════════════════════════════════════════
  SCREENS
═══════════════════════════════════════════════════════════

[ character sheet — abilities ]                 left panel + body
  ┌────────────────┬────────────────────────────┐
  │ ▼ All chars    │ Identities                 │
  │   ▶ Crowd 1    │ [ Abilities ]              │  inactive greyed
  │   › Char A ‹   │ Movements                  │
  │   Char B       ├────────────────────────────┤
  │                │ ability · rank · key       │
  │                │ › Strike · 3 · Q ‹         │
  │                │ Guard · 2 · E              │
  │                │ [ Create ] [ Delete ] [ Edit ]
  └────────────────┴────────────────────────────┘
  Stories (~4): Update Ability Rank · Create Ability · Delete Ability · Set Key
  Domain terms: ability · ability rank · activation key
  key:
    tree · list rows · [ btn ] button bar
    ▼/▶ expand · ›sel‹ selected
    on [ Edit ] → ability editor

[ ability editor ]                              modal dialog
  ┌─────────────────────────────┐
  │ ability name                │
  │ name [ Strike_________ ]    │
  │ rank [ 3 ▾ ]  key [ Q__ ]   │
  │ [x] persistent              │
  │ [ Save ] [ Cancel ]         │
  │ ! rank must be 1–10         │
  └─────────────────────────────┘
  Stories (~2): Update Ability Rank · Toggle Persistence
  Domain terms: ability rank
  key:
    [____] text · [▾] dropdown · [x] check · [ btn ] button
    ! error
    on [ Save ] → character sheet — abilities (update rank)

[ character sheet — identities ]     [ character sheet — movements ]
  ┌──────────┬────────────────┐        ┌──────────┬────────────────┐
  │ (dim)    │ [Identities]   │        │ (dim)    │ Identities     │
  │ tree     │ Abilities      │        │ tree     │ Abilities      │
  │          │ Movements      │        │          │ [Movements]    │
  │          ├────────────────┤        │          ├────────────────┤
  │          │ identity row   │        │          │ movement row   │
  │          │ [ Add ][ Remove ]       │          │ [ Add ][ Remove ]
  └──────────┴────────────────┘        └──────────┴────────────────┘
  chrome: same as character sheet — abilities
  key: (dim) = shared chrome

// context: rank update must leave sheet consistent
```

## Critical UX Rules

- **`tab-states-are-separate-screens`** — N tabs → N screens; chrome shared via `chrome_of`.
- **`screen-story-budget`** — ~4 user stories per screen; more signals missed decomposition.
- **`screen-names-use-domain-terms`** — Screen labels trace to domain language when it exists.
- **`ia-named-regions-only`** — At IA fidelity, regions are named slots; no control detail yet.
- **`controls-match-interaction-decisions`** — Exact control types; no invented affordances.

## UX Discipline

- No toolbar dumps, autocomplete, or copy walls
- ~4 user stories per screen
- Tab states are separate boxes; sibling chrome dimmed / `chrome: same as …`
- Pick from layout catalog below

## Layout Catalog

Choose an appropriate layout for each screen based on its purpose:

### Navigation Layouts
- **sidebar** — Navigation panel + main body (e.g., left nav + content area)
- **tabbed** — Tab bar + body (switching between views)
- **rail-navigation** — Compact rail + body (icons with labels)
- **top-header** — Logo + nav links + profile + body (horizontal nav)

### Structure Layouts
- **holy-grail** — Header + nav + body + aside + footer (classic web layout)
- **split-screen** — Left + right (side-by-side content)
- **dashboard** — Header + nav + main (analytics/monitoring screens)

### Content Layouts
- **stack** — Rows (vertical list of items)
- **accordion** — Section headers + expanded content (collapsible sections)

### Utility Layouts
- **form** — Body only (data entry)
- **modal** — Body (dialog/popup)
- **flyout** — Body + panel (slide-out panel)
- **wizard-stepper** — Step bar + body + back + continue (multi-step forms)
- **search-filter** — Search bar + filters + results (search interfaces)

**Usage**: Simply name the layout after the screen name in your sketch. The layout defines which named regions the screen will have.

Example:
```
[ shopping cart review ]                         split-screen
  ┌─────────────────┬─────────────────┐
  │ left: cart      │ right: summary  │
  │ items list      │ total           │
  └─────────────────┴─────────────────┘
```

## Control Glyphs

```
[____] text input
[▾] dropdown
[x] checked checkbox
[ ] unchecked checkbox
[ btn ] button
›sel‹ selected item
(dim) disabled/inactive
! error/validation message
```

## Important Notes

- **Sketch order**: site map first (connection tree), then screen boxes with regions/rows/verb rows (`ia`), then visual controls + states inside boxes (`mockup`), then brand/stub notes in key (`specification`), then real frontend/backend wiring (`code`, usually outside this sketch).
- **Do not annotate sketch lines** with fidelity tags (`<-i` / `<-m` / `<-s`). Declare fidelity once at the top.
- **IA discipline**: no toolbar dumps, AC, or copy walls. ~4 user stories per screen.
- **Layouts**: Choose from the layout catalog above. Name the layout after the screen name to indicate which regions it will have.
