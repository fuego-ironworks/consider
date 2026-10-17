# 07 — Distributed training and parallelism I

**Recording:** [lecture 7](https://www.youtube.com/watch?v=SzpOcwdIL0Y) · **Primary materials:** [lecture_07.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_07.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-07.md).

## Technical notes

Distributed training trades computation against communication. The basic collective operations are broadcast, all-reduce, all-gather, and reduce-scatter. Data parallelism replicates model weights and divides examples; synchronized gradients require cross-device reductions. Tensor parallelism partitions operations inside a layer; pipeline parallelism partitions successive layers and introduces scheduling bubbles.

Network topology matters. Interconnect bandwidth, communication latency, message size, and how well communication overlaps compute determine actual scaling. A first model is

\[
t_\text{transfer}\approx t_\text{startup}+
\frac{\text{transferred bytes}}{\text{effective bandwidth}},
\]

but all-reduce topology and contention add structure beyond this simple estimate.

Distributed execution introduces new failure states: one rank may progress while another fails, and successful *submission* does not prove successful aggregation. Profiling each rank and binding the reported result to the actual completed job is essential.

## What matters for consider

Generate, Contrast, and Choose have a strict data dependency. Generation can be parallelized internally or across models, but Contrast must consume *all* accepted candidate bytes; Choose must consume the accepted comparison. The pipeline should not treat concurrent requests as a proof of distinct candidates.

FP receipts ought to distinguish request identity, exact candidate output, and evidence of stage completion. Avoid merging partial results if one candidate stage timed out.

## Proposed experiment

Generate independent candidates concurrently, deliberately delay or fail one producer, and verify that Contrast either receives the full declared candidate set or fails closed. Compare wall-clock savings and provider throttling with serial generation. Document retries as new attempts, not silent replacement of the original candidate.
