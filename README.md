# paper2code

> **arxiv URL in -> translation-pipeline research brief out**

```
┌─────────────────────────────┐         ┌──────────────────────────────────────┐
│                             │         │  {paper_slug}/                       │
│  /paper2code                │         │  ├── README.md                       │
│  https://arxiv.org/abs/     │  ───▶   │  ├── EVIDENCE_AUDIT.md              │
│  1706.03762                 │         │  ├── PIPELINE_FIT.md                │
│                             │         │  ├── EXPERIMENT_PLAN.md             │
│                             │         │  └── OBSIDIAN_NOTE.md               │
└─────────────────────────────┘         └──────────────────────────────────────┘
```

This fork keeps the strong parts of the original `paper2code` workflow:

- arXiv acquisition
- appendix and footnote mining
- paper structure extraction
- official-code discovery

But it changes the end product. Instead of turning papers into implementation repos, this fork turns them into a **decision-ready research pack for the IS->EN translation pipeline**.

---

## Why this fork exists

The translation-pipeline does not need generic paper summaries or speculative rewrites. It needs a workflow that answers questions like:

- Does this paper help with chunking, terminology, translation, QA, or bilingual memoQ output?
- Are the reported gains relevant to legal or regulatory document delivery?
- Does the paper assume language pairs, datasets, or hardware that make it a bad fit here?
- What is the smallest safe experiment to run in the current pipeline?

This fork is tuned for that kind of research and discovery.

---

## What this fork does differently

1. **Evidence discipline**
   Benchmark wins are not treated as deployable improvements. Claims are separated into supported, partial, and unknown.

2. **Pipeline-fit mapping**
   Every paper is mapped onto concrete translation-pipeline surfaces such as `src/prompts/`, `src/validators/`, `src/db/termbase.py`, `src/bilingual/runner.py`, and the human-gated 9-stage flow.

3. **Operational realism**
   Legal/regulatory quality, terminology control, bilingual `.docx` handling, tag integrity, and deterministic validation matter more than leaderboard deltas.

4. **Experiment-first outputs**
   The final deliverable is a recommendation and experiment plan, not speculative production code.

5. **Vault-ready packaging**
   The workflow produces an Obsidian-friendly literature note so research can move directly into the wider knowledge system.

---

## Install

```bash
npx skills add laufeyg/paper2code/skills/paper2code
```

Once installed, run:

```text
/paper2code https://arxiv.org/abs/1706.03762
```

---

## Usage

### Basic triage

```text
/paper2code https://arxiv.org/abs/1706.03762
```

### Full discovery pack

```text
/paper2code https://arxiv.org/abs/2006.11239 --mode full
```

### Team-shareable version with extra explanation

```text
/paper2code https://arxiv.org/abs/2106.09685 --mode educational
```

---

## What you get

```text
{paper_slug}/
├── README.md             # Executive brief and recommendation
├── EVIDENCE_AUDIT.md     # Claims, datasets, metrics, missing details, reproducibility
├── PIPELINE_FIT.md       # Mapping to translation-pipeline stages, modules, and gates
├── EXPERIMENT_PLAN.md    # Minimal safe experiment inside the current workflow
└── OBSIDIAN_NOTE.md      # Vault-ready literature note with frontmatter
```

### Output intent

| File | Purpose |
|------|---------|
| `README.md` | The shortest possible answer to "should I care about this paper?" |
| `EVIDENCE_AUDIT.md` | Separates demonstrated results from paper hype or missing detail. |
| `PIPELINE_FIT.md` | Shows exactly where the paper could affect the pipeline, if anywhere. |
| `EXPERIMENT_PLAN.md` | Defines a bounded next step instead of a vague "we should try this." |
| `OBSIDIAN_NOTE.md` | Makes the paper easy to store, link, and revisit in the vault. |

---

## Recommendation states

Every run should end with one of these:

- `ADOPT NOW` — narrow change, strong evidence, low operational risk
- `PROTOTYPE` — promising, but needs a bounded experiment in the pipeline
- `WATCHLIST` — interesting but not ready to spend engineering time on
- `REJECT` — weak fit, weak evidence, or too much operational risk

`REJECT` is a valid outcome. The point of this fork is better decisions, not more experiments.

---

## Decision criteria for this fork

The paper is judged against translation-pipeline realities:

- IS->EN or otherwise relevant document-translation evidence
- terminology control and termbase compatibility
- bilingual/memoQ or document-structure friendliness
- quality assurance compatibility with validators and human gates
- legal/regulatory robustness, not just sentence-level benchmark quality
- operational cost and complexity relative to likely gain

---

## What this fork will not do

- It will not pretend BLEU or COMET gains automatically improve client deliverables.
- It will not recommend major architecture rewrites from a thin paper.
- It will not treat missing datasets, prompts, or evaluation details as "close enough."
- It will not generate production code just to make the output look complete.

---

## Contributing

The most useful additions are:

1. Translation-relevant worked examples
2. Domain knowledge for legal/regulatory translation research
3. Better guardrails for evidence quality and pipeline-fit decisions
4. Tighter vault note templates

If a paper repeatedly suggests the same kind of improvement, capture that pattern in `knowledge/` or `guardrails/` instead of rediscovering it every run.
