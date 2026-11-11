---
description: "Remove verified outdated documentation, dead code, or unnecessary artifacts"
trigger: { on: pace, every: 10 }
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

Look at how many open or draft pull requests this repository currently has. If 1 or more of them have a title starting with `refactor/cruft: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `refactor/cruft: `.

# Instruction

Find outdated documentation or dead code or other unnecessary artifacts (except those in `docs/`, and any file that was changed in the past 5 days). Then double check that they're actually outdated cruft. If you have not found any dead code, then abort yourself immediately.

Finally, simply delete the cruft. Open a pull request with a title that starts with `refactor/cruft: `.

