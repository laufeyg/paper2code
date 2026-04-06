# Stage 2: Contribution and Relevance Identification

## Purpose

Before deciding anything, understand what the paper actually contributes and whether that contribution matters for the translation-pipeline. This stage is about relevance, not hype.

## Input

- Parsed paper sections from Stage 1
- `paper_metadata.json`
- `knowledge/translation_pipeline_research.md`

## Output

- `.paper2code_work/{ARXIV_ID}/contribution.md`

---

## Reasoning protocol

### Step 1: State the paper's contribution in one sentence

Read the abstract and introduction twice.

Write a one-sentence answer to:

- What new thing does this paper claim to add?
- Is that addition a method, dataset, evaluation practice, system trick, or prompt/process trick?
- If this paper did not exist, what would be missing?

Do not write a generic summary. Write a precise contribution statement.

### Step 2: Classify the paper for translation-pipeline purposes

Pick the closest category:

#### (a) Translation or adaptation method
Examples:
- MT decoding changes
- document-level translation methods
- context or retrieval for translation
- post-editing or adaptation loops

Most relevant surfaces:
- `src/prompts/translate.py`
- `src/prompts/translate_bilingual.py`
- batch or context handling

#### (b) Terminology or lexicon method
Examples:
- term extraction
- lexicon induction
- constrained terminology insertion
- glossary enforcement

Most relevant surfaces:
- `src/db/termbase.py`
- term normalization/import flows
- prompt glossary construction

#### (c) Document structure or chunking method
Examples:
- layout understanding
- section-aware chunking
- structure-aware translation
- bilingual alignment

Most relevant surfaces:
- outline/chunk creation
- `src/bilingual/segment.py`
- `src/bilingual/batcher.py`

#### (d) Quality estimation or validation method
Examples:
- QE
- hallucination detection
- adequacy/consistency checks
- terminology or numeric consistency checks

Most relevant surfaces:
- `src/validators/`
- Stage 8 QA
- bilingual validation before delivery

#### (e) Human-in-the-loop or workflow paper
Examples:
- reviewer workflows
- interactive translation systems
- CAT integrations
- approval loops

Most relevant surfaces:
- human gates
- reviewer fix application
- bilingual memoQ flow

#### (f) Infrastructure or system paper
Examples:
- serving, latency, compression, retrieval infrastructure

Most relevant only if it improves the current workflow without forcing a rewrite.

### Step 3: Identify the primary decision this paper could influence

Ask:

- Which current pain point could this paper improve?
- Is the paper about discovery only, or could it inform an actual pipeline change?
- Does it affect the standard 9-stage flow, the bilingual runner, the termbase, validators, or the research/vault layer?

Prefer one primary decision. Secondary uses can be listed later.

### Step 4: Check domain fit early

Before going deeper, answer:

- Does the paper deal with document translation, sentence translation, terminology, QA, workflow, or something only loosely related?
- Is it multilingual, English-centric, or language-pair-specific?
- Is there any sign of Icelandic, morphologically rich languages, legal/regulatory text, or document-delivery constraints?

If the paper is only weakly related, say so directly. Weak relevance is a real result.

### Step 5: Identify candidate insertion points

Map the paper to at most three concrete surfaces such as:

- outline generation
- chunking
- term extraction / term review
- translation prompts
- bilingual batching / memoQ handling
- QA / validator layer
- final document assembly
- vault-based literature note or discovery workflow

If you cannot map it to a concrete surface, the paper is probably `WATCHLIST` or `REJECT`.

### Step 6: Record official code status

If `paper_metadata.json` contains `official_code`:

1. Verify the repository is actually for this paper.
2. Record what it appears to cover.
3. Treat it as reproducibility evidence, not proof that the method fits production.

### Step 7: Write the contribution memo

Save this structure to `.paper2code_work/{ARXIV_ID}/contribution.md`:

```markdown
# Contribution Analysis

## Paper
{title} ({year})
{authors}
arxiv: {id}

## One-sentence contribution
{precise statement}

## Paper type
{category with justification}

## Primary pipeline decision it could influence
{one paragraph}

## Relevance to translation-pipeline
- Strength of relevance: {direct | indirect | weak}
- Why: {brief explanation}

## Candidate insertion points
1. {surface} — {why}
2. {surface} — {why}
3. {surface} — {why}

## Official code
{URL or "None found"}
{what it appears to contain}

## Initial read
- Most promising aspect:
- Biggest fit risk:
- Biggest unanswered question:
```

### Step 8: Self-check

Before continuing:

- Could someone who knows the translation-pipeline read this and know why the paper matters?
- Did you identify real insertion points, not vague aspirations?
- Did you overstate relevance because the paper sounds modern or strong?
- If the paper is weakly related, did you say that plainly?
