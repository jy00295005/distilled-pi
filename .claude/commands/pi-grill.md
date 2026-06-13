---
description: Distilled PI workflow that stress-tests a generated research harness.
---

Use the Distilled PI project skill.

First read `.claude/skills/distilled-pi/SKILL.md`, then follow the canonical `/pi-grill` workflow from `skill/references/pi-grill.md`.

Use the user's message after this command as the harness and optional supporting material:

$ARGUMENTS

Follow the canonical rules:

- require a harness as the source of truth;
- identify the PI Focus before Round 1;
- ask only the 2-3 questions that matter most;
- avoid checklist dumping;
- critique first, then provide a repair path;
- generate `pi-grill-report.md` with Research Risk Heatmap and PI Verdict.

If the user explicitly asks to save the final report, write it to the requested filename.
