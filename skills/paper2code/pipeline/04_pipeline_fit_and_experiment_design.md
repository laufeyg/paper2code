# Stage 4: Pipeline Fit and Experiment Design

## Purpose

Turn the paper analysis into a bounded decision. This stage generates the research pack that says where the paper could fit, how risky it is, and what to test next.

## Input

- Contribution analysis from Stage 2
- Evidence audit from Stage 3
- `knowledge/translation_pipeline_research.md`
- scaffold templates

## Output

- `{paper_slug}/README.md`
- `{paper_slug}/EVIDENCE_AUDIT.md`
- `{paper_slug}/PIPELINE_FIT.md`
- `{paper_slug}/EXPERIMENT_PLAN.md`

---

## Pre-generation checklist

Before writing the final pack, verify:

- [ ] You know the paper's actual contribution
- [ ] You have identified real insertion points
- [ ] You have audited the evidence and risks
- [ ] You have read `guardrails/scope_enforcement.md`
- [ ] You have read `knowledge/translation_pipeline_research.md`
- [ ] You know whether the likely answer is `ADOPT NOW`, `PROTOTYPE`, `WATCHLIST`, or `REJECT`

---

## Recommendation rubric

Use exactly one recommendation:

### `ADOPT NOW`

Only if all of these are true:

- the change is narrow
- the evidence is directly relevant
- the operational cost is low
- the experiment is effectively already decided

This should be rare.

### `PROTOTYPE`

Use when:

- the paper is promising
- there is a plausible insertion point
- evidence is not strong enough for direct adoption
- a bounded experiment can answer the key question

This will be the most common positive outcome.

### `WATCHLIST`

Use when:

- the paper is interesting
- but current fit is weak, timing is wrong, or dependencies are missing

### `REJECT`

Use when:

- evidence is weak for this use case
- the paper ignores key operational constraints
- the required change is too invasive for the likely upside

---

## Fit mapping protocol

### Step 1: Map the paper to concrete pipeline surfaces

For each candidate surface, classify one of:

- `direct candidate`
- `prototype first`
- `not a fit`

Use concrete surfaces such as:

- `src/prompts/outline.py`
- `src/prompts/translate.py`
- `src/prompts/translate_bilingual.py`
- `src/db/termbase.py`
- `src/bilingual/batcher.py`
- `src/bilingual/segment.py`
- `src/validators/`
- Stage 8 QA / pre-delivery validation
- `src/assembly/`
- vault/discovery workflows

### Step 2: Decide the smallest useful change

Examples of acceptable experiment scopes:

- add one validator prototype
- compare one translation prompt variation on one document class
- test one terminology strategy on a narrow corpus
- evaluate one chunking change against reviewer pain points

Avoid proposing large rewrites unless the evidence is unusually strong.

### Step 3: Define success criteria

The experiment plan must state:

- what will be measured
- what counts as success
- what would falsify the idea
- which human gate or reviewer check is required

### Step 4: Identify rollback and safety

Every experiment should answer:

- what existing behavior stays untouched
- how to compare old vs new behavior
- what artefacts or metrics should be captured
- how to stop if quality drops

### Step 5: Write the pack

Generate the final files:

#### `README.md`
An executive brief using the scaffold template. Keep it concise and decision-ready.

#### `EVIDENCE_AUDIT.md`
Promote the Stage 3 audit into a polished artifact. Do not soften uncertainties.

#### `PIPELINE_FIT.md`
Include:

- relevant modules or stages
- likely insertion points
- why each point is or is not a fit
- dependencies or blockers

#### `EXPERIMENT_PLAN.md`
Include:

- experiment name
- hypothesis
- input set
- measurements
- safety checks
- recommendation after experiment

---

## Writing rules

1. Prefer the current pipeline over hypothetical greenfield systems.
2. Tie every recommendation to a current repo surface or explicit workflow step.
3. If the answer is "interesting but not for us," write that plainly.
4. Do not write implementation code in this stage.
5. Keep the pack useful to both engineering work and vault-based research review.

---

## Self-check

- [ ] Every recommendation names a concrete pipeline surface
- [ ] The recommendation level matches the evidence
- [ ] The experiment is small enough to run without a rewrite
- [ ] Success and failure criteria are explicit
- [ ] The pack would still be useful if the recommendation is `REJECT`
