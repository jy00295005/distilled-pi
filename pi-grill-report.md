# PI Grill Report

## 0. Input Type and Stage

Input type: Research Harness

Stage: Experiment design stage

PI Focus: Experiment design

## 1. PI's Main Concern

The project is now much more defensible, but only if the experiment proves critique-content dependence. The current paper lives or dies on whether valid critiques provide measurable value beyond answer-only and corrected-answer-only supervision.

This is not ready for a broad self-correction claim. It is ready for a diagnostic pilot.

## 2. The Questions That Matter

### Q1. What exact result pattern would make you say "critique content causally matters"?

- Why this matters: Without a pre-specified result pattern, any small gain can be overinterpreted as self-correction.
- Current answer quality: Strong.
- Stronger answer: Keep the proposed thresholds as provisional decision rules, then adjust them based on pilot variance before the main experiment.
- Evidence needed: The following thresholds are provisional and should be adjusted based on pilot variance: valid critique training beats answer-only finetuning by around 3 absolute points in correction success rate, beats corrected-answer-only supervision by around 2 points, loses at least half its advantage under unrelated or misleading critiques, and retains 30-50% of the valid-critique advantage on unseen error types or a held-out math set.

### Q2. If answer-only finetuning matches critique training, what remains of the paper?

- Why this matters: This baseline can kill the self-correction framing immediately.
- Current answer quality: Strong.
- Stronger answer: Use the fallback title and claim as a real planned paper path, not a backup excuse.
- Evidence needed: A clean negative comparison showing that synthetic critiques mostly behave like corrected-answer supervision under GSM8K-style correction training, plus a reusable diagnostic protocol for future critique-training work.

### Q3. How will you prevent critique leakage from making the result trivial?

- Why this matters: If GPT-4-level critiques contain corrected reasoning or answer hints, the model may be learning hidden solution supervision, not error feedback.
- Current answer quality: Strong.
- Stronger answer: Make leakage control part of the experimental design, not a post-hoc audit.
- Evidence needed: Separate critique-only, correction-only, critique-plus-correction, and leaky-critique conditions; constrain critiques to short error-localization feedback; audit sampled critiques for answer-revealing information.

## 3. Claim Risk Assessment

High but controlled. The phrase "learn to self-correct" is still too strong for the current project unless the diagnostics pass. The safer claim is about critique-content dependence in correction behavior.

## 4. Research Risk Heatmap

| Risk Type | Level | Reason |
| --- | --- | --- |
| Claim Risk | HIGH | The title still implies robust self-correction, which the pilot may not support. |
| Novelty Risk | MEDIUM | The method is not novel by itself; the diagnostic protocol is the contribution. |
| Experiment Risk | MEDIUM | The ablation plan is strong, but threshold rules are provisional and must be calibrated against pilot variance. |
| Execution Risk | MEDIUM | Generating non-leaky critiques and meaningful misleading critiques will be delicate. |
| Publication Risk | MEDIUM | Positive or negative results can both be publishable if framed as diagnostic empirical analysis. |

## 5. Weakest Claim

Small language models learn to self-correct through synthetic critiques.

This claim should not be used as the main paper claim until the diagnostic conditions pass.

## 6. Safer Claim

Synthetic critiques can improve correction behavior in small language models beyond corrected-answer supervision only when critique content carries useful, non-leaky error information and the model's advantage degrades under corrupted or misleading critiques.

## 7. Novelty Risk

The novelty is not "training on synthetic critiques." That will look like another synthetic-supervision setup.

The defensible novelty is the diagnostic separation between:

- critique-conditioned correction;
- corrected-answer supervision;
- hidden solution leakage;
- template-like answer rewriting;
- partial transfer to unseen error types.

## 8. Experiment / Evidence Risk

The evidence path is now credible, but only if the ablations are clean.

The two most important risks:

1. The critique may leak corrected reasoning.
2. The provisional thresholded gains may be too small or unstable across seeds/models.

The pilot should therefore test leakage and variance early, not after the full experiment is built.

## 9. Baseline / Comparison Gaps

Minimum comparisons:

- Answer-only finetuning.
- Corrected-answer-only supervision.
- Valid critique training.
- Critique-only condition.
- Critique-plus-correction condition.
- Generic critique replacement.
- Unrelated critique replacement.
- Misleading critique.
- Leaky critique.

Optional but useful:

- Self-refine prompting.
- Verifier reranking.

## 10. Alternative Explanations

- The model learns answer-revision templates.
- Gains come from corrected-answer exposure.
- GPT-4-level critiques leak hidden solution supervision.
- GSM8K-specific patterns explain most improvement.
- The model learns to follow feedback format without using semantic critique content.
- Misleading critiques are too artificial to test real robustness.

## 11. What to Fix Before Next Meeting

1. Stop using "learn to self-correct" as the main claim until diagnostics pass.
2. Write the provisional thresholded decision rule into the experiment plan, with a note that it should be adjusted based on pilot variance.
3. Define the critique-generation constraints precisely.
4. Add the four evidence conditions: critique-only, correction-only, critique-plus-correction, leaky critique.
5. Build a small critique leakage audit rubric.
6. Decide whether the paper is framed upfront as diagnostic empirical analysis.

## 12. Next 7-Day Plan

Day 1: Write the pre-registered provisional decision rule with the 3-point, 2-point, half-drop, and 30-50% transfer thresholds, explicitly noting that these should be adjusted based on pilot variance.

Day 2: Draft the critique-generation prompt with explicit no-answer, no-full-solution, and error-localization-only constraints.

Day 3: Generate a small GSM8K pilot set across valid, generic, unrelated, misleading, and leaky critique conditions.

Day 4: Manually audit a sample of critiques for answer-revealing leakage.

Day 5: Run a small pilot on one 1B-3B model with answer-only, correction-only, and valid critique training.

Day 6: Add unrelated/misleading critique tests and check whether the valid-critique advantage drops.

Day 7: Decide whether the next version is a critique-content paper, a negative empirical analysis paper, or not worth scaling yet.

## 13. Updated Questions for PI

1. Are the proposed provisional thresholds strong enough to claim critique-content dependence after accounting for pilot variance?
2. Should the title remove "self-correct" until after the pilot?
3. Is the leaky-critique condition necessary for the main paper or just an appendix control?
4. Would a negative result with this diagnostic protocol be publishable in the target venue?

## 14. Context for Future AI Work

Future agents should preserve the diagnostic framing. Do not expand the claim to robust self-correction unless the experiment shows:

- valid critiques outperform answer-only and corrected-answer-only supervision by the pre-specified margins;
- unrelated or misleading critiques remove a substantial share of the valid-critique advantage;
- some advantage transfers to unseen error types or a held-out math set;
- non-leaky critiques still work under audit.

If those conditions fail, pivot to the fallback paper:

**Synthetic Critiques Mostly Behave Like Corrected-Answer Supervision for Small Language Models.**

## 15. PI Verdict

Ready for a pilot experiment. Not ready for a full paper draft.

Currently, this is an empirical diagnostic paper, not a method paper. Stop using the strong self-correction claim until the diagnostics pass. The project is worth testing because you now have a fast way to kill it: answer-only equivalence, critique corruption insensitivity, or critique leakage.
