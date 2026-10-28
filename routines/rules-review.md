---
description: "Review pull requests for violations of repository rules"
triggers:
  - {"on": "github.pull_request.opened"}
  - {"on": "github.pull_request.synchronize"}
connectors: ["github"]
side_effects: ["github.pr_comment"]
memory: false
models:
  - { model: deepseek-v4.1-flash-1017, effort: high }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
A pull request was opened, or new commits were pushed to it.

Review whether the pull request in question is violating any of the rules in AGENTS.md or .cursor/rules ; if there are any violations, then flag those as code review comments, preferably inline.

When you comment on the pull request, always have the first sentence be either "**Rules: passed.** ☘️" (🍀 emoji is equally accepted as ☘️) or "**Rules: violated.** 🥀"
