# Changelog

## v0.1.1

Fixes from a real heterogeneous review (Codex, run in a working environment) — the meta-skill was breaking rules it preaches:

- **Spec-compliant frontmatter (it teaches "follow the standard" — must comply itself).** Move `version`/`author` under `metadata` (`version` as a quoted string), convert `metadata.tags` from a YAML array to a comma-separated string. The [agentskills spec](https://agentskills.io/specification) defines `metadata` as a string→string map with no top-level `version`/`author`.
- **`--list` is not an install check.** Clarified that `npx skills add --list` only confirms the frontmatter parses and the skill is listed — not that the install works. Added a real end-to-end smoke test (`npx skills add ... -g -a <agent> -y`) to the publish step, the checklist, and Mistake #6.
- **Description de-overlapped with skill-creator** — narrowed to package/publish/ship/distribute; authoring is delegated to a skill-creator skill.
- **Mistake #8 softened** — tag/release is a nice-to-have trust signal, not a hard rule.
- **mcpmarket note updated** — it has a submit page; submission varies by site, so "check each site" rather than "unverified."

## v0.1.0

- Initial preview of the `shipping-skills` meta-skill: the end-to-end pipeline for creating and publishing an Agent Skill to GitHub (design → SKILL.md → triggers/evals → repo structure → naming → cross-file consistency → `gh` publish → distribution → pre-publish review).
- "Common Mistakes" section captures real, field-tested gotchas (mechanism-jargon descriptions, name collisions, cross-file enum drift, announcing before verifying the install path, treating directories as manual submit forms).
- 20-prompt trigger eval set (`evals/eval-set.md`).
- Pre-publish reviewed via agent-arena. The review (degraded to solo_red_team because the heterogeneous reviewer was unavailable) caught the skill violating two of its own rules and they were fixed before release:
  - The mcpmarket distribution note was an unverified claim → relabeled as "not verified here" (the skill itself preaches "verify before announcing").
  - The name `shipping-skills` was checked for collisions (none found) and related projects are now disclosed in the README.
