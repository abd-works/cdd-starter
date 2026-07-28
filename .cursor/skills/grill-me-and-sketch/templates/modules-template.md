# Module Sketch Template

**Context perspective**: Structure, modularity, and object relationships

## Scaffold Level (use this first for new or large asks)

Scaffold = module folder names only (optional short purpose comment). Mark every scaffold line with `< scaffold`.
Do NOT define operations, properties, interactions, or `I{Type}` interfaces at scaffold.

```
# {module}/                                               < scaffold  // optional purpose
# {module}/                                               < scaffold
# {module}/                                               < scaffold
```

Pick ONE module as the active theme. Remove its `< scaffold` marker and fill in classes below.

---

## Template Structure

```
# {module}/
  ## ClassName : BaseClass
    propertyName
    operationName param param
         nestedThing                    ← owned class
         nestedOperation param
    -> collaborator.operation           ← real call
    -> super.operation                  ← base operation
    // invariant or sequencing note

  ## AnotherClass
    property
    operation param
    -> ClassName.operation
```

Every class in a module is a `##` — there is no primary class. `----` is not used.

---

## Module Nesting Example

When children share a base seam, nest them:

```
# powers/                              ← parent sub-system (shared seam)
  ## Effect                            ← shared base owned by the module
  ## Attack : Effect
  ## Control : Effect
  ## Defense : Effect

# conflicts/
  ## Turns
  ## Actions
  ## Conditions

# gear/
  ## Equipment
  ## Headquarters
  ## Vehicles

# checks/
# abilities/
```

**Hard rules:** nest only when there is a **shared base** or clear sub-system; children implement independently with siblings stubbed; shared mechanics live once under the parent module, not copy-pasted.

---

## Legend

| Symbol | Meaning |
|---|---|
| `# module/` | A module — a folder owning a cohesive domain concept |
| `## ClassName` | A class within the module — all classes are equal peers |
| `## ClassName : BaseClass` | Subtype; record only the delta from the base |
| `propertyName` | Something the class holds (noun phrase) |
| `operationName param` | Something the class does (verb phrase); trailing tokens are parameters |
| indent | Ownership / composition / subordination |
| `-> collaborator.operation` | Interaction — real operation on a property, peer, or `super` |
| `// …` | Invariant or sequencing note |

---

## Critical Modules Rules

- **`high-cohesion`** — Classes inside a module share a common purpose and domain concept.
- **`low-coupling`** — Modules depend only through small, clear public operations on classes — not through inventing `I{Type}` in sketches.
- **`deep-module`** — Small public surface, large hidden implementation. Max 40% of symbols public.
- **`single-boundary`** — Each module is the single source of truth for its domain concept.
- **`physical-folder`** — Each module occupies its own folder with `.context/module-context.md`.
- **`cohesive-file`** — One file per class family (primary type + subtypes + tightly connected peers).
- **`nested-physical-folder`** — Child module path is `{parent}/{child}/`. Parent owns shared base.
- **`shared-base-before-siblings`** — Extract parent-owned base types; children depend on base, not siblings.
- **`prefer-real-calls`** — Write `-> opposingTrait.resolve`, not placeholder helpers.

## Interaction Rules (Read These)

- **Prefer real calls**: Write `-> opposingTrait.resolve`, `-> cart.addItem`, `-> super.resolve`
- **Do not invent underscore placeholders** as default (`_opposing_roll`, `_private_helper`). Those hide the design.
- **`-> ClassName` alone is not an interaction.** Point at an operation or property read.
- Show only interactions that clarify collaboration; suppress incidental helpers.

## Example Factory Pattern (formal / engineering fidelity only)

**Informal sketches:** name concrete classes only (`Cart`, `Product`). Do **not** add `I{Type}` lines.

When generating formal artifacts / story-test factories later:

```
# {family}.{ext}
## {Type}

# {type}_example_factory.{ext}      // ALWAYS separate
## {Type}ExampleFactory
  {example_method}(mode)
    // loads examples[{example_key}] -> {Type}, {OtherType}, …
    // Fake | Isolated | Production are modes (not subclasses)
```

### Generation Modes

| Mode | When used | How built |
|---|---|---|
| **Fake** | explore / spec default | Mocking framework / fake of `{Type}`; feed `examples[{example_key}]` |
| **Isolated** | story-test tier | `new {Type}(...mocks/stubs via constructor injection...)` |
| **Production** | story-test tier | `new {Type}(...real collaborators...)` |
