# 16 — Reinforcement learning from verifiable rewards (RLVR)

**Recording:** [lecture 16](https://www.youtube.com/watch?v=dIFAi87Ws4E) · **Primary materials:** [lecture_16.pdf](https://github.com/stanford-cs336/lectures/blob/main/lecture_16.pdf) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-16.md).

## Technical notes

A policy \(\pi_\theta(y\mid x)\) can be trained to maximize expected reward:

\[
J(\theta)=\mathbb{E}_{x,y\sim\pi_\theta}[R(x,y)].
\]

The log-derivative identity turns this into a reward-weighted sum of token log-probability gradients; baselines reduce gradient variance. PPO constrains policy updates to avoid moving too far from the rollout distribution. GRPO uses comparisons among multiple sampled completions of the same prompt to estimate relative advantages instead of training a separate value function. This introduces its own normalization and length biases.

RL from automatically *verifiable* outcomes—correct mathematical answers, passing tests, formal checker results—can supply stronger feedback than a learned preference proxy. But verification has a specification problem: passing a weak test suite, an answer string check, or a manipulable harness is not equal to satisfying the intended task. Agentic models may actively exploit incomplete checks.

## What matters for consider

A model judge has a structural similarity to a learned reward: it can mistake persuasive explanations for correct ones. External oracles should override Contrast/Choose where possible, and **none** should be a legitimate decision when no candidate meets the declared criteria.

Do not interpret reasoning-looking text as evidence of reasoning correctness. A system can optimize the appearance of reasoning without improving the external objective.

## Proposed experiment

Create true and deliberately broken positive/negative cases for mathematical and code tasks. Some wrong answers should pass naive string matching or shallow tests; independent richer tests must reject them. Report how often the selector chooses an apparently high-scoring candidate that fails the stronger oracle. Preserve rejected intermediate artifacts.
