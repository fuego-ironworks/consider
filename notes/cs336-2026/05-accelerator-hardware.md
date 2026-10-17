# 05 — GPUs, TPUs, and the memory hierarchy

**Recording:** [lecture 5](https://www.youtube.com/watch?v=izZba4UA7iY) · **Primary materials:** [lecture_05.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_05.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-05.md).

## Technical notes

Parallel arithmetic is only part of accelerator performance. GPU throughput depends on streaming multiprocessors, register pressure, shared memory, memory coalescing, divergent execution, occupancy, and global-memory bandwidth. The roofline upper bound is roughly

\[
\text{attainable FLOP/s} \leq
\min(\text{peak FLOP/s},\;\text{bytes/s}\times\text{FLOPs/byte}).
\]

Tiling, operator fusion, mixed precision, and deliberate recomputation trade arithmetic and implementation complexity against expensive memory movement. FlashAttention avoids explicitly materializing the full attention matrix in high-bandwidth memory; it still computes attention and must preserve the required normalization behavior.

TPUs implement different instruction and interconnect choices but face the same basic concerns: utilization, data placement, and network topology.

## What matters for consider

The consider repository is a three-stage *inference orchestration* protocol. It does not need a GPU kernel implementation to establish whether the protocol improves answers. Accelerator engineering only becomes a blocker when self-hosting models, training judges, or meeting serving latency targets.

A1/C67 Android clients can submit and inspect candidates; they should not be treated as credible large-scale training hosts merely because they expose GPUs.

## Proposed experiment

Measure end-to-end latency separately from model-server token generation speed, and attribute overhead to model inference, network, queueing, request assembly, and staging. Optimize the dominant measured term first; never infer a performance bottleneck from an architecture diagram alone.
