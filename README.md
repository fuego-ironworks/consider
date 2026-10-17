# consider

**Visible, three-stage answer selection around any language model.**

`consider` does not require access to a model's hidden reasoning or a special reasoning mode. It retains the outputs of three separate calls in an inspectable staging area.

## 1. Generate

Given question `Q`, a fixed set of criteria `C`, and `n ≥ 2`, ask a model to produce **n substantially different, complete answers**. This is one stage; do not evaluate or select candidates yet.

Output: `candidates.md`, with answers A, B, C, … .

## 2. Contrast

Pass `Q`, `C`, and the candidate answers to the same model or a separate judge. Compare each answer under the **same criteria**. Identify factual errors, unsupported assertions, omissions, and relative strengths. Do not select the winner at this stage.

Output: `comparison.md`.

## 3. Choose

Pass `Q`, `C`, the candidates, and the comparison to a selector. Pick the best candidate and explain the choice briefly. If all are inadequate, choose `none`. Do not silently replace the chosen answer with an unexamined synthesis.

Output: `decision.md`.

## Staging area

```
consider-run/
  request.md
  candidates.md
  comparison.md
  decision.md
```

All intermediate answers and judgments are **visible**. A user can edit any stage, re-run subsequent stages, or swap generators and judges.

## Three prompts

1. **Generate:** “Give three meaningfully different, complete answers to this question. Label them A, B, C. Do not evaluate them.”
2. **Contrast:** “Evaluate answers A–C using the supplied criteria. Explicitly compare them; note errors and uncertainties. Do not choose one.”
3. **Choose:** “Using the question, criteria, answers, and comparison, choose the best answer or none. Output the answer and a short justification.”

These are separate calls even if all use the same inexpensive or Instant-style model.

## Testing the hypothesis

Compare this protocol to an ordinary one-call answer on held-out tasks; measure task correctness, cost, and latency. Multiple candidates can share errors, and model judges can be wrong or favor fluency over accuracy. Ground-truth tests or external checks should override model preference when available.

**Goal:** Improve answers without requiring access to hidden reasoning. **Not a guarantee:** Performance depends on candidate diversity and selection accuracy.
