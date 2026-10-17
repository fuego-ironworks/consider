# 12 — Evaluation: operational definitions and real oracles

**Recording:** [lecture 12](https://www.youtube.com/watch?v=JpAxdTWQJxM) · **Primary materials:** [lecture_12.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_12.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-12.md).

## Technical notes

A language-model metric is an operational definition of some goal, not the goal itself. Perplexity measures held-out token prediction:

\[
\mathrm{PPL}=\exp\left(-\frac1T\sum_{t=1}^{T}
\log p(x_t\mid x_{<t})\right).
\]

It does not directly establish whether a model answers a specific mathematical or programming question correctly. Exact-match accuracy, pass@k, execution tests, human preference, LLM-as-judge scores, and cost-adjusted performance answer different questions. Benchmark contamination, answer-format sensitivity, limited samples, and answer selection can change conclusions.

LLM judges can be biased toward verbosity, familiarity, style, position in a pair, or their own model family. A plausible, confident explanation is not an external correctness witness. Evaluations should distinguish directly measured task success from learned preference scores.

## What matters for consider

This is the **first lecture to operationalize**, because the repo's entire claim is comparative. Measure:
- final chosen-answer correctness with a task-specific independent oracle;
- single-answer baseline correctness at the same total inference spend;
- selector error even when at least one candidate is correct;
- the **none** decision's coverage, false abstention, and false acceptance;
- cost, latency, input length, and whether answer order changes the decision.

If each candidate is independently correct with probability \(p\), the probability at least one of \(n\) is correct is \(1-(1-p)^n\). This optimistic formula fails when errors are correlated **and** does not imply the selector can identify the correct one.

## Proposed experiment

Use math tasks with exact or independently reproducible solutions; code tasks with executable positive and known-bad test cases; and factual tasks with dated source evidence. Shuffle candidate labels and order, hide generator identity from the judge, retain all judgments and disagreements, and report paired per-question outcomes. A pipeline win without an oracle is only a preference claim.
