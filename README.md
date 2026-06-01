# Shipping Skills

> The end-to-end playbook for taking an Agent Skill from idea to a **published, discoverable GitHub repo** — battle-tested, every step earned from a real mistake.

Most "how to make a skill" guides stop at writing a `SKILL.md`. **shipping-skills** covers the part that actually gets your skill used: naming without collisions, tuning the `description` so it triggers, keeping multi-file specs consistent, publishing with `gh` + releases, verifying the install path works, and getting listed where people look.

## The pipeline

```
design → SKILL.md → triggers + evals → repo structure →
name + discoverability → cross-file consistency →
publish to GitHub → distribution → pre-publish review
```

Each step in [`skills/shipping-skills/SKILL.md`](skills/shipping-skills/SKILL.md) exists because skipping it caused a real, avoidable problem — and the "Common Mistakes" section is a list of things that actually bit the author (mechanism-jargon descriptions, name collisions, cross-file enum drift, announcing before verifying the install).

## Install

```bash
npx skills add zhjai/shipping-skills -g -a claude-code
```

Works with Claude Code, Codex, Cursor, OpenCode, and other Agent Skills hosts. Or copy `skills/shipping-skills/` into your agent's skills directory.

## When to use

Creating a new skill · writing/fixing a `SKILL.md` · tuning a trigger `description` · structuring a skill repo · naming it · publishing to GitHub with releases · getting it discovered.

**Not for:** application code, non-skill docs, or general Git questions unrelated to shipping a skill.

## Related (and how this differs)

- **Anthropic `skill-creator`** and the **agentskills.io quickstart** — focus on *authoring* a SKILL.md.
- **addyosmani/agent-skills** includes a `shipping-and-launch` skill (different scope).

`shipping-skills` focuses on the **full create → publish → distribute pipeline**, with field-tested gotchas and the exact `gh` / `npx skills` commands, plus how discovery actually works (skills.sh telemetry, agentskills.io as a spec — not a directory).

## Status

`v0.1.0` preview. MIT. Not affiliated with any vendor. Drafted and pre-publish-reviewed using [agent-arena](https://github.com/zhjai/agent-arena) (the review caught the skill violating its own rules — see CHANGELOG).
