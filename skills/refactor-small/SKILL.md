---
name: refactor-small
description: >-
  Identifies a single readability or maintainability problem in the Exis
  codebase and refactors it for better single responsibility, DRY, KISS, YAGNI,
  and a sound balance between concise and descriptive. Use when the user invokes
  /refactor-small or asks for one small, scoped readability/maintainability
  cleanup.
disable-model-invocation: true
---

# One scoped readability refactor

Find **one** real readability/maintainability problem and fix **only that**. This is a surgical cleanup, not a rewrite. Behavior must stay identical.

Follow the principles in `AGENTS.md` ("Readability and maintainability") — they are the source of truth and override anything here on conflict.

## Scope rules

- **Exactly one problem, one concept.** If you spot several, fix the highest-impact one and (optionally) note the rest for later. Do not batch.
- **No behavior change.** Pure refactor: same inputs → same outputs, same side effects, same public API unless the user explicitly asked otherwise.
- **No performance regression.** Refactors must not sacrifice performance — avoid extra allocations, re-renders, or work on hot paths in the name of readability.
- **Small blast radius.** Prefer changes contained to a function, component, or a tight cluster of call sites. If it touches many files or needs design decisions, stop and switch to Plan mode instead.
- **Match existing patterns** before introducing new ones. Consistency with nearby code beats personal preference.
- **No opportunistic churn** — no reformatting, renaming, or reordering unrelated code.

## Workflow

1. **Pick a target.** Use the candidate-finding tactics below, or honor the user's pointer (file, function, "this"). Confirm the target is genuinely worth touching — skip if the code is already clear.
2. **Name the problem in one sentence** and which principle it violates (SRP / DRY / KISS / YAGNI / naming / clarity).
3. **Refactor minimally.** Apply the smallest change that resolves it. Read the file(s) first; preserve types, invariants, and control flow.
4. **Verify** (see commands). Run the checks relevant to what you touched.
5. **Report** using the output template — what, why, and proof it's safe.

## What to look for

| Smell | Principle | Typical fix |
|-------|-----------|-------------|
| Function mixing orchestration + parsing + rendering + validation | SRP | Extract a focused helper; keep the seam obvious |
| Same rule/expression duplicated (same concept) | DRY | Hoist to one named helper/const — only if it's truly the same concept |
| Deep nesting, redundant branches, clever one-liners | KISS | Early returns, guard clauses, plain control flow |
| Speculative params/abstractions/flags with no caller | YAGNI | Delete the unused generality |
| Unnecessary code comments which just purely paraphrase code | KISS | Delete the code comment |
| Vague names (`data`, `tmp`, `handle`, `doIt`) on exported/shared symbols | Naming | Rename to intent-revealing names at the call site |
| Inconsistent naming for the same concept across a module or call chain | Naming | Align on one term that matches nearby conventions |
| Comments narrating obvious code | Clarity | Remove; let code speak (keep only non-obvious intent/tradeoff notes) |
| Over-abstracted one-off hidden behind indirection | KISS/YAGNI | Inline back to the simple direct form |

Avoid the reverse failure too: don't compress code into something terse-but-cryptic. Aim for the balance in `AGENTS.md` — descriptive enough to avoid guesswork, concise enough to read fast.

### Finding candidates (when the user didn't point at one)

- `Grep` for smell signals: `// TODO`, repeated literals, long `match`/`if` chains, `any`, `unwrap()` clusters, duplicated `css({...})` blocks.
- `SemanticSearch` for "the function that does X" when prose-described.
- Prefer authoritative/hot files so the cleanup matters.

## Output template

```markdown
**Refactor:** `path:line` — [problem in one sentence]
**Principle:** [SRP | DRY | KISS | YAGNI | naming | clarity]
**Change:** [what you did, 1–2 lines]
**Safe because:** [why behavior is unchanged + which checks passed]
```

Show the key before/after with a code citation when it aids review.

## Do not

- Touch more than one concern, or fan out across many files.
- Change behavior, public APIs, or test expectations to make a refactor "work".
- Reformat or rename unrelated code.
- Add abstractions for hypothetical future needs.
- Trade performance for readability (extra allocations, re-renders, or hot-path work).
- Proceed when the fix needs design decisions — switch to Plan mode and discuss first.
