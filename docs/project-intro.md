# Project Introduction

Distilled PI, 中文名“导师拷打器”, is a research idea pressure-testing Skill. It is designed for the moment before a real PI meeting, group meeting, proposal discussion, or formal writing session, when a research idea needs to become sharper, narrower, and more defensible.

The product stance is:

- concise,
- skeptical but helpful,
- senior-PI-like,
- assumption-aware,
- falsification-oriented,
- useful for both human meetings and downstream AI research agents.

## User Problem

Early research ideas often sound promising but fail under basic PI questions:

- What exactly is the claim?
- What would falsify it?
- Why is this new?
- Does the experiment prove the claim or only a proxy?
- What happens if the strongest baseline wins?

Distilled PI exists to surface those issues before the real meeting.

## Product Shape

The Skill has two workflows:

1. `/title-to-harness`: convert a rough title into a structured research harness.
2. `/pi-grill`: use that harness as source of truth for a concise PI-style stress test.

The intended artifact chain is:

```text
rough title
-> harness markdown
-> PI grill report
-> revised harness / proposal / experiment plan
```

## Design Sources

The complete workflow designs live in:

- `design/title-to-harness.md`
- `design/pi-grill.md`

