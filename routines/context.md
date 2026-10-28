---
description: "Align code names with one term from CONTEXT.md"
trigger: {"on": "github.push"}
connectors: ["github"]
side_effects: ["github.push", "github.open_pull_request"]
memory: true
skills: [examples-counterexamples]
params:
  proceed_threshold: { default: 10, description: "Proceed only when a random 0-99 roll is at most this" }
models:
  - { model: deepseek-v4.1-flash-1017, effort: high }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
# Conditions to proceed

Generate a random integer with `echo $(( RANDOM % 100 ))`, if it is less than or equal {{params.proceed_threshold}}, then proceed, otherwise abort yourself immediately.

Look at how many open or draft pull requests this repository currently has. If 2 or more of them have a title starting with `context: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `context: `.

# Instructions

If CONTEXT.md does not exist, abort yourself immediately. Otherwise, continue.

Read CONTEXT.md and focus on one term only. Then, scan the codebase to find misnamings of that term, and rename them correctly. Please see MEMORIES.md for the counterexamples, those are false misnamings that you should not attempt to rename. Then open a pull request with a title that starts with "context: ". Create it ready for review, not as a draft. Do not use --draft. If it opens as a draft, immediately mark it ready for review.
