---
name: distilled-pi
description: PI-style research idea pressure-testing skill for Claude Code. Use when the user invokes /title-to-harness to turn a rough research title into a structured harness, invokes /pi-grill to stress-test a generated harness, or asks for blunt senior-PI-style critique before a group meeting, proposal, paper draft, or research planning session.
---

# Distilled PI Claude Code Adapter

This is the Claude Code project adapter for Distilled PI.

The canonical workflow instructions live in the repository-level Skill package:

- `skill/SKILL.md`
- `skill/references/title-to-harness.md`
- `skill/references/pi-grill.md`

Use this adapter only for routing. Do not maintain a separate Claude-specific copy of the workflow logic.

## Routing

- If the user invokes `/title-to-harness`, read `skill/SKILL.md`, then read `skill/references/title-to-harness.md`, and follow that workflow.
- If the user invokes `/pi-grill`, read `skill/SKILL.md`, then read `skill/references/pi-grill.md`, and follow that workflow.
- If the user asks for Distilled PI help without naming a workflow, choose `/title-to-harness` when they provide only a title or rough idea; choose `/pi-grill` only when they provide a generated harness.
- If the user asks to run `/pi-grill` without a harness, stop and ask for the harness instead of brainstorming from scratch.

## Claude Code Notes

- Treat the `skill/` directory as the source of truth.
- Preserve the same PI persona, output formats, and artifact names as the canonical Skill.
- If the user asks to save an artifact, write the requested Markdown file in the workspace.
- If the user does not ask to save a file, provide the artifact in the chat response.
