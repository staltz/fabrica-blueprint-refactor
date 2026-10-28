---
description: "Remove verified outdated documentation, dead code, or unnecessary artifacts"
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

Look at how many open or draft pull requests this repository currently has. If 1 or more of them have a title starting with `cruft: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `cruft: `.

# Instruction

Find outdated documentation or dead code or other unnecessary artifacts (except those in `docs/`, and any file that was changed in the past 5 days). Then double check that they're actually outdated cruft. Finally, simply delete it. Open a pull request with a title that starts with "cruft: ". Create it ready for review, not as a draft. Do not use --draft. If it opens as a draft, immediately mark it ready for review.

After opening the pull request, wait 10 minutes, then check whether CI is failing or passing in that pull request branch. If there are failing CI tasks, debug and fix, to restore them to green.
