# Guardrail: Handling Incomplete or Misleading Papers

## Reality check

Many papers are not written for operational decision-making. They are written to present a method, not to answer whether the method belongs in a client-deliverable translation workflow.

Your job is not to rescue the paper. Your job is to assess it honestly.

---

## Common failure patterns

### Missing evaluation detail

The paper reports a metric gain but does not specify:

- prompt format
- preprocessing
- decoding settings
- dataset version
- human evaluation rubric

Treat this as a reproducibility and confidence problem, not a minor omission.

### Weak domain match

The paper works on:

- general web data
- short sentence benchmarks
- English-only tasks
- synthetic evaluation

This may still be useful, but it should usually lower confidence for the translation-pipeline.

### Vague workflow claims

Papers often say they improve:

- consistency
- faithfulness
- controllability
- terminology

If the measurement behind that claim is unclear or indirect, mark the claim `PARTIAL` or `UNKNOWN`.

### Hidden operational cost

Some papers look attractive until you notice they require:

- retrieval infrastructure
- annotation pipelines
- large teacher models
- custom datasets
- unavailable human review resources

Call those out directly.

---

## Decision rules for incomplete papers

### If the core claim is underspecified

- keep the contribution summary
- downgrade confidence
- prefer `WATCHLIST` or `REJECT` unless a very small prototype can answer the unknown

### If evaluation is weak but the idea is still plausible

- use `PROTOTYPE` only if the prototype is narrow
- make the key unknown the centerpiece of the experiment

### If the paper depends on inaccessible assets

Examples:

- private prompts
- private datasets
- unavailable code
- missing preprocessing scripts

Record them as blockers. Do not write around them.

---

## Official code handling

Official code can help answer:

- whether the implementation exists
- what framework or tooling the authors used
- whether prompts/configs are partially available

Official code does not fix:

- weak domain fit
- unconvincing evaluation
- absent operational constraints

Use it to sharpen the audit, not to excuse the paper's gaps.

---

## When to reject

Reject when one or more of these are true:

- the paper never gets close to the translation-pipeline use case
- the paper's evidence is too thin to justify engineering time
- the method requires a structural rewrite for uncertain upside
- the main claimed benefit cannot be measured in this workflow

Rejection is a successful outcome if it saves time.
