# Example PI Grill Report

Filename: `pi-grill-report.md`

```markdown
# PI Grill Report

## 0. Input Type and Stage
Input type: Research Harness

Stage: Proposal stage

## 1. PI's Main Concern
The project currently risks overclaiming "self-correction" when the proposed evidence may only show critique-conditioned answer editing.

## 2. The Questions That Matter

### Q1. How will you prove the model learned error diagnosis rather than critique-style rewriting?
- Why this matters: Without this distinction, the core claim collapses into supervised revision.
- Current answer quality: Okay but incomplete.
- Stronger answer: Add error localization, unseen error types, and corrupted critique robustness.
- Evidence needed: Diagnostic results showing the model uses critique content rather than merely following critique format.

### Q2. If answer-only finetuning or verifier reranking wins, what remains of the paper?
- Why this matters: A simpler baseline can kill the method contribution.
- Current answer quality: Missing.
- Stronger answer: Reframe the contribution as a diagnostic analysis of when critiques help or fail.
- Evidence needed: Baseline comparison plus analysis of failure regimes.

## 3. Claim Risk Assessment
High. The phrase "learn to self-correct" is stronger than the current experiment can safely support.

## 4. Research Risk Heatmap

| Risk Type | Level | Reason |
| --- | --- | --- |
| Claim Risk | HIGH | "Self-correction" is stronger than current evidence. |
| Novelty Risk | MEDIUM | Method novelty is weak unless diagnostics are central. |
| Experiment Risk | HIGH | Accuracy gains may not prove critique use. |
| Execution Risk | MEDIUM | Misleading-critique and error-type diagnostics need careful construction. |
| Publication Risk | MEDIUM | The paper is publishable only if framed as diagnostic analysis or backed by strong causal evidence. |

## 5. Weakest Claim
Small models learn self-correction through synthetic critiques.

## 6. Safer Claim
Synthetic critique training can improve critique-conditioned answer revision, but genuine self-correction requires additional evidence from error localization, OOD transfer, and robustness to misleading critiques.

## 7. Novelty Risk
The method may look like standard synthetic-feedback finetuning unless the paper foregrounds the diagnostic separation between revision, imitation, and error diagnosis.

## 8. Experiment / Evidence Risk
Accuracy improvements alone are insufficient. The project needs one diagnostic experiment that can falsify the stronger claim.

## 9. Baseline / Comparison Gaps
The minimum baseline set should include answer-only finetuning, verifier reranking, and critique-free correction.

## 10. Alternative Explanations
- The model learned the surface form of critique-conditioned editing.
- The stronger model's critique leaks answer information.
- Gains come from extra supervision rather than self-correction ability.

## 11. What to Fix Before Next Meeting
1. Define self-correction operationally.
2. Add the strongest simple baseline.
3. Add one diagnostic test separating error diagnosis from rewriting.
4. Narrow the claim in the abstract and harness.

## 12. Next 7-Day Plan
Day 1: finalize operational definition.
Day 2-3: implement answer-only finetuning baseline.
Day 4-5: create corrupted critique diagnostic set.
Day 6: run a small pilot.
Day 7: update harness and prepare a revised PI pitch.

## 13. Updated Questions for PI
1. Is this more defensible as an empirical analysis paper than a method paper?
2. Which baseline would you consider decisive?
3. What diagnostic result would make the self-correction claim credible?

## 14. Context for Future AI Work
Future agents should preserve the narrower claim unless new diagnostics support the stronger self-correction framing.

## 15. PI Verdict
Not ready for a full paper draft. Ready for a pilot experiment if the student stops using the strong self-correction claim until the diagnostics pass.
```
