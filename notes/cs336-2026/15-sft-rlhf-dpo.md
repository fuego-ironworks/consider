# 15 — Mid-training, supervised fine-tuning, and preferences

**Recording:** [lecture 15](https://www.youtube.com/watch?v=2oH6PWPrYFo) · **Primary materials:** [lecture_15.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_15.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-15.md).

## Technical notes

Pretraining learns a distribution over sequences; instruction supervised fine-tuning (SFT) teaches response format, conversational behavior, and compliance with particular tasks. The optimization is still a log-probability objective, but on selected instruction–response data, sometimes masking non-answer tokens. More instructions are not necessarily better if demonstrations are low quality or redundant.

Preference training uses comparisons between responses. RLHF often fits a reward model from such comparisons and then optimizes the generation policy, for example via PPO. Direct Preference Optimization (DPO) expresses a related preference objective directly through policy/reference log-probability ratios, without an explicit separate reward-model training phase. Neither makes preference labels an oracle of factual truth.

Optimizing a learned reward can exploit its blind spots. Style, verbosity, confidence, sycophancy, safety refusals, and actual task competence are distinguishable dimensions. Reward-model optimization may improve a headline preference metric while degrading calibration or diversity.

## What matters for consider

Contrast and Choose are a form of *test-time* selection, not equivalent to SFT/RLHF. A pretrained or post-trained judge may be systematically biased toward polished language or answers reminiscent of its own training distribution. Do not tune the selector only to imitate an LLM judge.

Fixed criteria should distinguish correctness, completeness, cost, unsupported assertions, and uncertainties. Keep criteria independent of candidate identity. Make a distinction between an answer that is stylistically preferable and one that satisfies a verifiable requirement.

## Proposed experiment

Include paired answers in which one is correct but terse, the other eloquent and wrong; also correct lengthy versus correct short responses. Reverse order and anonymize model origins. Audit whether Contrast flags the actual flaw and whether Choose follows the comparison or reintroduces its own style bias. Calibrate against external task labels.
