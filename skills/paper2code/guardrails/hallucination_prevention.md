# Guardrail: Evidence Discipline

## The core problem

The dangerous failure mode in this fork is not invented code. It is **invented applicability**.

A paper can be strong and still be a bad fit for the translation-pipeline. A paper can also be weak, incomplete, or only indirectly relevant while still sounding impressive.

This file exists to stop that drift.

---

## Bright-line rule

**If the paper does not show evidence for a claim in a setting meaningfully related to the translation-pipeline, that claim is not established for this workflow.**

Do not upgrade:

- sentence-level evidence into document-level evidence
- high-resource language evidence into Icelandic evidence
- academic benchmark gains into legal/regulatory deliverable gains
- official code into operational readiness

---

## What counts as evidence

### Counts as strong evidence

- evaluation on document translation when the claim is document handling
- human evaluation with clear rubric
- terminology or consistency evaluation when the method claims those gains
- ablations showing the claimed component matters
- official code that aligns with the paper and resolves missing implementation detail

### Does not count as strong evidence

- generic BLEU improvement with no document or domain relevance
- "state of the art" without a meaningful baseline for this use case
- examples cherry-picked in the paper
- official code with no usable evaluation setup
- claims that rely on private data, unpublished prompts, or omitted preprocessing

---

## Inference discipline

You may make inferences, but label them as inferences.

Examples:

- "This might transfer to bilingual memoQ documents because the method is layout-aware" is an inference.
- "This improves memoQ output quality" is not allowed unless the paper shows something close to that.

Whenever you infer, name the missing evidence.

---

## Common traps

### The benchmark trap

The paper reports better COMET, BLEU, or chrF.

Questions you must ask:

- On what domain?
- On what languages?
- Sentence or document level?
- Any human evaluation?
- Any terminology or consistency measurement?

If those answers are weak, downgrade confidence.

### The language-pair trap

Most papers are not about Icelandic. That is fine. But if morphology, terminology, or data scale matter, say the transfer risk out loud.

### The document-fidelity trap

A paper can improve translation quality while being irrelevant to:

- tags
- bilingual alignment
- table structure
- number preservation
- legal formatting

That gap matters here.

### The official-code trap

Official code helps with reproducibility. It does not prove:

- production fit
- domain fit
- evaluator fit
- maintainability in the current pipeline

---

## Prohibited phrasing

Do not write:

- "drop-in improvement"
- "production-ready"
- "clearly better for our pipeline"
- "should generalize"
- "probably works here"

unless the paper actually supports that claim.

---

## Required phrasing for uncertainty

Use direct language like:

- "Not shown in the paper"
- "Unclear whether this transfers to legal/regulatory IS->EN documents"
- "Evidence is sentence-level only"
- "No formatting or tag-integrity evidence"
- "Requires prototype to answer"

---

## Self-audit questions

Before finalizing:

1. Did I confuse a promising idea with a validated fit?
2. Did I upgrade weak domain match into strong relevance?
3. Did I treat unavailable prompts, data, or code as minor omissions when they are actually blockers?
4. Would a skeptical engineer agree that every recommendation is proportional to the evidence?
