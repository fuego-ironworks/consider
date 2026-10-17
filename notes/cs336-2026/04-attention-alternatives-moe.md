# 04 — Attention alternatives and mixtures of experts

**Recording:** [lecture 4](https://www.youtube.com/watch?v=cKSwj_qZ8Jg) · **Primary materials:** [lecture_04.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_04.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-04.md).

## Technical notes

Ordinary causal attention computes a masked, row-normalized form of

\[
\operatorname{softmax}(QK^\top/\sqrt{d})V.
\]

The length-by-length attention matrix creates quadratic work in sequence length. Linear attention variants replace the softmax kernel with a feature-map factorization and exploit associativity to maintain smaller state, though they alter memory and retrieval behavior. Gated recurrent/linear mechanisms and hybrid attention seek better long-context tradeoffs. Sparse attention retains a subset of exact key–query interactions.

A mixture-of-experts (MoE) layer routes each token through only a subset of specialized feed-forward networks. This decouples total parameter capacity from active per-token work, but adds load-balancing, expert collapse, all-to-all communication, and router-stability problems. Sparsity is not free capacity.

## What matters for consider

Candidate diversity must be measured on **errors**, not on model family labels or superficial wording. Two models with different architectures may share pretraining data, post-training objectives, and false claims. Conversely, several samples of one model may supply useful independent solution approaches.

A contrasting judge should identify contradictions and verifiable claims even when the texts are long or their ordering changes. It should not quietly treat longer explanations as more reliable.

## Proposed experiment

For each question, sample multiple candidates from a dense model, a MoE model, and independent model families at comparable budget. Build an error-incidence matrix from external oracles; estimate pairwise failure correlation. Compare selector performance as a function of *actual complementarity*, not simply count of candidates.
