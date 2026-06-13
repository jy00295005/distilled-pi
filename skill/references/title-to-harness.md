# /title-to-harness Reference

Use this workflow to turn a rough research title, vague idea, or short context into a structured harness that can feed later research agents and `/pi-grill`.

## Core Contract

Input:

```text
/title-to-harness

Title:
<research title>

Optional context:
<short background, idea, direction, worry, or constraints>
```

If the user only gives a title, start anyway.

Output one harness Markdown artifact named by type:

- `research-harness.md`
- `survey-harness.md`
- `benchmark-harness.md`
- `dataset-harness.md`
- `system-harness.md`
- `position-harness.md`

Do not write a proposal. First triage the title, ask intake questions, check answer quality, optionally ask one targeted follow-up, then generate the harness.

## Workflow

Follow this sequence:

```text
Step 0: Title Triage
Step 1: Broad Intake
Step 2: Answer Quality Check
Step 3: Targeted Follow-up if needed
Step 4: Assistive Completion if user cannot answer
Step 5: Generate Harness Markdown
```

Never ask more than two rounds of questions:

- Round 1: broad intake.
- Round 2: targeted follow-up only if critical information is missing.

If the user is still unsure after the follow-up, generate the harness anyway and mark uncertain parts under `Assumptions and Unknowns`.

## Step 0: Title Triage

First classify the title. Do not jump directly into research design.

Types:

- `Research Paper`
- `Survey / Review`
- `Benchmark`
- `Dataset`
- `System`
- `Position / Perspective`
- `Unclear / Hybrid`

Use these heuristics:

- `survey`, `review`, `taxonomy`, `landscape` -> likely `Survey / Review`
- `benchmark`, `evaluation`, `measuring` -> likely `Benchmark`
- `dataset`, `corpus`, `data` -> likely `Dataset`
- `system`, `framework`, `platform`, `tool` -> likely `System`
- `can X do Y`, `improving X`, `learning to X`, `method for X` -> likely `Research Paper`
- `why`, `should`, `implications`, `perspective` -> likely `Position / Perspective`
- If uncertain, mark `Unclear / Hybrid` and explain the plausible paths.

Output:

```markdown
## Title Triage

Original Title:
...

Likely Type:
...

Confidence:
High / Medium / Low

Reason:
...
```

## Step 1: Broad Intake

Ask the type-specific broad intake questions. Keep the preface short: the point is to make the title safe for a PI meeting.

### Research Paper

```text
1. What failure mode or unresolved phenomenon are you trying to explain or solve?
2. What are the research objects: model, task, data, and setting?
3. What core variable are you introducing, changing, or comparing?
4. What is insufficient about existing methods?
5. What claim do you most want the paper to prove?
```

### Survey / Review

```text
1. What is the survey boundary: what is included and excluded?
2. What taxonomy or organizing framework do you want to propose?
3. What do existing surveys miss?
4. What comparison axes will you use across prior work?
5. What new judgment should readers gain after reading it?
```

### Benchmark

```text
1. What capability, failure mode, or system property do you want to measure?
2. Why are existing benchmarks insufficient?
3. What is the key task, data, or evaluation design?
4. What new finding should the benchmark reveal?
5. What result would show the benchmark has no discriminative power?
```

### Dataset

```text
1. What phenomenon does this dataset capture that existing datasets miss?
2. What are the data sources, annotation method, and quality controls?
3. What research questions or tasks does it support?
4. How is it different from existing datasets?
5. How will you prove this is not ordinary data collection?
```

### System

```text
1. What concrete workflow or technical bottleneck does the system solve?
2. Who are the users, and what is the use case?
3. What are the core design choices?
4. How is it different from existing tools or frameworks?
5. How will you evaluate that it is actually useful?
```

### Position / Perspective

```text
1. What mainstream view are you challenging, refining, or advancing?
2. What is your core position?
3. What types of evidence support it?
4. Where will readers most likely disagree?
5. Is the contribution conceptual clarification, agenda setting, critique, or something else?
```

For `Unclear / Hybrid`, ask the 3-5 questions that distinguish the plausible type paths.

## Step 2: Answer Quality Check

After the user answers, judge whether there is enough to generate a harness.

Check:

- Problem clarity
- Scope clarity
- Claim clarity
- Novelty clarity
- Evidence / experiment clarity
- Falsification clarity
- Risk clarity

Use `Clear`, `Partial`, or `Missing`.

Output a short diagnosis:

```markdown
## Answer Quality Check

Clear:
- ...

Partial:
- ...

Missing:
- ...
```

If most critical items are clear, generate the harness. If 2 or more critical items are missing or vague, ask one targeted follow-up.

## Step 3: Targeted Follow-up

Ask at most one targeted follow-up round.

Rules:

- Ask only the most important 3-5 questions.
- Do not repeat broad intake.
- Do not keep questioning just because the answer is imperfect.
- Each question should close a real hole in claim, novelty, experiment, baseline, falsification, scope, or risk.

Example:

```text
I still cannot generate a safe harness. Four points are missing:

1. Do you mean self-correction during the same inference attempt, or improved post-training accuracy?
2. What is the strongest baseline: answer-only finetuning, verifier reranking, self-refine, or something else?
3. Is the intended contribution a method, an empirical finding, or a diagnostic framing?
4. What result would make you admit the model only learned critique style rather than real correction?
```

## Step 4: Assistive Completion

If the user says they do not know, cannot answer, or is blocked, do not stop. Offer 2-3 candidate assumptions and recommend the safest default.

Format:

```markdown
You may not know yet. Here are usable defaults:

A. Conservative version
...

B. Ambitious version
...

C. Analysis-first version
...

My recommendation:
Use C because it is easier to defend against novelty and falsification questions.
```

Rules:

- Label candidates as assumptions.
- Explain tradeoffs briefly.
- Recommend the safest default.
- Let the user say "use your recommendation".
- If there is no further preference, proceed with the recommended default and mark it explicitly in the harness.

## Step 5: Generate Harness Markdown

Generate the type-specific artifact. Every harness must include assumptions, risks, unknowns, and questions for PI.

Use concise but complete content. Fill sections with research-agent-readable notes, not prose padding.

### Research Harness

Filename: `research-harness.md`

```markdown
# Research Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Research Pitch

## 3. Problem Statement

## 4. Why This Matters

## 5. Research Question

## 6. Core Hypotheses

## 7. Novelty Claim

## 8. Related Work Buckets

## 9. Proposed Approach

## 10. Minimum Viable Experiment

## 11. Baselines

## 12. Ablations

## 13. Evaluation Metrics

## 14. Falsification Conditions

## 15. Expected Contributions

## 16. Known Risks

## 17. Assumptions and Unknowns

## 18. Next 7-Day Plan

## 19. Questions for PI
```

### Survey Harness

Filename: `survey-harness.md`

```markdown
# Survey Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Survey Pitch

## 3. Scope Boundary

## 4. Exclusion Criteria

## 5. Why This Survey Is Needed

## 6. Target Audience

## 7. Proposed Taxonomy

## 8. Comparison Axes

## 9. Related Survey Gap

## 10. Literature Buckets

## 11. Key Questions This Survey Answers

## 12. Expected Contribution

## 13. Risks

## 14. Assumptions and Unknowns

## 15. Next 7-Day Plan

## 16. Questions for PI
```

### Benchmark Harness

Filename: `benchmark-harness.md`

```markdown
# Benchmark Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Benchmark Pitch

## 3. Capability or Failure Mode Being Measured

## 4. Why Existing Benchmarks Are Insufficient

## 5. Benchmark Design

## 6. Dataset / Task Construction

## 7. Evaluation Metrics

## 8. Baselines / Models to Test

## 9. Expected Findings

## 10. What Would Make This Benchmark Uninformative

## 11. Expected Contribution

## 12. Risks

## 13. Assumptions and Unknowns

## 14. Next 7-Day Plan

## 15. Questions for PI
```

### Dataset Harness

Filename: `dataset-harness.md`

```markdown
# Dataset Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Dataset Pitch

## 3. Phenomenon Captured

## 4. Why Existing Datasets Are Insufficient

## 5. Data Sources

## 6. Annotation / Construction Method

## 7. Quality Control Plan

## 8. Supported Tasks or Research Questions

## 9. Comparison to Existing Datasets

## 10. Evaluation / Usefulness Evidence

## 11. Expected Contribution

## 12. Risks

## 13. Assumptions and Unknowns

## 14. Next 7-Day Plan

## 15. Questions for PI
```

### System Harness

Filename: `system-harness.md`

```markdown
# System Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence System Pitch

## 3. Workflow or Technical Bottleneck

## 4. Target Users and Use Cases

## 5. Core Design Choices

## 6. System Architecture Sketch

## 7. Difference from Existing Tools

## 8. Evaluation Plan

## 9. Success Criteria

## 10. Expected Contribution

## 11. Risks

## 12. Assumptions and Unknowns

## 13. Next 7-Day Plan

## 14. Questions for PI
```

### Position Harness

Filename: `position-harness.md`

```markdown
# Position Harness

## 0. Original Title

## 1. Paper Type

## 2. One-sentence Position Pitch

## 3. Mainstream View Being Challenged

## 4. Core Position

## 5. Why This Position Matters

## 6. Evidence Types

## 7. Strongest Counterarguments

## 8. Conceptual Contribution

## 9. Agenda / Implications

## 10. Risks

## 11. Assumptions and Unknowns

## 12. Next 7-Day Plan

## 13. Questions for PI
```

## Style Rules

Use a senior PI / senior PhD voice:

- Direct but supportive.
- Skeptical but not discouraging.
- Force clarity.
- Mark assumptions.
- Prefer falsifiable claims.
- Avoid overclaiming.

Acceptable short lines:

```text
This title is not safe for a group meeting yet.
The claim is too soft.
This is not a bad idea, but right now it is an experiment idea, not a paper.
This needs a hypothesis that can be proven wrong.
```

