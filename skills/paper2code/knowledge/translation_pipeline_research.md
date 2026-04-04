# Translation-Pipeline Research Context

Use this file whenever you assess whether a paper belongs in the translation-pipeline.

## Core mission

This project is an **IS->EN translation pipeline** for client-deliverable legal and regulatory documents.

That changes what counts as a good paper fit:

- faithfulness matters more than flashy generation
- terminology control matters more than broad stylistic fluency
- document structure, numbers, tags, and bilingual formatting matter
- human review is part of the system, not a failure of the system

## Current workflow surfaces

The main integration surfaces are:

- `src/pipeline/orchestrator.py`
- `src/prompts/outline.py`
- `src/prompts/translate.py`
- `src/prompts/translate_bilingual.py`
- `src/db/termbase.py`
- `src/bilingual/batcher.py`
- `src/bilingual/segment.py`
- `src/validators/`
- `src/assembly/`
- `src/vault/io.py`

## Human gates

The pipeline has three human gates:

- **Gate A:** outline review
- **Gate B:** term review / translation authorization
- **Gate C:** chunk QA validation

Any proposed method that breaks, bypasses, or obscures these gates should be treated skeptically unless the evidence is unusually strong.

## What strong paper fit looks like

A strong fit usually improves one of these:

- outline or document-structure understanding
- chunking or alignment
- term extraction, normalization, or enforcement
- translation adequacy in domain text
- bilingual memoQ behavior
- deterministic QA or reviewer productivity
- pre-delivery validation confidence

## What weak paper fit looks like

Weak fit signals include:

- sentence-only results with no document perspective
- no terminology, consistency, or QA story
- no relevance to low-resource or morphologically rich languages
- methods that assume fresh greenfield infrastructure
- gains that depend on private data or unclear human effort

## Preferred recommendations

Default bias:

- `PROTOTYPE` over `ADOPT NOW`
- `WATCHLIST` over vague implementation enthusiasm
- `REJECT` when fit is structurally weak

## Good experiment patterns

Prefer experiments that can run against:

- existing regression fixtures
- bilingual test documents
- termbase-backed prompts
- current validator outputs
- specific document classes already seen in the pipeline

Avoid experiments that require a whole new platform before learning anything.
