---
description: "Refactor Rust or TypeScript using the existing repository skills"
trigger: {"on": "github.push"}
connectors: ["github"]
side_effects: ["github.push", "github.open_pull_request", "github.pr_comment"]
memory: true
models:
  - { model: grok-4.6, effort: high, fast: false }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
# Conditions to proceed

Generate a random integer with `echo $(( RANDOM % 100 ))`, if it is less than or equal 10, then proceed, otherwise abort yourself immediately.

Look at how many open or draft pull requests this repository currently has. If 2 or more of them have a title starting with `refactor: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `refactor: `.

# Instruction

Generate a random integer with `echo $(( RANDOM % 100 ))`, if it is less than or equal 50, then you will refactor Rust files, otherwise you will refactor TypeScript files.

Use the /refactor-big OR the /refactor-small skill, opening a new pull request (with a title that starts with "refactor: "). After opening the pull request, wait 10 minutes, then check whether CI is failing or passing in that pull request branch. If there are failing CI tasks, debug and fix, to restore them to green.

Your pull request description should have `## Diagnosis`, `## Refactoring principle`, `## What was refactored`, `## Why this is beneficial`. Speak in casual and conversational plain English expository prose, easily readable by junior developers.

# Criteria

## Good refactors

A good refactor usually has a git diff statistics that has approximately as many deletes as additions, OR MORE deletes than additions which is even better.

A good refactor improves how code is organized according to ontological aspects.

A good refactor improves the internal consistency of the codebase in terms of patterns used. It reduces the use of methods and solutions that would surprise a reader familiar with the remaining codebase.

See "examples" in MEMORIES.md too.

## Bad refactors

* Splitting a file into many; splitting a function into many
* Removing encapsulations and SRP (even if it has a net negative git diff)) is a bad refactor
* Substantially more git additions than deletions is usually a bad refactor. Run a critique subagent who thinks in long-term strategic  if you want to make an exception. If the critique approves, then you allow an exception.
* Centralizing one aspect of many entities into one place. Prefer to centralize all aspects of a single entity instead.
* Anything resembling code golf

See "counterexamples" in MEMORIES.md too.
