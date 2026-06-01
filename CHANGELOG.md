# Changelog

## v0.1.0

- Initial preview of the `shipping-skills` meta-skill: the end-to-end pipeline for creating and publishing an Agent Skill to GitHub (design → SKILL.md → triggers/evals → repo structure → naming → cross-file consistency → `gh` publish → distribution → pre-publish review).
- "Common Mistakes" section captures real, field-tested gotchas (mechanism-jargon descriptions, name collisions, cross-file enum drift, announcing before verifying the install path, treating directories as manual submit forms).
- 20-prompt trigger eval set (`evals/eval-set.md`).
- Pre-publish reviewed via agent-arena. The review (degraded to solo_red_team because the heterogeneous reviewer was unavailable) caught the skill violating two of its own rules and they were fixed before release:
  - The mcpmarket distribution note was an unverified claim → relabeled as "not verified here" (the skill itself preaches "verify before announcing").
  - The name `shipping-skills` was checked for collisions (none found) and related projects are now disclosed in the README.
