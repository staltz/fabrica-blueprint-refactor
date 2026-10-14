---
description: "Extract a large Rust or TypeScript file or function into logical parts"
trigger: {"on": "github.push"}
connectors: ["github", "github-mcp"]
side_effects: ["github.push", "github.open_pull_request"]
memory: true
models:
  - { model: grok-4.6, effort: high }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
# Conditions to proceed

Generate a random integer with `echo $(( RANDOM % 100 ))`, if it is less than or equal 11, then proceed, otherwise abort yourself immediately.

Look at how many open or draft pull requests this repository currently has. If 2 or more of them have a title starting with `split: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `split: `.

# Instruction

Your goal is to refactor something in the codebase by simply splitting a large thing into smaller logical things.

1. Look for a source code file with more than 1000 lines of code, or a function with more than 300 lines of code. If you don't find one, abort yourself immediately.
2. "Extract" the parts of the file or function you identified in the step above
3. Open a pull request (with a title that starts with "split: "). Create it ready for review, not as a draft. Do not use --draft. If it opens as a draft, immediately mark it ready for review. In the pull request description, you must use a markdown table syntax with columns before/after to indicate the number of lines of code in the file that is being split.

Extraction is the act of treating a stretch of behavior as a thing that already existed inside a larger act, then giving that thing its own identity. A good split is not a cut by size. It is the recognition of a seam: a part that has one purpose, one reason to change, and a name that states what that purpose is. The parent says what happens; the child is one of those happenings.
