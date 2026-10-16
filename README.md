# Refactor blueprint

This is a [Fabrica](https://github.com/staltz/fabrica) blueprint. It is a small pack of instructions that AI coding agents follow to tidy a codebase: better names, smaller files, less dead code, and the occasional real refactor.

The agents work on your product repo, the one with the actual software. They open ordinary GitHub pull requests, and a person reviews those the way they review anyone else's.

## What happens after a push

Five routines listen for `github.push`. They are stingy on purpose. Each one rolls a number from 0 to 99 and continues only on roughly a one-in-ten chance. They also stop if a couple of their own PRs are already sitting open. You get a slow drip of cleanup, and the review queue stays small.

Every PR they open uses a title prefix, so you can skim the list and know who did what.

**`refactor`** (`refactor: `) flips a coin between Rust and TypeScript, then runs either a local readability fix or a bigger architectural one. It prefers diffs that delete as much as they add, and it waits about ten minutes to see if CI is red, then patches it.

**`split`** (`split: `) looks for a source file over 1000 lines, or a function over 300, and extracts one logical part along a real seam. A good split is a thing that already existed inside a larger act, now with its own name. The PR description has a before/after line-count table.

**`naming`** (`naming: `) renames one badly named thing in source code. A variable, a type, a file, a folder, a concept. Names in the user interface are left alone.

**`context`** (`context: `) reads `CONTEXT.md` in the product repo, picks one term, and fixes places the code calls that same idea something else. If the file is missing, this routine stops.

**`cruft`** (`cruft: `) deletes outdated docs, dead code, or leftover files, after a second look that they really are unused. It skips the `docs/` folder, and anything edited in the last five days.

A sixth routine, **`rules-review`**, fires on every new commit to a pull request. It reads `AGENTS.md` and `.cursor/rules` in the product repo and leaves a review comment whose first sentence is either **Rules: passed.** or **Rules: violated.**

Before the main job, the PR-opening routines refresh a `MEMORIES.md` in the product repo, at most once a week per prefix. Merged PRs become examples. PRs closed without a merge become counterexamples. That file is how later runs learn what this particular repo accepts.

## Skills

The longer how-tos live under `skills/`. `refactor` uses `refactor-small` (one local problem) or `refactor-big` (one architectural pattern, still behavior-preserving). The PR-opening routines use `examples-counterexamples` to keep `MEMORIES.md` current.

## Using it

These routines fire on GitHub events, so they need a **cloud factory**. From a [home repo](https://github.com/staltz/fabrica/blob/main/docs/home-repos.md):

```yaml
# ~/.fabrica/factories/refactor.yaml
blueprint:
  git: github.com/staltz/fabrica-blueprint-refactor
  ref: master
server: https://fabrica.example.com
runtimes: [cursor-cloud]
targets: [github.com/you/your-app]
```

Then `fabrica plan` and `fabrica apply`. [Getting started](https://github.com/staltz/fabrica/blob/main/docs/getting-started.md) walks through the rest.

You can also import this blueprint into one of your own:

```yaml
imports:
  refactor:
    git: github.com/staltz/fabrica-blueprint-refactor
    ref: master
```

Run `fabrica lock` after that, so the import pins to an exact commit.

Give the product repo an `AGENTS.md`. Add a `CONTEXT.md` if you want the term-alignment routine to have something to do.

## Editing this repo

Run `fabrica check` after every change. It is strict on purpose. It catches typos in frontmatter and missing connectors.

Prompt and skill edits take effect on the next run, once they are merged, for factories that follow this branch. Changing a routine's trigger or connectors needs another `fabrica apply` in each home repo that uses this blueprint.

[Writing a blueprint](https://github.com/staltz/fabrica/blob/main/docs/blueprints.md) is the full format.
