# Stage 3: Evidence and Risk Audit

## Purpose

The main failure mode in research triage is not wrong code. It is **overstating applicability**. This stage audits what the paper actually proves, what it leaves unclear, and what would make it risky for the translation-pipeline.

## Input

- Parsed paper sections from Stage 1
- Contribution analysis from Stage 2
- `paper_metadata.json`
- official code repository, if found

## Output

- `.paper2code_work/{ARXIV_ID}/evidence_audit.md`

---

## Critical mindset

Assume the paper is narrower than its abstract makes it sound until the evidence proves otherwise.

Leaderboard gains, polished prose, and official code do not by themselves justify a pipeline change.

---

## Reasoning protocol

### Step 1: Audit what the paper actually evaluates

For every core claim, capture:

- what the claim is
- what evidence supports it
- whether the evidence is direct, partial, or absent

Use these evidence states:

- `SUPPORTED` — clearly demonstrated in the paper
- `PARTIAL` — some evidence exists, but not enough for this use case
- `UNKNOWN` — not shown
- `CONTRADICTED` — evidence in the paper or setup suggests the opposite

### Step 2: Audit dataset and domain fit

Check:

- Which languages are used?
- Are the tasks sentence-level, segment-level, or document-level?
- Is the domain legal, regulatory, technical, general web, or mixed?
- Are there document-structure constraints, tables, tags, forms, or bilingual layouts?
- Is there any evidence relevant to Icelandic or similarly low-resource, morphologically rich settings?

If the data is far from legal/regulatory document translation, mark that explicitly as a fit risk.

### Step 3: Audit metrics fit

For each metric the paper uses, answer:

- Does this metric tell us anything useful for client-deliverable translation?
- Does it measure adequacy, terminology, consistency, or formatting fidelity?
- Is there human evaluation, or only automatic metrics?
- Are the reported gains large enough to matter operationally?

Be skeptical of papers whose evidence rests entirely on metrics that do not track your actual quality bar.

### Step 4: Audit operational assumptions

Look for assumptions about:

- model size or hardware
- retrieval infrastructure
- latency or throughput
- annotation needs
- external datasets or proprietary resources
- prompt access or unavailable system details
- human review burden

Anything that would materially complicate the current workflow should be recorded as a deployment risk.

### Step 5: Audit reproducibility

Check:

- Is there official code?
- Are the datasets accessible?
- Are prompts, preprocessing, and evaluation details included?
- Are the main ablations understandable?
- Could you rerun or prototype the core idea without rebuilding the pipeline?

If official code exists, note what it resolves and what it does not resolve.

### Step 6: Audit direct pipeline risks

Evaluate paper fit against translation-pipeline realities:

- terminology control
- source/target number integrity
- memoQ tag safety
- bilingual `.docx` handling
- human gates
- legal/regulatory style constraints
- deterministic validation needs

If the paper ignores these concerns, that is not fatal, but it is a reason to downgrade confidence.

### Step 7: Write the audit

Save this structure to `.paper2code_work/{ARXIV_ID}/evidence_audit.md`:

```markdown
# Evidence Audit: {paper_title}

## Claim audit

| Claim | Evidence state | Evidence from paper | Why it matters here |
|------|----------------|---------------------|---------------------|
| {claim} | {SUPPORTED/PARTIAL/UNKNOWN/CONTRADICTED} | {table/section/quote} | {note} |

## Dataset and domain fit
- Languages:
- Granularity:
- Domain:
- Relevance to IS->EN legal/regulatory translation:

## Metrics fit
- Metrics used:
- Human evaluation:
- What these metrics do not tell us:

## Reproducibility
- Official code:
- Data availability:
- Prompt/preprocessing detail:
- Main missing details:

## Operational risks
1. {risk}
2. {risk}
3. {risk}

## Unknowns that block confident adoption
1. {unknown}
2. {unknown}

## Bottom line
- Strongest reason to care:
- Strongest reason to doubt:
- Confidence level: {high | medium | low}
```

### Step 8: Self-check

Before proceeding:

- Did you separate paper evidence from your own inference?
- Did you downgrade claims that only hold on weakly related datasets?
- Did you avoid treating official code as proof of production fit?
- Did you record missing prompt, preprocessing, or evaluation details as real unknowns?
