# CS336 Spring 2026 — language-model training, filtered for consider

These are **technical working notes**, not a course-completion claim. They cover the 18 public recordings of Stanford's Spring 2026 [CS336: Language Modeling from Scratch](https://cs336.stanford.edu/) and explicitly flag the scheduled guest lecture whose source is missing. The official [YouTube playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rMqXOcazWaTUHhq-yembLCV) and [lecture materials](https://github.com/stanford-cs336/lectures) are primary references.

**Reading order for this repository:** [12 — Evaluation](12-evaluation.md) → [10 — Inference](10-inference-serving.md) → [15 — SFT/RLHF](15-sft-rlhf-dpo.md) → [16 — RLVR](16-rl-verifiable-rewards.md) → [14 — Data preparation](14-data-processing-dedup-mixing.md) → [experiment design](consider-evaluation-design.md).

## Lecture index

| Scheduled session | Technical topic / note | Importance to consider |
|---|---|---|
| 01 | [Overview, tokenization](01-overview-tokenization.md) | Medium: precise input/output and token costs |
| 02 | [Tensors and compute accounting](02-tensors-cost-accounting.md) | Medium: whole-run cost control |
| 03 | [Architecture and hyperparameters](03-architecture-hyperparameters.md) | Medium: context, judge/model choices |
| 04 | [Attention alternatives, MoE](04-attention-alternatives-moe.md) | Medium: long-context limits; error diversity |
| 05 | [GPU and TPU hardware](05-accelerator-hardware.md) | Low now: only if self-hosting/training |
| 06 | [GPU kernels and benchmarking](06-triton-kernels-benchmarking.md) | Low now: measurement discipline |
| 07 | [Parallelism I](07-distributed-parallelism-i.md) | Low now: concurrency and complete stage handoffs |
| 08 | [Parallelism II](08-distributed-parallelism-ii.md) | Low now: artifact identity and optional training |
| 09 | [Scaling laws I](09-scaling-laws-i.md) | Medium: model size vs pipeline cost |
| 10 | [Inference](10-inference-serving.md) | **High:** prefill/decode, cache, latency, spend |
| 11 | [Scaling laws II](11-scaling-laws-practice.md) | Medium: transfer and experiment comparability |
| 12 | [Evaluation](12-evaluation.md) | **Highest:** independent proof of improved answers |
| 13 | [Training data sources](13-pretraining-data-sources.md) | High: provenance, shared errors, contamination |
| 14 | [Processing/deduplication/data mixing](14-data-processing-dedup-mixing.md) | **High:** holdouts and correlated candidates |
| 15 | [Supervised/preference post-training](15-sft-rlhf-dpo.md) | **High:** preference ≠ truth; judge bias |
| 16 | [Verifiable-reward RL](16-rl-verifiable-rewards.md) | **High:** independent oracles, reward hacking |
| 17 | [Multimodal models](17-multimodal-language-models.md) | Low now: text-only contract; future visual inputs |
| 18 | [Daniel Selsam guest session](18-daniel-selsam-source-gap.md) | **SOURCE GAP — no public lecture verified** |
| 19 | [Dan Fu guest talk — inference](19-guest-dan-fu-inference.md) | High: serving costs and systems tradeoffs |

The publicly indexed video sequence numbers Dan Fu as its eighteenth recording. The official course calendar numbers that **guest session 19** because the Daniel Selsam session is listed in the schedule without a verifiable video/material source.

## The application: not another introductory course

The top-level [consider protocol](../../README.md) calls a model to Generate different complete answers, calls a judge to Contrast them under fixed criteria, and calls a selector to Choose a candidate or none. The notes distinguish what must be learned about training language models from what can be **tested today without training one**. The central proposed question is whether this separation improves true answer quality per full-pipeline dollar and per minute.

Begin with [the experimental design](consider-evaluation-design.md). It defines baselines, candidate error correlation, role and order controls, independently checked correctness, abstentions, sample provenance, and negative acceptance fixtures. Nothing in these notes demonstrates a performance gain by itself.

## Source and review status

Prepared on 2026-10-17 from Stanford's public syllabus and lecture-material index, the published recordings' metadata, and independently prepared, [transcript-derived notes for the 2026 recordings](https://github.com/narain1/stanford-cs336-2026-notes). These files are **original concise application-focused syntheses** rather than copies of the source study notes. Full videos were **not independently watched and verified end to end** for this curation; recording-specific details should be checked against the linked original before treated as definitive. The Dan Fu note rests especially on its recording-derived study note, not on an official transcript or provided slides.

The [FP job request](https://github.com/isomorphisms/flexible-pipes/issues/149) records the desired acquisition → extract → relate → verify → publish contract. This candidate is not an FP runtime execution receipt, not independent ai-ci acceptance, and not permission to merge directly.

For the alternative MIT material, see [course source status](mit-course-source-status.md).
