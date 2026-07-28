# Stories Sketch Template

**Context perspective**: Stakeholder interactions and end-to-end flows

## Template Structure

Use this for sketching user stories and acceptance scenarios.

```
# {Epic verb-noun}
* approx N–M total stories

## {Sub-epic verb-noun}

### {Actor} --> {Confirming story verb-noun}
given {shared setup}                    // specification only

#### {main scenario name}
given {precondition with object.field}
    and {precondition with object.object.field}
when {action}
    and …
then {observable outcome {object.field=descriptive term}}
    and …

#### {next scenario name}                    // specification only
…

### {Actor} --> {Confirming story verb-noun}
* approx N–M more stories (what unmapped work likely includes)

## {Sub-epic verb-noun}
* approx N–M more stories

~> Increment 1: {capability outcome}: {Story}, {Story}, …
```

## Example

# Manage Customer Orders
* approx 18-22 total stories

## Place New Order

### Customer --> Browse Product Catalog

#### browse catalog shows available products
given a Catalog with published Products
    and a Customer with an empty Cart
when the Customer browses the Catalog
then available Products are listed with price
    and Product.name and Product.price are shown

### Customer --> Submit Order
given a Cart with line items and a Payment Method

#### order accepted for valid cart and payment
given a Cart with Items totalling amount.currency
    and a Payment Method with status authorised
when the Customer submits the Order
then an Order is created with status placed
    and an Order.number is returned

#### order rejected when payment declined
given a Cart with Items totalling amount.currency
    and a Payment Method with status declined
when the Customer submits the Order
then the Order is rejected with reason payment_declined
    and the Cart contents are preserved

* approx 4-5 more stories (cart, address, delivery, review)

## Track Order Status
* approx 3-4 more stories (pending, shipped, delivered)

## Cancel Order

### Customer --> Request Order Cancellation
* approx 2-3 more stories (refund, partial cancel, policy)

~> Increment 1: Customer can place a paid order: Browse Product Catalog, Submit Order

## Critical Stories Rules

- **`verb-noun-format`** — Name Epic/SubEpic/Story as verb–noun; actor is metadata.
- **`four-to-nine-children`** — 4–9 direct children (warn at 3/10; error ≤2 or ≥11).
- **`behavioral-observable-outcomes`** — Name and Then in domain-observable terms; never internals.
- **`branch-on-mechanical-uniqueness`** — Branch on distinct mechanics. Different requirement entries with same mechanic = one story with different examples/scenarios.
- **`vocabulary-traces-to-domain-source`** — Trace terms to domain language/model when present.
- **`read-all-source-context-in-full`** — Before locking hierarchy and before any question about a seam, prove-read every relevant context: `*-segment.md`, `module-context.md`, session sketches, grill-answers, build-order, cited paths. Index stubs are structure hints only — not story inventory.
- **`right-size-story-nodes`** — One demonstrable interaction per story.

## Stories Notation

- `#` = Epic
- `##` = Sub-epic
- `###` = `{Actor} --> {Verb Noun}` = story
- `####` = scenario name
- `given / when / then / and` = regular body text (no heading)
- `* approx N–M …` = unmapped work
- `~>` = increment
- `//` = note

## Fidelity Guidance

**Do not** tag lines with fidelity markers. Depth is what you fill:

| Fidelity | Fill |
|---|---|
| **discovery** | Epic / SubEpic / named stories + thin-slice; clear approx gaps as you name stories |
| **exploration** | Main-flow Given / When / Then under each confirming story; objects from ExampleFactory fakes; assert public interface. No shared background yet. |
| **specification** | Extra scenarios, shared setup / background; still fake + public interface; values from factories |
| **engineering** | Which tier(s) (`isolated` / `production`); not full impl in the sketch |

## Important Notes

- **MUST**: Read all source context in full before drafting or refining.
- **MUST**: Branch on **mechanical uniqueness** only — split distinct mechanics; do not mint one story per TOC / catalog / requirements row.
- Keep unmapped areas in the sketch as `* approx N–M stories…` lines — not in a separate outline map.
- Discovery materializes named stories; drop approx lines once those stories are named.
- **Order**: epics → sub-epics → confirming stories + approx gaps → thin-slice order → main-flow scenario → variations / shared setup (`specification`) → tier notes (`engineering` only).
