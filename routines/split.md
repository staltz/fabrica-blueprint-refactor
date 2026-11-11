---
description: "Extract a large Rust or TypeScript file or function into logical parts"
trigger: { on: pace, every: 8 }
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

Look at how many open or draft pull requests this repository currently has. If 2 or more of them have a title starting with `refactor/split: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `refactor/split: `.

# Instruction

Your goal is to refactor something in the codebase by simply splitting a large thing into smaller logical things.

1. Look for a source code file with more than 1000 lines of code, or a function with more than 300 lines of code, or a folder with more than 10 files. If you don't find one, abort yourself immediately.
2. "Extract" the parts of the file or function you identified in the step above, or the files in the folder you identified in the step above.
3. Open a pull request (with a title that starts with `refactor/split: `). In the pull request description, you must use a markdown table syntax with columns before/after to indicate the number of lines of code in the file that is being split.

Extraction is the act of treating a stretch of behavior as a thing that already existed inside a larger act, then giving that thing its own identity. A good split is not a cut by size. It is the recognition of a seam: a part that has one purpose, one reason to change, and a name that states what that purpose is. The parent says what happens; the child is one of those happenings.

