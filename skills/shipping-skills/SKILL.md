---
name: shipping-skills
description: Use when the user wants to create, build, author, package, publish, or ship an Agent Skill — writing a SKILL.md, structuring a skill repo, naming it, making it discoverable, publishing to GitHub with tags/releases, and getting it listed (skills.sh, directories). Covers the full pipeline from design to distribution, including trigger-description tuning, cross-file consistency, and pre-publish review. Do not use for writing application code, non-skill docs, or general Git questions unrelated to shipping a skill.
version: 0.1.0
author: zhjai
license: MIT
metadata:
  tags: [agent-skill, claude-code-skill, skill-creation, skill-publishing, skill-authoring, github, distribution, discoverability]
---

# Shipping Skills

## Overview

This skill is the end-to-end playbook for taking an Agent Skill from idea to a published, discoverable GitHub repo. It is opinionated and battle-tested: every step here exists because skipping it caused a real, avoidable problem.

The pipeline: **design → write SKILL.md → triggers + evals → repo structure → name + discoverability → cross-file consistency → publish to GitHub → distribution → pre-publish review.**

## When to Use / Not Use

**Use** when: creating a new skill, writing or fixing a `SKILL.md`, structuring a skill repo, naming it, publishing to GitHub with releases, or getting it discovered.

**Do not use** for: writing application code, non-skill documentation, or general Git questions unrelated to shipping a skill.

## 1. Design first

Before writing anything, answer three questions:

- **What cognitive/work failure does it fix, in one sentence?** A skill that does "many things" triggers reliably for none.
- **When should it fire?** This becomes the `description`. Write down 5 example user phrases that should trigger it and 5 that should NOT.
- **Single skill or a repo of skills? Standalone repo or fold into an existing one?** Decide by *mechanism + coupling + maintenance bandwidth*, not vibes. A tool whose main use is independent of an existing repo belongs in its own repo. Don't put a tool inside a repo whose **name implies a different mechanism** (it misleads users about how it works).

## 2. Write the SKILL.md

Required frontmatter: `name` and `description`. Add `version`, `license`, `metadata.tags`.

**The `description` is the single most important field** — agents load only `name` + `description` at startup and use it to decide whether to activate the skill (progressive disclosure). So:

- Lead with **user-intent trigger phrases** ("second opinion", "fact-check this", "review my plan"), NOT internal-mechanism jargon ("heterogeneous multi-agent orchestration"). Users don't speak in your architecture's terms.
- Include a **negative boundary**: "Do not use for …". This stops the skill from grabbing tasks that belong to other skills.
- Keep the body focused; push large reference material into `references/` and load on demand (progressive disclosure). A bloated SKILL.md wastes every session's context.

Body essentials: Overview · When to use/not · the core protocol/steps · examples · a verification checklist.

## 3. Triggers + evals

Ship an `evals/` set from day one: ~10 should-trigger and ~10 should-not-trigger prompts. The **boundary cases** (prompts that look like they fit but belong to a neighboring skill) are the highest-value ones — they stop misfires. Re-run after every `description` edit; it's a regression test.

## 4. Repo structure

Use the portable layout so the `skills` CLI discovers it:

```
my-skill/
├── skills/<skill-name>/SKILL.md   # the skill (CLI scans skills/<name>/SKILL.md)
├── README.md                      # result-first; what it does, install, when to use
├── LICENSE                        # e.g. MIT
├── CHANGELOG.md
├── evals/                         # trigger eval set
├── assets/                        # banner, etc.
└── references/                    # optional deep docs (progressive disclosure)
```

## 5. Name + discoverability (a common trap)

- **Check for name collisions before committing to a name.** Generic names ("agent-arena", "fact-check") collide with big projects and dilute search. Pick something descriptive + distinctive that does **not imply the wrong mechanism**.
- Make the GitHub **repo description** carry category keywords + a user-intent phrase.
- Add **topics**: `claude-code-skill`, `agent-skill`, plus your domain — directories often auto-index by topic.

## 6. Cross-file consistency (a trap I hit)

If the skill spans multiple spec files (SKILL.md + interop/schema docs), **declare ONE file the normative source** for every enum/contract and have the others defer to it. Otherwise the files drift (e.g. `code-api-behavior` in one, `code-api` in another) and break integrators. Verify with a quick grep that enums/terms match across files before publishing.

## 7. Publish to GitHub

```bash
# local
git init -b main && git add -A && git commit -m "feat: initial <skill> scaffold (v0.1.0)"

# create public repo and push (gh CLI)
gh repo create <owner>/<name> --public --source=. --remote=origin --push \
  --description "<one line with category keywords + user intent>"

# topics for discoverability
gh repo edit <owner>/<name> --add-topic claude-code-skill --add-topic agent-skill

# tag + release = a trust signal for a new repo
git tag -a v0.1.0 -m "v0.1.0 — initial preview" && git push origin v0.1.0
gh release create v0.1.0 --title "v0.1.0 — <name>" --notes "<highlights>"
```

**Then verify the install path actually works** (evidence over assumption):

```bash
npx skills add <owner>/<name> --list   # must print "Found N skills" and your skill name
```

If `--list` can't parse it, your frontmatter is broken — fix before announcing.

## 8. Distribution (how skills actually get found)

- **skills.sh** — no manual submit. The directory/leaderboard is built from the `skills` CLI's anonymous install telemetry. You appear by getting people to run `npx skills add <owner>/<name>`. So: **promote the install command** (README, a launch post, socials).
- **agentskills.io** — this is the **open-standard spec site, not a submission directory.** You don't "publish" to it; you just conform to the standard (valid SKILL.md). Engage via its GitHub/Discord if you want.
- **Marketplaces (e.g. mcpmarket)** — submission process varies and is **not verified here**; some directories auto-index public repos by topic, others require a manual submit. Check each site's own process before assuming (don't claim a path you haven't confirmed — see Mistake #6).
- **Highest-leverage single action:** a one-line `npx skills add` command + a short launch post telling the result-first story. That feeds the leaderboard and lets auto-indexers find you.

## 9. Review before publishing

Run a **heterogeneous review** (e.g. a second model / agent) over the skill before announcing. It reliably catches what you can't see in your own work: cross-file enum drift, misleading wording, over-claims, and `README` commands that don't actually run. Fix, bump a patch version, then publish.

## Common Mistakes (each one bit me)

1. **Mechanism-jargon description** — users never trigger it because they don't speak your architecture's language. Use intent phrases.
2. **No negative boundary** — the skill grabs tasks that belong elsewhere.
3. **Unchecked name collision** — buried under same-name giants in search.
4. **Name implies the wrong mechanism** — e.g. a single-agent tool in an "arena" repo misleads users about how it works.
5. **Cross-file enum drift** — multiple spec files with no normative source; integrators break.
6. **Announcing without verifying `--list`** — a broken frontmatter ships silently.
7. **Treating directories as manual submit forms** — skills.sh is automatic via install telemetry; promote the install command instead.
8. **No release/tag** — a 0-release repo reads as abandoned; tagged releases are a trust signal.

## Verification Checklist

- [ ] `description` leads with user-intent phrases + has a "Do not use" boundary.
- [ ] `evals/` has should-trigger AND should-not-trigger prompts, incl. boundary cases.
- [ ] One normative source declared for any cross-file enums/contracts; grep-confirmed consistent.
- [ ] Repo has README (result-first), LICENSE, CHANGELOG, topics, description.
- [ ] Name checked for collisions; doesn't imply the wrong mechanism.
- [ ] `npx skills add <owner>/<name> --list` prints the skill (frontmatter parses).
- [ ] Tag + GitHub release published.
- [ ] Heterogeneous review done; README commands actually run.

## Example Prompts

- "Help me turn this workflow into an Agent Skill and publish it to GitHub."
- "Write a SKILL.md for X and tune its description so it triggers reliably."
- "Review my skill repo before I publish — naming, frontmatter, consistency."
- "How do I get my skill listed on skills.sh?"
