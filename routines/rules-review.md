---
description: "Review pull requests for violations of repository rules"
trigger: {"on": "github.pull_request.opened"}
connectors: ["github"]
side_effects: ["github.pr_comment"]
memory: false
models:
  - { model: grok-4.7, effort: high, fast: false }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
Review whether the pull request in question is violating any of the rules in AGENTS.md or .cursor/rules ; if there are any violations, then flag those as code review comments, preferably inline.

When you comment on the pull request, always have the first sentence be either "**Rules: passed.** ☘️" (🍀 emoji is equally accepted as ☘️) or "**Rules: violated.** 🥀"
