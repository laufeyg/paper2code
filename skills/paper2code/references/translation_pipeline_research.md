# Translation Pipeline Research Lens

Use this reference to decide whether a paper is genuinely useful for the translation pipeline and what kind of prototype, if any, should follow.

## Primary outcomes

Prefer papers that improve one or more of these operational outcomes:

- terminology fidelity
- safer handling of memoQ bilingual `.docx`
- better preservation of numbers, dates, codes, and identifiers
- better inline tag preservation
- stronger legal and regulatory register
- more reliable document parsing, chunking, or QA
- better retrieval and knowledge capture for future translation work

## Existing pipeline touchpoints

Map every paper to one or more real integration points before recommending code.

### 1. Intake and document parsing

Relevant when the paper helps with:

- OCR or PDF parsing
- structure recovery
- table or footnote extraction
- section detection
- bilingual alignment

Likely repo anchors:

- `src/assembly/`
- `src/pipeline/orchestrator.py`
- document-ingestion utilities and future intake scripts

### 2. Outline and chunking

Relevant when the paper helps with:

- discourse segmentation
- chunk boundary selection
- context-window packing
- heading recovery
- retrieval-aware chunking

Likely repo anchors:

- `src/prompts/outline.py`
- `src/pipeline/orchestrator.py`

### 3. Terminology and termbase work

Relevant when the paper helps with:

- term extraction
- terminology normalization
- DNT detection
- glossary ranking
- terminology retrieval during generation

Likely repo anchors:

- `src/db/termbase.py`
- `scripts/import_review_terms.py`
- `scripts/normalize_termbase.py`

### 4. Translation prompting and routing

Relevant when the paper helps with:

- constrained translation
- retrieval-augmented generation
- adaptation to domain or style
- model-routing decisions
- uncertainty-aware generation

Likely repo anchors:

- `src/prompts/translate.py`
- `src/prompts/translate_bilingual.py`
- `src/prompts/common.py`
- `config/routing.yml`

### 5. memoQ bilingual mode

Relevant when the paper helps with:

- segment-level translation
- alignment
- retry strategies
- tag-safe generation
- structured output for bilingual tables

Likely repo anchors:

- `src/bilingual/runner.py`
- `src/bilingual/batcher.py`
- `src/bilingual/docx_io.py`
- `src/bilingual/segment.py`

### 6. QA and validators

Relevant when the paper helps with:

- faithfulness checking
- quality estimation
- consistency validation
- terminology enforcement
- error detection before delivery

Likely repo anchors:

- `src/validators/tag_integrity.py`
- `src/validators/source_target_number_match.py`
- `src/validators/legal_style.py`
- `src/validators/zero_tolerance.py`

### 7. Formatting and delivery

Relevant when the paper helps with:

- docx regeneration
- layout preservation
- table reconstruction
- delivery-time formatting checks

Likely repo anchors:

- `src/assembly/assemble.py`
- `src/assembly/assemble_docx.py`
- `src/assembly/assemble_pdf.py`

### 8. Research memory and discovery

Relevant when the paper helps with:

- literature-note generation
- related-paper discovery
- retrieval over prior notes
- research triage and clustering

Likely repo anchors:

- `pipelines/readwise/`
- `integrations/smart_connections/`
- vault literature-note flows

## Questions to answer for every paper

Before recommending any implementation, answer these explicitly:

1. Which pipeline touchpoint does this paper improve?
2. Which current failure mode or manual bottleneck does it address?
3. What is the smallest prototype that would validate the idea?
4. Which existing file or module would own that prototype?
5. What data or fixtures are required?
6. How would we evaluate success without harming delivery quality?
7. Does the idea fit the current human-gate workflow?
8. Does it require cloud inference, new infrastructure, or external services?
9. Is the paper strong enough to justify engineering time?

## Evaluation checklist

Prefer ideas that can be judged against concrete pipeline criteria:

- terminology fidelity
- DNT and identifier preservation
- number and date consistency
- memoQ inline-tag preservation
- legal and regulatory style compatibility
- reviewer auditability
- ease of rollback if the prototype fails
- compatibility with the current 9-stage pipeline

## Discovery output expectations

Every run should produce a clear recommendation:

- `Prototype now`
  The paper maps cleanly to a concrete module and can be evaluated with existing fixtures or a small new harness.
- `Research only`
  The paper is informative, but the right next step is a note, comparison, or related-paper search rather than code.
- `Reject for now`
  The paper is interesting but has poor operational fit, weak evidence, or too much hidden implementation complexity.

## When not to code

Do not generate a substantial scaffold when:

- the paper has no credible mapping to a current or planned pipeline component
- the paper is mostly benchmark scaling without operational relevance
- the paper's gains depend on unavailable data or infrastructure
- the core contribution is too underspecified to evaluate honestly
- the likely prototype would add fragility to memoQ, terminology, or QA-critical paths

When that happens, write the note, document the reason, and stop.
