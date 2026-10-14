---
name: examples-counterexamples
description: >-
  Maintains the list of example pull requests and counterexample pull requests
  in MEMORIES.md. Use when a routine says to refresh memories before its main
  instructions. The caller passes the "title prefix" argument.
disable-model-invocation: true
---

# Examples and counterexamples pull requests

**Parameter:** `prefix` which is the pull request title prefix to match, including its trailing space when the caller writes one (for example `context: `).

Run this before the routine's main instructions. Finishing early here only skips the memory update. Continue with the rest of the routine either way.

## Steps

1. Read `MEMORIES.md` and find the recorded "last date". If none is recorded, treat the last date as the beginning of time.
2. Get today with `date +%Y-%m-%d`. Continue only when that last date is at least 7 days before today. Otherwise stop this skill.
3. Look up every pull request created since the last date whose title starts with `prefix`.
4. For each closed pull request, add a counterexample to `MEMORIES.md`.
5. For each merged pull request, add an example to `MEMORIES.md`.
6. Set the "last date" in `MEMORIES.md` to today.

A merged pull request counts as an example. A pull request closed without merge counts as a counterexample.
