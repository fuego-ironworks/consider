# 08 — Distributed training and parallelism II

**Recording:** [lecture 8](https://www.youtube.com/watch?v=6-cXp-aOmdg) · **Primary materials:** [lecture_08.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_08.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-08.md).

## Technical notes

Ordinary data parallelism stores a full copy of model weights, gradients, and optimizer state on each worker. ZeRO-style sharding progressively removes redundant copies: Stage 1 shards optimizer state; Stage 2 also shards gradients; Stage 3/FSDP shards parameters and gathers them when needed. Activation memory remains a distinct budget, addressable with checkpointing and sequence parallelism.

Tensor, pipeline, expert, and context parallelism split different axes of computation. Combining them can make very large models trainable, but raises synchronization costs, communication volume, fault recovery, and configuration complexity. More GPUs are useful only when per-device work and topology justify the extra coordination.

The quantity to optimize is *tokens processed to an accepted quality level per unit money/time*, not nominal GPU count.

## What matters for consider

The three-stage protocol is not distributed neural-network training. Its distributed analogue is controlled composition of multiple inference workers with exact handoff identities. A judge that silently sees only two of three candidates invalidates the selection claim even if the response parses.

If consider eventually trains its own generator or judge, sharding and deterministic checkpoint identities matter. Until then, this lecture is a systems reference, not a prerequisite for testing the central hypothesis.

## Proposed experiment

Use a fixed input and a reproducible replay fixture with stage outputs named by cryptographic digests. Omit, duplicate, reorder, or corrupt a candidate artifact. Accept only the cases permitted by the declared candidate ordering and completeness contract; every rejected case should have an explicit diagnostic.
