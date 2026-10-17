# 09 — Scaling laws I: model size, data, and compute

**Recording:** [lecture 9](https://www.youtube.com/watch?v=Q15rhEWZPQ4) · **Primary materials:** [lecture_09.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_09.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-09.md).

## Technical notes

Empirical language-model loss often follows approximate power laws in a controlled regime. An illustrative model is

\[
L(N,D)\approx L_\infty+A N^{-\alpha}+B D^{-\beta},
\]

with \(N\) parameter count and \(D\) training tokens. The exponents and coefficients are *fitted*, not physical constants. Dense pretraining compute is approximately proportional to \(ND\); a budget constraint determines the compute-optimal allocation between model size and training duration.

Kaplan-style and Chinchilla-style prescriptions differ partly because they use different assumptions and experimental regimes. A fit can become misleading across architecture changes, data repetition, altered filtering, learning-rate schedules, or extrapolation too far beyond observed scales. Training-optimal is not necessarily inference-optimal if one model will serve many requests.

Smooth changes in log loss do not imply that discrete success rates change smoothly. A thresholded downstream task can appear to jump even when underlying probabilities vary continuously.

## What matters for consider

A smaller model can be cost-effective in a three-stage pipeline even if it is weaker in a single call. That is a separate empirical scaling question. Pretraining loss does not measure factuality of the judge, ability to detect mathematical errors, or expected quality of the final chosen answer.

Use Pythia checkpoint families or other fixed model series to separate learning-stage effects from protocol effects, when model weights and runtime are actually available and pinned.

## Proposed experiment

Plot directly answered task accuracy and consider-selected task accuracy **against total spend**, using several models and several generation budgets. Add confidence intervals, abstention rate, and answer length. Test fits on held-out compute/model sizes; do not extrapolate pipeline utility from LM loss alone.
