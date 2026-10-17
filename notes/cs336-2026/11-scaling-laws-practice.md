# 11 — Scaling laws II: optimizers and hyperparameter transfer

**Recording:** [lecture 11](https://www.youtube.com/watch?v=vTfEyOyzV9E) · **Primary materials:** [lecture_11.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_11.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-11.md).

## Technical notes

Scaling law fits are only useful if experimental recipes stay comparable. Model width and depth, batch size, optimizer, token distribution, precision, regularization, and learning-rate schedule can interact. A model trained less than optimally at small size may suggest the wrong large-scale trend.

Warmup–stable–decay schedules separate learning at a useful stable rate from the final decay phase. This can be more practical for experiments at several token budgets than running a fresh cosine schedule each time. Muon is an optimizer design using matrix-aware updates; comparisons against AdamW require comparable tuning effort and budgets, not a single convenient parameter setting.

Maximum Update Parameterization (μP) aims to preserve training behavior when width changes by choosing consistent initialization and learning-rate scalings. It is an empirical/analytic discipline with boundary conditions, not a proof that every architecture and data regime admits transferable hyperparameters.

## What matters for consider

The parallel issue is experimental comparability: a prompt protocol may be tuned on one model, budget, and task distribution, then silently fail elsewhere. Comparisons must record exact model revisions, decoding parameters, criteria, and prompt text.

If a self-hosted judge is ever fine-tuned, start with small scale but validate which hyperparameters and behaviors actually transfer. Do not make training a prerequisite for evaluating an external-model protocol.

## Proposed experiment

Pre-register one set of fixed criteria and prompts. Evaluate on one model, then transfer *unchanged* to other model sizes and families. Compare with separately tuned variants and disclose tuning expense. This tests whether consider's gain is stable protocol structure or overfit prompt engineering.
