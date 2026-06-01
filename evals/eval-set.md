# shipping-skills trigger eval set

20 prompts to test activation. Target: both groups ≥ 90%. Re-run after any `description` change.
★ = boundary case (looks adjacent but should NOT trigger) — highest value.

## should-trigger (10)

| # | prompt | dimension |
|---|--------|-----------|
| 1 | 帮我把这个工作流做成一个 Agent Skill 并发布到 GitHub | create + publish |
| 2 | Write a SKILL.md for X and tune its description so it triggers reliably | author + trigger tuning |
| 3 | 我的 skill 发布前帮我 review 命名、frontmatter、一致性 | pre-publish review |
| 4 | How do I get my skill listed on skills.sh? | distribution |
| 5 | 帮我给这个 skill 仓库起个不撞名的好名字 | naming |
| 6 | Package these instructions into a proper skill repo with a release | repo structure + release |
| 7 | 我的 description 不触发,怎么改 | description tuning |
| 8 | Publish my Claude Code skill to GitHub with tags | publish |
| 9 | 这个 skill 的 SKILL.md 和 schema 文件枚举对不上,怎么统一 | cross-file consistency |
| 10 | Set up evals so I know my skill triggers correctly | evals |

## should-NOT-trigger (10)

| # | prompt | why not |
|---|--------|---------|
| ★1 | 解释一下什么是 Agent Skill | explanation, not creating/publishing |
| ★2 | 用 agent-arena 评审这个架构决策 | running a skill, not shipping one |
| ★3 | 帮我 fact-check 这段内容 | using a skill (groundcheck), not shipping |
| 4 | 帮我写个 React 组件 | application code |
| 5 | git rebase 怎么用 | general Git, unrelated to shipping a skill |
| 6 | 给我推荐几个好用的 Claude skill | discovery as a user, not authoring |
| 7 | 把这个 PDF 总结一下 | unrelated task |
| 8 | 我的 npm 包怎么发布到 registry | publishing, but not a skill |
| 9 | 写一篇博客介绍我的项目 | marketing copy, not skill shipping |
| 10 | 修复这个 Python 报错 | debugging app code |

## Notes

- ★1/★2/★3 guard the key boundaries: shipping-skills is about *making and publishing* a skill, not *explaining* skills (#1) or *running* other skills like agent-arena/groundcheck (#2/#3).
- #8 guards the "publish, but not a skill" boundary (npm package ≠ Agent Skill).
- Each should-trigger prompt should also be walked end-to-end once to confirm the pipeline produces a publishable repo.
