# Experiment design — Does Generate → Contrast → Choose improve correctness?

## Hypothesis

The public [consider README](../../README.md) defines three **separate calls**:

1. **Generate** at least two meaningfully different, complete candidates, without evaluating or selecting.
2. **Contrast** the candidates under the *same* fixed criteria, identifying factual errors, unsupported claims, omissions, strengths, and uncertainty, without choosing.
3. **Choose** the best candidate or none, without inventing an unexamined synthesis.

The hypothesis is not that writing three prompts makes a better model. It is that exposing competing answers and a separate comparison stage can improve **independently established task success** at an acceptable *whole-pipeline* cost.

## Baselines and controls

- One direct answer with the same model; one direct answer with equal total token/cost budget.
- Multiple candidates with a simple fixed selector, when objective validation exists.
- Generate → Choose without the separate Contrast stage.
- Generate → Contrast → Choose, with both same-model and independent judge/selector variants.
- Independent model families versus repeated samples of one model; hold the total budget fixed.
- Several candidate counts and sampling temperatures, with frozen criteria.
- Shuffle anonymous candidate order/labels to measure position and presentation bias.

## Evaluation data

Use disjoint development and held-out sets of:
- short mathematical and numerical tasks with independent exact or independently computed answers;
- code tasks with runnable assertions, known-bad mutations, and effect/scope checks;
- factual questions with dated primary sources and explicit unknown/ambiguous labels;
- deliberately adversarial pairs (terse correct vs verbose incorrect, common false premises, duplicated plausible errors);
- long-context questions, including ones where the answer occurs early or late.

Exclude ambiguous ground truth from the numeric accuracy denominator unless an explicit adjudication rule has been defined *before* seeing candidate outputs. Preserve rejected/unknown cases in the corpus.

## Measurements

For each task, retain original question, criteria, generator/judge/selector identities and immutable revisions when available, full prompts, sampling parameters, raw candidates, comparison, decision, oracle result, tokens, provider invoice/cost, per-stage timestamps, errors/retries, and output hashes. Never replace a failed candidate with an unreported regenerated one.

Report task-level paired success, uncertainty intervals, *conditional selection accuracy when some candidate is correct*, false acceptances, false rejections by the none option, abstention/coverage, cost, and p50/p95 latency. Record an error-by-candidate matrix to assess common-mode mistakes. Evaluate *quality versus cost* rather than quality in isolation.

A tempting upper bound, \(1-(1-p)^n\), for finding at least one correct candidate presumes independent Bernoulli candidates and a common success probability \(p\). Correlated errors break that formula, and a perfect candidate in the set gives no guarantee that the selector will recognize it.

## Fail-closed and negative acceptance tests

A passing harness must reject:
- no visible candidates, missing candidate, or a response that rewrites a candidate;
- a Contrast stage that selects rather than compares;
- a Choose stage that creates an unexamined new answer;
- missing data/criteria or a different criteria set for each candidate;
- an invented external correctness oracle or an unverified source;
- a claimed success with unmeasured output quality;
- stale or mismatched stage artifact revisions/digests;
- order changes that were not declared by the run spec.

Every checker should have a realistic positive example and a known-bad example that causes a targeted failure. “Model judge preferred it” is evidence of preference, never a substitute for the external oracle.

## Priorities learned from CS336

1. **Lecture 12 evaluation:** define real outcomes and controls.
2. **Lecture 10 and guest Dan Fu inference:** account for full stage cost/latency.
3. **Lectures 15–16 post-training:** understand judge/reward-model bias and gaming.
4. **Lectures 13–14 data:** prevent contamination and correlated errors.
5. **Lectures 1, 3–4:** tokenization, long context, model-family effects.
6. **Lectures 2, 5–9, 11:** training/systems only when self-hosting or model training is actually justified.
7. **Lecture 17:** multimodality only when consider acquires a real image-input contract.

## Provenance and execution boundary

These are proposed experiments, not executed benchmarks. No performance win, trained judge, full-video audit, FP runtime receipt, or ai-ci PASS is asserted. [FP job registration request](https://github.com/isomorphisms/flexible-pipes/issues/149) tracks an independently accepted curation path.
