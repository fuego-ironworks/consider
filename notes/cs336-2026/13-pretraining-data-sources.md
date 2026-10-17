# 13 — Pretraining data: sources, acquisition, provenance

**Recording:** [lecture 13](https://www.youtube.com/watch?v=-qm0ln33G24) · **Primary materials:** [lecture_13.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_13.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-13.md).

## Technical notes

A model is not trained on an abstract “Internet.” It is trained on a particular sampled, filtered, serialized mixture: crawled HTML and documents, books, Wikipedia, articles, code, mathematical text, and more. Examples in the lecture's history include WebText, C4, The Pile, RefinedWeb/FineWeb, Dolma, and code-oriented corpora. Acquisition, legal status, attribution, deduplication, and quality controls affect both what the model knows and what it repeats.

Common Crawl captures a changing subset of the public web; a crawl is neither a complete population census nor a uniform sample. GitHub, arXiv, and other structured sources require distinct extraction and provenance policies. Copyright/license considerations and practical access constraints differ by source. Model output quality reflects these mixture choices as well as model capacity.

Dataset metadata should separate source, capture date, extraction transform, revision, filtering decisions, and train/test split. Reproducibility requires more than a memorable corpus name.

## What matters for consider

Shared source material can create correlated hallucinations across candidate models. A judging model trained on similar sources can repeat the mistake rather than detecting it. Use retrieved ground truth or independent executable oracles when possible.

Source provenance is also necessary for a useful held-out set. For factual questions, record the date on which the answer was true. For software/code questions, pin repository and dependency revisions. For specialized mathematics, do not let a polished restatement count as a proof.

## Proposed experiment

Construct a stratified evaluation corpus with math, code, factual/source-grounded, and adversarially misleading prompts. Record provenance and avoid prompts drawn verbatim from model-development demonstrations. Compare performance by class and time period, not only aggregate scores. Reserve an untouched final test set.
