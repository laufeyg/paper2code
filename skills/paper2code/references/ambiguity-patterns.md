# Ambiguity Patterns

Use this reference when the paper does not map cleanly to `SPECIFIED`, `PARTIALLY_SPECIFIED`, `UNSPECIFIED`, `CONFLICTING`, or `FROM_OFFICIAL_CODE`.

## Classification order

Apply evidence in this order:

1. Main paper body
2. Tables, captions, footnotes, and appendix
3. Paper-linked supplemental material
4. Official code released by the authors
5. Common framework defaults, clearly marked as your choice

Do not silently jump to step 4 or 5 when the paper is merely inconvenient to parse.

## Common ambiguity patterns

### Architecture

- A paper says "standard Transformer encoder" without defining:
  - pre-norm vs post-norm
  - feed-forward expansion ratio
  - activation choice
  - attention bias usage
  - dropout placement
- A paper says a block is "similar to" prior work.
  Treat inherited details as unspecified unless the cited work clearly pins them down and the current paper explicitly adopts them.

### Optimization

- The optimizer name is given, but `betas`, `eps`, or weight decay are omitted.
- The scheduler family is named, but warmup length, total decay horizon, or floor learning rate are omitted.
- Batch size is given without clarifying whether it is per-device, global, or after gradient accumulation.

### Data

- Tokenization, filtering, normalization stats, or augmentation order are left out.
- A dataset is named, but the exact split, deduping, or preprocessing recipe is omitted.
- Evaluation preprocessing differs subtly from training preprocessing and the paper does not say how.

### Loss

- A loss family is named but weighting coefficients, label smoothing, margin values, or auxiliary terms are missing.
- Multiple objectives are described in prose, but the exact combined objective is not written down.

### Evaluation

- The metric is named, but the implementation details are not.
- Best-checkpoint selection is omitted.
- Number of seeds, confidence intervals, or model-selection procedure are missing.

### Training details

- Epochs are given, but total steps depend on filtering or repeated sampling that is not described.
- Mixed precision, gradient clipping, or gradient accumulation are not mentioned.
- Early stopping is implied by results tables but never stated.

## How to tag borderline cases

### `PARTIALLY_SPECIFIED`

Use when the paper narrows the space but still leaves a load-bearing choice open.

Examples:

- "Cosine schedule" with no `T_max`
- "LayerNorm" with no epsilon
- "Data augmentation" with no exact recipe

### `UNSPECIFIED`

Use when the paper gives no usable value or procedure for the decision.

Examples:

- no random-seed strategy
- no optimizer epsilon
- no checkpoint-selection rule

### `CONFLICTING`

Use when two paper sources cannot both be true.

Examples:

- main text says `dropout=0.1`, appendix says `dropout=0.3`
- table lists 8 heads, equation prose lists 16 heads

Record both sources explicitly. Do not average, merge, or infer a compromise.

### `FROM_OFFICIAL_CODE`

Use when the missing detail is recoverable only from the authors' code.

Record:

- repository URL
- file path
- commit or tag when available
- the exact value or behavior adopted

## Load-bearing defaults to call out

Surface these prominently when they are not paper-specified:

- normalization epsilon
- optimizer `betas` and `eps`
- warmup steps or decay horizon
- data preprocessing order
- checkpoint-selection rule
- seed count and evaluation aggregation
- padding, masking, or batching conventions

## Reporting rule

When in doubt, make the uncertainty visible. A reproduction audit is more valuable when it is explicit about what the paper did not tell you.
