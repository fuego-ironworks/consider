# Scheduled lecture 19 / public recording 18 — Dan Fu: inference systems

**Recording:** [Stanford CS336, Dan Fu guest lecture](https://www.youtube.com/watch?v=9EEm4iMAF5s) · **Primary source:** video; official schedule lists the session on June 3, 2026. **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-18.md). No official PDF or code file is linked for this guest talk.

## Technical notes

The lecture approaches LM inference as full-stack engineering rather than merely an optimized matrix multiplication. A request's lifetime includes request preparation, prefill, state creation, token-by-token decode, caching, batching, routing, and completion. Prefill and decode have different work shapes and bottlenecks; an architecture or scheduler optimal for one may be poor for the other.

KV-cache reuse and prefix caching can avoid repeating work, but depend on exact prefix identity, model/configuration, context isolation, and available memory. Continuous batching increases accelerator use while creating complex queueing and latency tradeoffs. Systems may route requests based on cached-prefix locality or separate prefill-heavy and decode-heavy work.

Fusing many small decode kernels into larger coordinated execution units (“megakernels”) can reduce launch and scheduling overhead; specialization increases engineering complexity. The cited recurrent/looped-model ideas illustrate that model architecture and serving systems can co-evolve rather than being separate fixed layers.

**Source limitation:** This is a condensed conceptual reading of an unofficial transcript-derived note, not a verified quotation or complete review of the recorded talk.

## What matters for consider

The three-stage protocol has repeated prefixes and repeated long candidate text. Measure whether a provider actually offers prompt-prefix caching and account for its rates; never presume caches are shared between unrelated model identities, requests, or users. Compare separate short candidate generations with a single longer generation under the same total cost and latency.

## Proposed experiment

Record time to first token, output throughput, cache-hit evidence if exposed, and queueing delay for each stage. Compare ordinary versus cache-aware request assembly without altering visible stage inputs; require byte-identical inputs and the same held-out answer-quality acceptance.
