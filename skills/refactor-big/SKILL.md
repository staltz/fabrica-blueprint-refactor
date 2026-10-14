---
name: refactor-big
description: >-
  Identifies an unhealthy architectural pattern in the Exis codebase and
  refactors it for better readability, simplicity, KISS, YAGNI, and a balanced
  path for future features without speculative generality. Use when the user
  invokes /refactor-big or asks for a larger architecture-level readability or
  maintainability refactor.
disable-model-invocation: true
---

# Architecture-level readability refactor

Find **one** unhealthy architectural pattern and refactor it into a clearer, simpler shape. This is bigger than `/refactor-small`: it may cross files or module boundaries, but it must still preserve behavior unless the user explicitly asks for a behavior change.

Follow `AGENTS.md` ("Readability and maintainability") as the source of truth. If this skill conflicts with local code patterns or `AGENTS.md`, prefer the repo guidance.

## When to use this

Use this skill for architectural cleanup where the problem is a recurring pattern, boundary issue, or abstraction problem, such as:

- One module owning too many responsibilities.
- Data shape or control flow that forces callers to know too much.
- Repeated orchestration logic spread across layers.
- An abstraction that exists for hypothetical future needs but complicates current work.
- Divergent names or concepts for the same domain rule across several files.
- Tight coupling between UI, transport, persistence, and calculation logic.

Do **not** use this for a small local cleanup. Use `/refactor-small` when a single function, component, or tight cluster can be fixed surgically.

## Scope rules

- **One architectural pattern only.** Fix the highest-impact pattern; do not batch unrelated cleanups.
- **Behavior stays the same.** Preserve public APIs, persisted data, user-visible behavior, and tests unless the user approves a change.
- **Refactor toward current needs.** Support clear next steps only when the current design already points there; do not build speculative extension points.
- **Prefer deletion over indirection.** Remove unused layers, flags, adapters, and options before introducing new abstractions.
- **Keep ownership obvious.** Move logic to the module that owns the invariant, not to generic "utils" unless a shared helper already fits.
- **Respect hot paths.** Do not trade readability for extra render work, allocations, server round trips, or recalculation cost.
- **Limit blast radius.** Multi-file is allowed, but every touched file should be directly part of the same pattern.

## Workflow

1. **Explore first.** Read the relevant files and use semantic search or focused greps to understand how the pattern repeats across call sites.
2. **State the diagnosis.** Name the unhealthy pattern in one sentence and identify the violated principle: SRP, DRY, KISS, YAGNI, naming, coupling, or ownership.
3. **Switch to Plan mode for design choices.** If there are multiple plausible architectures, user-facing risks, or broad API changes, present a short plan before editing.
4. **Choose the smallest coherent shape.** Prefer a direct module boundary, plain data flow, or named domain operation over generic frameworks or configuration.
5. **Refactor in slices.** Keep each edit reviewable: move ownership, update call sites, delete obsolete code, then tighten names.
6. **Verify behavior.** Run checks relevant to touched areas and add or update focused tests when the refactor changes shared contracts or exposes risk.
7. **Report clearly.** Explain the pattern removed, the new ownership boundary, and why future work is easier without adding speculative machinery.

## What to look for

- **Responsibility leaks:** UI code parsing engine concepts, transport code owning domain rules, or persistence code shaping calculation behavior.
- **Orchestration duplication:** Several call sites performing the same sequence of validation, conversion, dispatch, and reconciliation.
- **Speculative layers:** Interfaces, flags, config objects, factories, or adapters with one real implementation and no current need.
- **Unused or deprecated subsystems:** Modules, routes, protocol branches, or feature flags with no live callers; legacy adapters kept after a migration; code marked deprecated or "temporary" that has outlived its replacement. Confirm with grep, imports, and tests before deleting.
- **Overgrown subsystems:** A bounded area (collab transport, agent loop, activity log, persistence layer) that has accumulated modes, fallbacks, or parallel code paths when one straightforward shape would cover current needs. Look for optional branches that always take the same path, or configuration that never varies.
- **Concept drift:** Multiple names or data shapes for the same domain concept across frontend, server, and engine.
- **Hidden invariants:** Callers required to remember ordering, normalization, cleanup, or reconciliation rules manually.
- **Over-broad modules:** Files that mix rendering, state transitions, I/O, parsing, and domain decisions.
- **Utility dumping grounds:** Shared helpers that hide ownership instead of clarifying it.

Avoid the opposite mistake: do not flatten meaningful boundaries just to reduce file count. A good architecture-level refactor makes the important ownership clearer, not merely shorter.

## Refactoring moves

- Move a rule to the module that owns the invariant, then call it from the edges.
- Replace repeated call-site choreography with one named operation.
- Collapse a one-off abstraction back into direct code.
- Split a mixed module along existing domain boundaries.
- Rename concepts consistently across a call chain.
- Replace loosely typed plumbing with a small domain type when it removes real ambiguity.
- Delete unused options, deprecated subsystems, and future-facing branches after confirming there are no callers.
- Simplify a subsystem by collapsing redundant modes or merging parallel implementations into one clear path.

## Output template

```markdown
**Refactor:** [architectural pattern removed, with key paths]
**Principle:** [SRP | DRY | KISS | YAGNI | naming | coupling | ownership]
**Diagnosis:** [one sentence naming the unhealthy pattern]
**Change:** [what moved, collapsed, renamed, or deleted]
**Safe because:** [behavior-preservation reasoning + checks passed]
**Residual risk:** [remaining risk or "None known"]
```

Include a short before/after explanation or code citation when it helps review the architectural shift.

## Do not

- Turn a refactor into a feature project.
- Preserve compatibility with unused in-progress code by layering shims around it.
- Add frameworks, registries, factories, or plugin points for imagined future features.
- Change public behavior, persistence shape, or protocol semantics without explicit approval.
- Mix unrelated renames, formatting, or cleanup into the same refactor.
- Proceed directly to edits when the architecture tradeoff is unclear; switch to Plan mode first.
