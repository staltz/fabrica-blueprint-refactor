---
description: "Align code names with one term from CONTEXT.md"
trigger: { on: pace, every: 12 }
connectors: ["github"]
side_effects: ["github.push", "github.open_pull_request"]
memory: true
skills: [examples-counterexamples]
models:
  - { model: grok-4.7, effort: high, fast: false }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
# Conditions to proceed

Look at how many open or draft pull requests this repository currently has. If 2 or more of them have a title starting with `refactor/context: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `refactor/context: `.

# Instructions

If CONTEXT.md does not exist, abort yourself immediately. Otherwise, continue.

Read CONTEXT.md and focus on one term only. Then, scan the codebase to find misnamings of that term, and rename them correctly. Please see MEMORIES.md for the counterexamples, those are false misnamings that you should not attempt to rename. If you did not find any misnaming, abort yourself immediately.

Then open a pull request with a title that starts with `refactor/context: `.

