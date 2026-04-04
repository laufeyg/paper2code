# Guardrail: Scope Enforcement

## Purpose

This fork produces research decisions, not speculative build-outs.

The goal is to decide whether a paper belongs in the translation-pipeline roadmap, the vault, a prototype queue, or nowhere at all.

---

## The decision tree

For each paper, walk through this tree:

```
Does the paper influence a concrete translation-pipeline surface?
├── NO → WATCHLIST or REJECT
│
└── YES →
    Is the evidence strong enough for this use case?
    ├── YES →
    │   Is the required change narrow and operationally cheap?
    │   ├── YES → ADOPT NOW
    │   └── NO  → PROTOTYPE
    │
    └── NO →
        Can a bounded experiment answer the open question?
        ├── YES → PROTOTYPE
        └── NO  → WATCHLIST or REJECT
```

---

## In scope

This fork should produce:

- contribution analysis
- evidence audit
- concrete pipeline-fit mapping
- bounded experiment design
- vault-ready note

## Out of scope

This fork should not:

- generate production code
- design a greenfield architecture for the whole pipeline
- recommend vendor switches without paper-backed justification
- convert every interesting paper into a roadmap item
- hide weak fit behind vague future possibilities

---

## Preferred experiment sizes

Good experiment scopes:

- one prompt variation
- one validator prototype
- one terminology intervention
- one chunking/layout heuristic
- one offline comparison against an existing fixture set

Bad experiment scopes:

- rewrite the orchestration layer
- replace the whole translation engine
- rebuild bilingual memoQ handling
- redesign all human gates

---

## How to handle weak fit

If the paper is interesting but not actionable:

- use `WATCHLIST` if it may matter later
- use `REJECT` if the mismatch is structural

Do not create a prototype plan just to avoid saying no.

---

## Translation-pipeline bias

Favor:

- iterative improvements over big rewrites
- decisions that preserve human gates
- approaches compatible with termbase and validator workflows
- experiments that can run on existing artefacts and fixtures

Be suspicious of:

- ideas that require huge new datasets
- methods that ignore document structure or post-editing realities
- papers that optimize for benchmark elegance over operational usefulness

---

## Documentation rule

Every recommendation must name:

- the exact pipeline stage or module it touches
- the smallest experiment worth running
- the main blocker or reason not to proceed

If you cannot do that, the paper is not ready for action.
