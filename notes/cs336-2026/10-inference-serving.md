# 10 — Inference: prefill, decoding, and serving

**Recording:** [lecture 10](https://www.youtube.com/watch?v=EfM546A79aM) · **Primary materials:** [lecture_10.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_10.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-10.md).

## Technical notes

Autoregressive serving has two regimes. **Prefill** processes the supplied prompt and populates the attention key/value cache; **decode** produces new tokens one by one, often limited by memory bandwidth or KV-cache traffic. Caching K/V avoids recomputing the full prompt for every token. Its memory grows with context length and with the number/size of stored K/V heads.

Grouped-query or multi-query attention reduces KV traffic. Quantization trades precision for memory and bandwidth; speculative decoding drafts tokens with a cheaper process then verifies them with the target model, preserving its target distribution under the correct acceptance algorithm. Continuous batching and paged KV storage help a server allocate capacity among variable-length requests.

Sampling temperature, truncation, and explicit stop conditions change the candidate distribution; throughput and output quality must be assessed together.

## What matters for consider

Candidate generation creates a larger-than-usual output. Comparison ingests the question and *all* candidate texts. Choosing ingests the candidates again plus the comparison. Thus the protocol pays both repeated prefill and repeated context-token costs; prefix-cache reuse is possible only when an engine actually supports the exact shared prefix and model configuration.

A simple accounting identity is

\[
\text{total cost}=\sum_{s\in\{\mathrm{generate,contrast,choose}\}}
\bigl(p_{s,\mathrm{in}}\,T_{s,\mathrm{in}}+
p_{s,\mathrm{out}}\,T_{s,\mathrm{out}}\bigr),
\]

with prices per token and observed stage-token counts, plus any other billed fees.

## Proposed experiment

For each stage measure time to first token, decode token rate, input/output tokens, wall-clock time, and billed cost. Test whether shorter candidate output *actually* improves end-to-end correctness per dollar, or merely makes the judge less informed. Verify that cache optimizations do not share private or mismatched context.
