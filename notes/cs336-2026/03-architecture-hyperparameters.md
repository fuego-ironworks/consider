# 03 — Transformer architecture and hyperparameters

**Recording:** [lecture 3](https://www.youtube.com/watch?v=lVynu4bo1rY) · **Primary materials:** [lecture_03.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_03.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-03.md).

## Technical notes

A decoder Transformer stacks attention and feed-forward transformations around a residual stream. In pre-normalized form, one residual sublayer is approximately

\[
x_{l+1}=x_l+F(\operatorname{Norm}(x_l)).
\]

Keeping an identity path outside normalization helps gradient propagation. Common contemporary variants use RMSNorm, bias-free projections, gated feed-forward units such as SwiGLU, and rotary positional embeddings (RoPE). Attention-head dimensions, depth/width ratios, vocabulary size, and learning-rate stability cannot be selected from a single abstract optimum; throughput and inference constraints participate in the choice.

Multi-head attention uses separate key/value projections for query heads. Grouped-query attention (GQA) reuses key/value heads across groups of query heads, reducing inference KV-cache requirements. Longer context capacity is partly architectural and partly a matter of actual training distribution and inference cost. A large advertised context window is not evidence of faithful comparison over that whole window.

## What matters for consider

The contrast call receives multiple long answers. Its useful context length, positional effects, and fidelity to earlier candidates deserve direct testing. Model parameter count and architecture do not tell us whether a model can reliably judge its own alternatives.

The three roles need not use one model. A small generator plus a stronger independent judge may beat same-model self-selection, or it may not. Treat this as an experiment.

## Proposed experiment

Construct sets of 2, 4, and 8 answers containing one provable mathematical error, move the erroneous answer among positions, and vary distractor length. Measure whether a judge detects the same error and whether the selector chooses it. Preserve every intermediate answer.
