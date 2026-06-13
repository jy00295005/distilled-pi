# Title to Harness Design

This document summarizes the `/title-to-harness` workflow. The full source design is in `design/title-to-harness.md`; the operational Skill instructions are in `skill/references/title-to-harness.md`.

## Goal

Turn a rough research title and optional short context into a structured Markdown harness that can be reviewed by a PI, used by a collaborator, or passed to a later AI research agent.

## Flow

```text
Step 0: Title Triage
Step 1: Broad Intake
Step 2: Answer Quality Check
Step 3: Targeted Follow-up if needed
Step 4: Assistive Completion if user cannot answer
Step 5: Generate Harness Markdown
```

## Design Constraints

- Do not write a full proposal first.
- Triage the title type before asking research design questions.
- Ask at most two rounds of questions.
- After answer quality check, emit a clear `Decision` field.
- Ask targeted follow-up only when decision-critical fields are missing.
- Include `Operational Definition` in research harnesses for overloaded concepts such as self-correction, reasoning, robustness, alignment, generalization, agency, understanding, or intelligence.
- If the user cannot answer, provide plausible candidate assumptions and recommend the safest default.
- Always mark assumptions, risks, and unknowns.
- Generate a Markdown artifact suitable for later `/pi-grill` use.

## Supported Harness Types

- `research-harness.md`
- `survey-harness.md`
- `benchmark-harness.md`
- `dataset-harness.md`
- `system-harness.md`
- `position-harness.md`
