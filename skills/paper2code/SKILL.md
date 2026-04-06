---
name: paper2code
description: Converts an arxiv paper into a translation-pipeline research and discovery pack. Trigger when the user runs /paper2code with an arxiv URL or paper ID, asks whether a paper is relevant to the translation pipeline, or wants a paper turned into a decision-ready brief, evidence audit, experiment plan, and vault note.
---

# paper2code — Translation-Pipeline Research Orchestration

You are executing the `paper2code` skill for **translation-pipeline research and discovery**.

Do not treat this as a paper-reproduction workflow. The goal is to help decide whether a paper is worth integrating into the IS->EN legal/regulatory translation pipeline, where it would fit, and what the smallest safe experiment should be.

Execute the stages in order. Do not skip stages.

## Parse arguments

Extract from the user's input:

- `ARXIV_ID`: the arxiv paper ID. Strip any URL prefix.
- `MODE`: one of `minimal` (default), `full`, `educational`.

If the user provided a full URL like `https://arxiv.org/abs/2106.09685`, extract the ID `2106.09685`.
If the user provided a versioned ID like `2106.09685v2`, keep the version.

## Set up working directory

Create a temporary working directory:

```text
.paper2code_work/{ARXIV_ID}/
```

Use it for intermediate artifacts. The final output goes in the current directory under:

```text
{paper_slug}/
```

## Install dependencies

Run via Bash:

```bash
pip install pymupdf4llm pdfplumber requests pyyaml
```

## Execute pipeline

### Stage 1 — Paper Acquisition and Parsing

Read and follow: `pipeline/01_paper_acquisition.md`

Run the helper scripts:

```bash
python skills/paper2code/scripts/fetch_paper.py {ARXIV_ID} .paper2code_work/{ARXIV_ID}/
python skills/paper2code/scripts/extract_structure.py .paper2code_work/{ARXIV_ID}/paper_text.md .paper2code_work/{ARXIV_ID}/
```

Verify the expected outputs exist before continuing.

### Stage 2 — Contribution and Relevance Identification

Read and follow: `pipeline/02_contribution_identification.md`

Before writing anything, also read:

- `knowledge/translation_pipeline_research.md`

Identify what the paper actually contributes, what kind of paper it is, and whether it is directly relevant to the translation pipeline. Save the result to:

```text
.paper2code_work/{ARXIV_ID}/contribution.md
```

### Stage 3 — Evidence and Risk Audit

Read and follow: `pipeline/03_evidence_audit.md`

Before working this stage, also read:

- `guardrails/hallucination_prevention.md`
- `guardrails/badly_written_papers.md`
- `knowledge/translation_pipeline_research.md`

Audit the paper's evidence, evaluation, reproducibility, domain fit, and missing details. Save the result to:

```text
.paper2code_work/{ARXIV_ID}/evidence_audit.md
```

### Stage 4 — Pipeline Fit and Experiment Design

Read and follow: `pipeline/04_pipeline_fit_and_experiment_design.md`

Before generating the final research pack, also read:

- `guardrails/scope_enforcement.md`
- `knowledge/translation_pipeline_research.md`
- relevant files in `scaffolds/`

Generate the final output directory:

```text
{paper_slug}/
```

The default output files are:

- `README.md`
- `EVIDENCE_AUDIT.md`
- `PIPELINE_FIT.md`
- `EXPERIMENT_PLAN.md`

### Stage 5 — Vault Note Packaging

Read and follow: `pipeline/05_vault_note_packaging.md`

Generate:

```text
{paper_slug}/OBSIDIAN_NOTE.md
```

This note should be ready to drop into the vault with light edits.

## Cleanup

Remove the `.paper2code_work/` directory after successful completion.

## Final output

Print a summary:

```text
✓ paper2code research pack complete for: {paper_title}
  Output directory: {paper_slug}/
  Recommendation: {ADOPT NOW | PROTOTYPE | WATCHLIST | REJECT}
  Critical unknowns: {count}
  Suggested experiment count: {count}
  Mode: {MODE}
```

## Mode-specific behavior

- `minimal`: concise decision brief, evidence audit, pipeline fit, and one bounded experiment
- `full`: full research pack with deeper risk analysis, multiple experiments or rollout options when justified
- `educational`: same as `full`, but written so a collaborator can understand the paper and the decision without reading the paper first

## Guardrails — always active

These apply at all stages:

- `guardrails/hallucination_prevention.md`
- `guardrails/scope_enforcement.md`
- `guardrails/badly_written_papers.md`

## Knowledge base — consult as needed

Always read:

- `knowledge/translation_pipeline_research.md`

Use the original domain knowledge files only when the paper is deeply model- or training-heavy and the distinction matters for the research assessment:

- `knowledge/transformer_components.md`
- `knowledge/training_recipes.md`
- `knowledge/loss_functions.md`
- `knowledge/paper_to_code_mistakes.md`
