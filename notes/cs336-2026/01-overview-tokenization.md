# 01 — Overview and tokenization

**Recording:** [Stanford CS336, Spring 2026, lecture 1](https://www.youtube.com/watch?v=JuoVZkPBiKk) · **Primary materials:** [lecture_01.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_01.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-01.md).

## Technical notes

A language model factorizes a sequence probability into conditional probabilities:

\[
p(x_1,\ldots,x_T)=\prod_{t=1}^{T}p(x_t\mid x_1,\ldots,x_{t-1}).
\]

The input is a sequence of *token IDs*, not words or Unicode code points. Byte-pair encoding (BPE) initializes a byte vocabulary and iteratively merges frequent adjacent pairs. The learned merge ranks determine both training examples and inference-time token counts. UTF-8 decoding after generation needs an explicit policy for incomplete or invalid byte sequences. Tokenizer comparisons should be made on the same underlying text, not merely on nominal tokens per second.

The course's organizing problem is efficiency under a finite budget: architecture, data quality, kernels, model size, and inference all interact. Mechanics learned from small models transfer better than empirical claims about large-model capabilities. Implementing a small model is useful for understanding interfaces; it is not automatically a route to frontier-quality judges.

## What matters for consider

The existing protocol has three calls: Generate several distinct complete candidates; Contrast under fixed criteria without choosing; Choose or return none. Treat the generator, judge, and selector as independently configurable roles. The input question, full candidate texts, prompts, raw responses, token counts, and selection rationale are part of the evidence.

Tokenization affects effective context length and price. Some prompts, unusual mathematical symbols, and long candidates tokenize very differently across models. Model comparisons at fixed *token* budget need to report bytes or characters covered as well.

## Proposed experiment

Use ordinary English, mathematical Unicode, code, and long documents. For each candidate generator, record bytes, token IDs/counts if available, elapsed time, and failure to round-trip text. Compare identical questions under fixed currency and latency budgets; do not confuse tokenizer differences with reasoning quality.
