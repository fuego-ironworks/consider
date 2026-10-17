# 17 — Multimodal language models

**Recording:** [lecture 17](https://www.youtube.com/watch?v=26FtD08ZpOU) · **Primary materials:** [lecture_17.py](https://github.com/stanford-cs336/lectures/blob/main/lecture_17.py) · **Study reference:** [transcript-derived notes](https://github.com/narain1/stanford-cs336-2026-notes/blob/main/lecture-17.md).

## Technical notes

A text-only autoregressive LM receives token IDs; a vision-language system must connect visual information to the sequence model. CLIP-style contrastive learning trains image and text encoders to prefer matched pairs. SigLIP uses a different pairwise loss. A common VLM design has a vision encoder, a projector or adapter, and an LM that consumes projected visual embeddings alongside textual tokens.

Resolution and token budget are central constraints. A single resized image can erase essential small features. Dynamic resolution, multiple crops, or patch selection improve coverage but increase context tokens and prefill cost. Alternative architectures discretize images into token sequences; they introduce reconstruction and representation tradeoffs.

Multimodal evaluations need modality-specific correctness criteria. A fluent text answer can be wrong about the supplied image, diagram, table, or spatial relation.

## What matters for consider

The current README specifies a text-answer protocol. Do not silently claim the model judge has inspected an image if it only received a textual description. If multimodal candidates are supported later, stage artifacts must bind the source image identity, transformations, and model-specific representations where observable.

A candidate-generation pipeline using diagrams or scanned math will need an external validation path; independent image analysis is not equivalent to comparing the candidate descriptions.

## Proposed experiment

For a future multimodal extension, test image questions requiring small labels, spatial relations, and mathematical diagrams, plus counterexamples with misleading text descriptions. Verify that every stage receives the same original image or an explicitly identified transform. For the current text-only scope, defer this work.
