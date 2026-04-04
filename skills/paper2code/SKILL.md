---
name: paper2code
description: Turn an arXiv paper into translation-pipeline research artifacts: a citation-anchored ambiguity audit, a vault-ready literature note, and, when warranted, a prototype plan or scaffold for a relevant translation-pipeline component. Use when the user provides an arXiv link or ID and wants to evaluate, prototype, reproduce, or mine papers for the Icelandic-to-English translation pipeline.
---

# Paper2Code for Translation Pipeline

## Overview

This fork is for translation-pipeline research and discovery, not generic benchmark reproduction.

Default to `framework=torch` and `mode=discovery` unless the user asks for `prototype`, `full`, `minimal`, `educational`, `jax`, or `numpy`.

Your job is to produce three things, in this order:

1. a paper-grounded assessment of whether the paper matters for the translation pipeline
2. an explicit ambiguity audit of what the paper specifies, omits, or conflicts on
3. the smallest useful next artifact: a research note, a prototype plan, or a code scaffold

Do not default to writing a full standalone reproduction when a narrow prototype or a research-only note is the honest outcome.

## Output Paths

When running inside the translation-pipeline repo, first look for `config/paths.yml` and use it as the source of truth:

- notes path: `vault.literature_notes`
- ops data path: `ops.data`

If that file is unavailable, use these fallbacks:

- ops output root: `~/Projects/Claude/projects/translation-pipeline/data/`
- vault note root: `~/Projects/Laufey-verse/Efforts/Projects/Literature Notes/`

Place machine-generated research artifacts under:

`{ops_data_root}/paper2code/{paper_slug}/`

Create missing directories when needed.

## Required References

Read these before deciding what to build:

- `references/translation_pipeline_research.md`
- `references/ambiguity-patterns.md`

Use the research reference to decide whether the paper belongs in:

- `Prototype now`
- `Research only`
- `Reject for now`

## Workflow

### 1. Normalize the paper input

- Accept a full arXiv URL, a PDF URL, or a bare arXiv ID.
- Canonicalize to `arxiv_id` and `https://arxiv.org/pdf/{arxiv_id}.pdf`.
- Parse optional flags from the user request:
  - `--framework jax|torch|numpy`
  - `--mode discovery|prototype|full|minimal|educational`

If the user did not specify a mode, use `discovery`.

### 2. Obtain the source PDF

- Prefer an attached PDF or user-provided local file path.
- If network access is available, fetch the paper from arXiv.
- If the runtime cannot reach arXiv and no PDF is attached, ask the user to upload the PDF or provide a local path before continuing.
- Treat the PDF as the ground-truth source. Use the abstract page only for metadata checks or official-code links.

### 3. Extract structured paper content

Use `pdfplumber`, `pymupdf4llm`, or `pymupdf` to extract:

- title and authors
- abstract
- numbered sections and headings
- equations with surrounding prose
- tables with hyperparameters, ablations, or results
- appendices, footnotes, and figure captions
- references to official code or supplementary material

Organize the extraction by section hierarchy before summarizing anything. The audit and all downstream outputs must cite back to the extracted structure.

### 4. Assess translation-pipeline fit before coding

Map the paper to one or more concrete pipeline touchpoints from `references/translation_pipeline_research.md`.

For each plausible touchpoint, record:

- the exact pipeline problem it addresses
- the likely owning module or file
- whether the benefit is direct, indirect, or speculative
- the minimum test or fixture needed to evaluate it
- the operational risks to memoQ, terminology control, or QA

Then classify the paper as one of:

- `Prototype now`
- `Research only`
- `Reject for now`

If the right answer is `Research only` or `Reject for now`, do not force a code scaffold just because the skill is named `paper2code`.

### 5. Audit implementation ambiguity before writing any code

Classify implementation-relevant choices as:

- `SPECIFIED`
- `PARTIALLY_SPECIFIED`
- `UNSPECIFIED`
- `CONFLICTING`
- `FROM_OFFICIAL_CODE`

Audit at least these categories:

- architecture
- data processing
- optimization
- loss
- evaluation
- inference or decoding
- translation-specific constraints
- document or structure handling

For each decision, record:

- the decision name
- the paper evidence or quote
- the exact source location
- the ambiguity, conflict, or omission
- the implementation choice you would make if prototyping it

Use `references/ambiguity-patterns.md` to keep the classification honest.

### 6. Choose the correct output scope

#### `discovery`

Always generate:

- `README.md`
- `research_summary.md`
- `REPRODUCTION_NOTES.md`
- `prototype_plan.md`
- the vault literature note

Do not generate a substantial code scaffold unless the paper maps cleanly to an existing translation-pipeline component and the user asked for it.

#### `prototype`

Generate the discovery outputs plus the smallest useful scaffold for the most relevant module. Favor lightweight hooks, evaluators, prototype prompts, or validator experiments over full research repos.

#### `full`

Generate a broader scaffold only when the paper has a strong, direct implementation path and the extra code is justified.

#### `minimal`

Generate the discovery outputs plus a narrow implementation sketch or a single-file prototype if that is enough to test the main claim.

#### `educational`

Same as `prototype` or `full`, but with extra explanatory comments and more tutorial-style notes.

### 7. Write the ops artifacts

Create this structure under `{ops_data_root}/paper2code/{paper_slug}/`:

```text
{paper_slug}/
├── README.md
├── research_summary.md
├── REPRODUCTION_NOTES.md
├── prototype_plan.md
├── requirements.txt            # prototype/full/minimal when needed
├── src/                        # prototype/full/minimal when needed
│   ├── model.py
│   ├── loss.py
│   ├── data.py
│   ├── train.py
│   ├── evaluate.py
│   └── utils.py
└── configs/
    └── base.yaml
```

Apply these rules:

- Keep the implementation focused on the paper's core idea and its translation-pipeline touchpoint.
- Name the intended landing point in the real repo if the prototype succeeds.
- Add an inline citation or nearby note for every non-trivial implementation choice.
- Mark all paper gaps with `[UNSPECIFIED]`, `[PARTIALLY_SPECIFIED]`, or `[FROM_OFFICIAL_CODE]`.
- Prefer prototype harnesses, validators, scoring modules, or prompt logic when those are the real fit.
- Do not create a large training stack unless it is necessary to evaluate the idea.
- Do not touch production `src/` in the translation-pipeline repo unless the user explicitly asks for implementation there.

### 8. Write `research_summary.md`

Include these sections:

- `Paper Summary`
- `Why This Matters for Translation Pipeline`
- `Relevant Pipeline Touchpoints`
- `Operational Risks`
- `Recommendation`
  Use one of `Prototype now`, `Research only`, or `Reject for now`.
- `Fastest Credible Experiment`

### 9. Write `REPRODUCTION_NOTES.md`

Render the ambiguity audit into sections:

- `SPECIFIED choices`
- `PARTIALLY_SPECIFIED choices`
- `UNSPECIFIED choices`
- `CONFLICTING specifications`
- `FROM_OFFICIAL_CODE choices`
- `Known Deviations`
- `Recommended Ablations`

Make it clear which choices are paper-grounded and which are engineering defaults.

### 10. Write `prototype_plan.md`

Include:

- target translation-pipeline module
- prototype objective
- input and output expectations
- fixture or corpus requirements
- evaluation plan
- rollback plan
- productionization criteria

### 11. Write the vault literature note

Create `{vault_note_root}/{YYYY-MM-DD}-{paper-title}.md` with this structure:

```markdown
---
title: "{Paper Title}"
aliases:
  - "{arxiv_id}"
tags:
  - paper
  - translation-pipeline
  - paper2code
  - "{domain}"
date: {today}
arxiv: "{arxiv_id}"
authors: "{Author1}, {Author2}, ..."
framework: "{framework}"
research_disposition: "{Prototype now | Research only | Reject for now}"
ops_artifact_path: "{ops_data_root}/paper2code/{paper_slug}/"
status: generated
type: literature_note
---

# {Paper Title}

**arXiv**: [{arxiv_id}](https://arxiv.org/abs/{arxiv_id})
**Authors**: {authors}
**Artifacts**: `{ops_data_root}/paper2code/{paper_slug}/`

## Abstract

{verbatim abstract}

## Why This Matters for Translation Pipeline

{brief prose}

## Relevant Touchpoints

{brief prose}

## Recommendation

{Prototype now | Research only | Reject for now}

## Fastest Credible Experiment

{brief prose}

---

{key contents of REPRODUCTION_NOTES.md}
```

The note should be useful even if no code is written. Optimize for future retrieval and research review, not just immediate implementation.

### 12. Report completion

Summarize the run with:

1. paper title and authors
2. the translation-pipeline disposition
3. audit counts for each ambiguity tag
4. the ops artifact path
5. the vault note path
6. the most load-bearing risks or unspecified choices

## Citation Rules

- Add a nearby citation or note for every important implementation choice.
- When the paper omits a value, use an explicit `[UNSPECIFIED]` comment with the chosen default and plausible alternatives.
- When the paper contradicts itself, cite both sources and record the resolution in `REPRODUCTION_NOTES.md`.
- When a decision comes from official code, mark it `[FROM_OFFICIAL_CODE]` and capture the repo location when available.

## Error Handling

- If PDF retrieval fails, try both `https://arxiv.org/pdf/{id}` and `https://arxiv.org/pdf/{id}.pdf`.
- If text extraction is poor because the PDF is scanned or image-heavy, note low confidence and continue with best-effort extraction.
- If the paper is not strictly ML, continue anyway if it has operational relevance to the translation pipeline.
- If the paper has weak operational fit, produce the note and reject it honestly instead of manufacturing a prototype.
