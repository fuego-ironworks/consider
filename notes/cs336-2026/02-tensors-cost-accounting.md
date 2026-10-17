# 02 — Tensor semantics and resource accounting

**Recording:** [lecture 2](https://www.youtube.com/watch?v=kuYAsz7zspQ) · **Primary materials:** [lecture_02.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_02.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-02.md).

## Technical notes

Tensor shape, stride, dtype, and device all matter. Einstein-style contractions let one describe an operation independently from an implementation. In mixed-precision training, parameters, gradients, optimizer moments, master copies, and activations can have different precisions. Counting only parameter bytes systematically underestimates training memory.

A frequently used *leading-order estimate* for dense Transformer pretraining is about \(6ND\) floating-point operations for \(N\) parameters and \(D\) processed tokens; this is not an exact hardware bill. Backward computation, attention at long context, recomputation, padding, MoE sparsity, optimizer work, and hardware utilization must be accounted for separately.

Arithmetic intensity is operations per byte transferred; high operation count does not imply long runtime if the hardware is instead constrained by memory traffic. Gradient accumulation exchanges more steps for smaller per-step memory; activation checkpointing deliberately recomputes work to save activation storage.

## What matters for consider

Do **not** treat training FLOPs as the cost of running consider. The actual workload is one long candidate-generation completion, one comparison over all candidates, and one final choice. Record input/output token counts, cached-input charges if applicable, observed per-stage latency, and error/retry overhead. The comparison prompt grows with the combined length of candidates.

## Proposed experiment

Instrument the three stages independently, then report total cost and the 50th/95th percentile end-to-end latency. Compare a single direct answer with consider under equal **total spend** as well as under equal request counts. A claimed win without controlling the budget is incomplete.
