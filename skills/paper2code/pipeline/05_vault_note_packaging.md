# Stage 5: Vault Note Packaging

## Purpose

Package the research result into a vault-ready literature note so the paper is easy to revisit, link to projects, and compare with later findings.

## Input

- Final research pack from Stage 4
- Contribution analysis from Stage 2
- Evidence audit from Stage 3

## Output

- `{paper_slug}/OBSIDIAN_NOTE.md`

---

## Required note qualities

The note should:

- be readable without opening the paper immediately
- preserve the recommendation and key risks
- point back to the generated pack files
- be easy to tag and link inside the vault

---

## Recommended frontmatter

Use frontmatter like:

```yaml
---
type: literature-note
title: "{paper title}"
arxiv_id: "{id}"
paper_slug: "{paper_slug}"
decision: "{ADOPT NOW | PROTOTYPE | WATCHLIST | REJECT}"
pipeline_focus:
  - "{surface}"
  - "{surface}"
tags:
  - paper
  - translation-pipeline
  - "{domain tag}"
created_at: "{iso timestamp}"
---
```

Adjust fields if the user's vault conventions differ, but keep them minimal and useful.

## Note structure

Write the note with these sections:

### 1. What this paper is

- title
- authors/year
- one-sentence contribution

### 2. Why it matters here

- the translation-pipeline problem it may influence
- why that matters now

### 3. Recommendation

- one of `ADOPT NOW`, `PROTOTYPE`, `WATCHLIST`, `REJECT`
- one short paragraph explaining why

### 4. Evidence summary

- strongest supporting evidence
- biggest weakness or missing detail

### 5. Pipeline fit

- candidate stages/modules
- what kind of change it implies

### 6. Next step

- the single best next action

### 7. Links to generated artifacts

- `README.md`
- `EVIDENCE_AUDIT.md`
- `PIPELINE_FIT.md`
- `EXPERIMENT_PLAN.md`

---

## Writing rules

1. Keep the note shorter than the full pack.
2. Do not duplicate tables unless they materially help.
3. Preserve uncertainty. The vault note is not marketing copy.
4. Make the "next step" field concrete enough that future you can act on it.

---

## Self-check

- [ ] The recommendation is visible near the top
- [ ] The note says why the paper does or does not matter to the pipeline
- [ ] The note links back to the deeper artifacts
- [ ] Someone skimming the vault can decide whether to open the full pack
