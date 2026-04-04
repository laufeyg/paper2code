# paper2code

> **arXiv URL in -> translation-pipeline research note, ambiguity audit, and prototype direction out**

This fork is tuned for Laufey's Icelandic-to-English translation work, not generic benchmark reproduction.

The base `paper2code` idea is still intact: extract what a paper actually specifies, surface what it leaves vague, and only then write code. The difference is the target. Instead of defaulting to a standalone ML reproduction repo, this fork defaults to research and discovery outputs that are directly actionable for the translation pipeline.

## Why this fork exists

The translation pipeline already has real operational constraints:

- memoQ bilingual `.docx` handling
- termbase-backed terminology control
- number and identifier preservation
- inline tag preservation
- legal and regulatory style constraints
- human review gates before delivery

Most papers are not written with those constraints in mind. A generic paper-to-code workflow is useful, but it still leaves a gap between "interesting paper" and "something worth testing in production."

This fork closes that gap by asking a different first question:

**How does this paper help the translation pipeline, and what is the smallest credible experiment to prove it?**

## What this fork does differently

1. **Research-first default**

   The default output is `discovery`, not a full reproduction. The skill produces a vault-ready literature note, a paper-grounded ambiguity audit, and a prototype recommendation before it writes substantial code.

2. **Translation-pipeline touchpoint mapping**

   Every paper is mapped to one or more real pipeline areas:

   - intake and document parsing
   - chunking and outline generation
   - terminology extraction and termbase work
   - translation prompting and routing
   - memoQ bilingual batch handling
   - QA and validators
   - formatting and delivery
   - research memory and discovery

3. **Two-repo output model**

   Research artifacts go to the ops repo. Human-readable notes go to the vault.

   - Ops artifacts: `translation-pipeline/data/paper2code/{paper_slug}/`
   - Vault note: `Laufey-verse/Efforts/Projects/Literature Notes/{date}-{paper-title}.md`

   When available, the skill reads `translation-pipeline/config/paths.yml` and uses those configured paths instead of hardcoded guesses.

4. **Prototype discipline**

   The skill does not default to building a full training repo. It recommends the smallest useful prototype that can plug into the existing pipeline and names the likely integration point.

5. **Operational evaluation lens**

   Papers are evaluated against the concerns that matter in this repo:

   - terminology fidelity
   - DNT and identifier safety
   - tag integrity
   - numeric consistency
   - legal and regulatory tone
   - auditability and reviewer ergonomics

## Usage

### Discovery mode (default)

```text
/paper2code https://arxiv.org/abs/1706.03762
```

Produces:

- a literature note for the vault
- `research_summary.md`
- `REPRODUCTION_NOTES.md`
- `prototype_plan.md`

### Prototype mode

```text
/paper2code https://arxiv.org/abs/1706.03762 --mode prototype
```

Adds a minimal scaffold only for the most relevant pipeline touchpoint.

### Full mode

```text
/paper2code https://arxiv.org/abs/1706.03762 --mode full --framework torch
```

Generates a broader scaffold when the paper maps cleanly to a concrete implementation path.

### Educational mode

```text
/paper2code 2006.11239 --mode educational
```

Same as prototype or full, but with extra explanatory notes.

## What you get

```text
translation-pipeline/data/paper2code/{paper_slug}/
├── README.md
├── research_summary.md
├── REPRODUCTION_NOTES.md
├── prototype_plan.md
├── requirements.txt            # prototype/full only
├── src/                        # prototype/full only
│   ├── model.py
│   ├── loss.py
│   ├── data.py
│   ├── train.py
│   ├── evaluate.py
│   └── utils.py
└── configs/
    └── base.yaml

Laufey-verse/Efforts/Projects/Literature Notes/
└── {YYYY-MM-DD}-{paper-title}.md
```

### Key files

| File | Purpose |
|------|---------|
| `research_summary.md` | Why the paper matters, which pipeline touchpoints it affects, and whether it is worth prototyping |
| `REPRODUCTION_NOTES.md` | Explicit audit of what the paper specifies, omits, or conflicts on |
| `prototype_plan.md` | Smallest credible experiment for the translation pipeline |
| Vault note | Durable research note with links back to the ops artifacts |

## What this fork will not do

- It will not pretend every paper deserves code.
- It will not silently fill in missing details.
- It will not recommend architecture work without naming the existing pipeline module it should touch.
- It will not optimize for leaderboard replication if the paper has weak operational fit.
- It will not bypass human-review concerns such as terminology approval, memoQ compatibility, or QA gates.

## Repo layout

- `skills/paper2code/SKILL.md`
  The main workflow, rewritten for translation-pipeline research and discovery.
- `skills/paper2code/references/translation_pipeline_research.md`
  The evaluation lens for pipeline fit.
- `skills/paper2code/references/ambiguity-patterns.md`
  The ambiguity taxonomy used during auditing.
- `skills/paper2code/scripts/`
  Helper scripts for paper retrieval and structure extraction.
- `skills/paper2code/worked/`
  Older worked examples from the generic paper-to-code workflow. Still useful as quality references.

## Worked examples

The repo still includes worked examples such as Attention Is All You Need and DDPM. They remain useful as trust-building examples, but this fork now treats them as examples of citation discipline rather than the default shape of every output.
