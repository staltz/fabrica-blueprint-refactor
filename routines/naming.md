---
description: "Improve a poorly named source-code concept"
trigger: {"on": "github.push"}
connectors: ["github"]
side_effects: ["github.push", "github.open_pull_request"]
memory: true
skills: [examples-counterexamples]
params:
  proceed_threshold: { default: 10, description: "Proceed only when a random 0-99 roll is at most this" }
models:
  - { model: grok-4.6, effort: high, false: false }
  - { model: claude-sonnet-5-5, effort: xhigh }
  - default
---
# Conditions to proceed

Generate a random integer with `echo $(( RANDOM % 100 ))`, if it is less than or equal {{params.proceed_threshold}}, then proceed, otherwise abort yourself immediately.

Look at how many open or draft pull requests this repository currently has. If 2 or more of them have a title starting with `naming: `, abort yourself immediately.

# Side quest

Before moving on to the "Instructions", use the /examples-counterexamples skill with prefix `naming: `.

# Goal

Rename something in the codebase. Choose names that make code easy to read, discuss, and search globally. Good naming is aesthetic, reduces confusion, improves communication, and makes future changes safer.

# Instructions

Pick a badly named variable, concept, folder, file, namespace, project, entity, or something in the source code (never in user interface), and rename it. Follow the principles below, rename something, then open a pull request with a title that starts with "naming: ".

# Principles

Names are documentation. Good names help developers understand what code does and why, without reading every implementation detail. They should be clear, meaningful, and unsurprising, especially in codebases maintained by people with different experience levels and backgrounds. On the other hand, too long and descriptive names pollute the structure of the code, decreasing readability overall. Good names are often one or two or three words long. Naming is thus an art of balancing between self-explanatory names and short names.

Prefer names from the technical domain when they are well known. Otherwise, use established business terms instead of inventing new ones. Make names searchable, pronounceable, and consistent. Use one word for one concept, and different words for different concepts. Follow common conventions such as nouns for classes and verbs or verb phrases for methods.

Inconsistent naming is worse than a bad name. Look for equivalent concepts or entities named differently, and find a common name.

Avoid misleading names, unnecessary words like `data` or `info`, outdated type encodings, slang, and obscure acronyms. Single letter names are acceptable only in limited contexts, such as loop counters, or for geometry.

Read MEMORIES.md for examples and counterexamples.
