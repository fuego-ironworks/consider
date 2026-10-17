# 06 — Kernels, Triton, and performance measurement

**Recording:** [lecture 6](https://www.youtube.com/watch?v=xnDHaNUvHBg) · **Primary materials:** [lecture_06.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_06.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-06.md).

## Technical notes

Triton exposes block-oriented GPU kernels. The lecture works from simple elementwise operations to fused softmax, reductions, and tiled matrix multiplication. Key performance issues include occupancy, register spilling, bank conflicts, coalescing, warp divergence, and insufficient parallel blocks.

A valid benchmark must account for asynchronous execution. Warm up the device, synchronize around the interval being measured when needed, distinguish compilation from steady-state execution, and run enough repeats to expose variance. Compare like-for-like tensor shapes and dtypes. Inspect a profiler before explaining an unexpected result.

Fusing an activation into a preceding operation can reduce extra reads and writes. The benefit depends on shape, cache behavior, precision, and launch overhead: specialized kernels should not be assumed faster by definition.

## What matters for consider

The analogous engineering discipline is to time and validate *the exact accepted pipeline*. Counting only the selector call, ignoring preprocessing or provider retry delays, is as misleading as timing a GPU kernel without synchronizing.

Do not invest in Triton until the need to self-host substantial models is established. The nearest operational wins are fewer redundant tokens, prefix reuse where safe, and correctly scoped parallel requests.

## Proposed experiment

Compare sequential and concurrent candidate generation under a declared rate limit, then the full three-stage workflow. Record queueing, latency, timeout, cost, and output correctness. A changed serving implementation only qualifies after the same held-out correctness tests pass.
