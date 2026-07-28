# Grill-Me: Context-Aware Questioning

Interview me relentlessly about every aspect of this design until we reach shared understanding. Walk down each branch of the design tree, resolving one decision at a time. For each question, provide your recommended answer.

Ask questions one at a time, waiting for feedback before continuing.

If a question can be answered by exploring the codebase or existing context files, explore first instead of asking.

---

## Context awareness

During exploration, look for existing context files. Most projects keep context close to what it describes.

### File structure

Most repos have a single project-level context:

```
/
├── .context/
│   ├── grill-answers.md
│   ├── some-theme-sketch.md
│   ├── module-context.md
│   └── decisions/
│       ├── 0001-payment-owns-retry.md
│       └── 0002-cart-delegates-subtotals.md
└── src/
```

If a module owns its own context, it lives beside the code:

```
/
├── .context/                             ← project-wide context
├── src/
│   ├── payment/
│   │   └── .context/                     ← module-specific context
│   │       └── module-context.md
│   └── cart/
│       └── .context/
│           └── module-context.md
```

Context can also live outside `.context/` folders — any markdown, any file with "context" in the name, any prior sketch or handoff doc. Read what looks relevant before concluding nothing exists.

Create files lazily — only when you have something to write. If no `.context/` exists, create one when the first decision is captured. If no `.context/decisions/` exists, create it when the first CDR is needed.

---

## Before you ask anything

### Read context first

Look for `.context/` folders, files with "context" in the name, any markdown that looks like prior decisions or sketches. Read them. You must be able to cite specific terms and paths from what you read before you open your mouth.

"I read `payment/module-context.md` — it defines PaymentGateway with a `process` operation that takes an amount. No mention of retry behavior."

If you can't point to something concrete you read, you haven't done this step yet.

### If context already answers the question — validate, don't ask

When you find the answer in existing files, confirm it instead of treating it as open:

"Based on `module-context.md`, Cart owns `add_item` and `calculate_total`. Should I proceed with that understanding, or has it changed?"

---

## When you do need to ask

### Frame with evidence

Name the branch being decided, state what's already agreed, cite what you read, and ground the decision in the active perspective's concepts. Then present 3-5 options with the recommended one first. Always end with "Other / I'll specify."

"I read `module-context.md` — Cart lists `add_item` but doesn't mention how items are stored internally. We've agreed Cart owns `add_item`. The open question is whether items should be a separate CartItem class or inline tuples.

1. (Recommended) CartItem class — cart doesn't own per-item subtotal math
2. Inline tuples — simpler but cart must compute everything
3. Other / I'll specify"

### Challenge against existing context

When the user uses a term that conflicts with what's already in context files, call it out immediately. "Your module-context defines 'payment' as the gateway call, but you seem to mean the entire checkout flow — which is it?"

### Sharpen fuzzy language

When the user says something vague or overloaded, force precision. "You're saying 'process the order' — do you mean validate it, charge payment, or dispatch it? Those are three different operations."

### Stress-test with scenarios

When a structure or decision is on the table, invent a scenario that probes its edges. "What happens if the payment gateway times out halfway through? Does the cart hold state, or does the gateway own the retry?"

BDD and user stories scenarios are the go-to form for capturing these — they force concrete actors, actions, and outcomes rather than abstract descriptions. Sketch them inline as decisions crystallise.

### Cross-reference with code

When the user states how something works, check whether existing context or code agrees. If you find a contradiction, surface it: "Your sketch shows Cart calling PaymentGateway directly, but `module-context.md` says Cart depends only on PaymentGateway through `process` — which is right?"

---

## After each resolved decision

Record it immediately to `{session}/.context/grill-answers.md`. 1-3 sentences, reference paths. Don't batch — capture as decisions happen.

### Offer CDRs sparingly

A CDR (Context Decision Record) lives at `.context/decisions/` and captures a design decision worth remembering. Only offer to create one when all three are true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip it. Format:

```markdown
# {Short title of the decision}

## Status
Accepted

## Context
{Why this decision came up — 2-3 sentences}

## Decision
{What was decided — 1-2 sentences}

## Consequences
{What follows from this — good and bad}
```

Create `.context/decisions/` lazily — only when the first CDR is needed.

---

## What not to do

- Ask without having read context first
- Present bare options with no framing or evidence ("Should we use A or B?")
- Batch multiple unrelated questions in one turn
- Ask something context already answers
- Go more than two questions without sketching something
