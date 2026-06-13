---
description: Distilled PI workflow that turns a research title into a structured harness.
---

Use the Distilled PI project skill.

First read `.claude/skills/distilled-pi/SKILL.md`, then follow the canonical `/title-to-harness` workflow from `skill/references/title-to-harness.md`.

Use the user's message after this command as the title/background input:

$ARGUMENTS

Follow the canonical rules:

- classify the paper type;
- perform broad intake;
- run the answer quality check;
- make a clear Decision: proceed, ask targeted follow-up, or use assistive defaults and proceed;
- only ask targeted follow-up when decision-critical fields are missing;
- generate the appropriate Markdown harness.

If the user explicitly asks to save the final harness, write it to the requested filename.
