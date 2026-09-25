---
name: staged-pr-train
description: Ship a large feature as a sequence of small, independently reviewable pull requests instead of one giant PR. Use when a change will touch many files or run past a few hundred lines, when someone asks to "split this PR", "stack PRs", "break this into smaller PRs", or plans a feature that needs schema, backend and UI work together.
---

# Staged PR train

A 2,000-line pull request gets skimmed, not reviewed. The same change split into four or five
small PRs, each one green and mergeable on its own, gets read. This skill plans the split,
builds the branches in order, and keeps every stop on the train shippable.

## When to use it

- The change will be more than roughly 400 changed lines, or touch more than one layer
  (data model, backend, frontend, config).
- A reviewer has asked for a smaller PR.
- You're about to start a feature and want the plan before the code.

Skip it for a one-file fix or anything a reviewer can read in ten minutes.

## Step 1: map the change before writing code

Read the request and the code it touches. Write a short plan in the conversation with one line
per stop, in merge order. Each stop needs:

- **What it does** in one sentence a reviewer would understand without the others.
- **Why it's safe on its own.** It builds, tests pass, and production behaves the same or
  better if the train stopped here.
- **Rough size.** Aim for under 400 changed lines per stop.

Good default order: (1) pure refactors or moves with no behaviour change, (2) new data model or
schema, additive only, (3) backend logic behind a flag or unused entry point, (4) the UI or API
surface that turns it on, (5) cleanup of the old path. Drop any stop the change doesn't need.

Show the plan and get a yes before building. If a stop can't be made safe on its own, say so
and merge it with its neighbour rather than shipping a broken intermediate state.

## Step 2: build the train

Work in order, one branch per stop, each branched from the previous one:

```bash
git switch -c train/1-<name> main
# build stop 1, commit, run the tests
git switch -c train/2-<name>        # branches from train/1
# build stop 2 ...
```

For every stop:

1. Keep the diff to that stop's job. Anything unrelated goes in a later stop or a separate PR.
2. Run the project's type check and tests before moving on. A red stop blocks the train.
3. Write the PR description for that stop alone: what changed, why it's safe to merge now,
   and "Part N of M" with links to the other stops.

Open each PR against the branch before it (stop 1 against `main`, stop 2 against stop 1),
so each PR's diff shows only its own changes.

## Step 3: merge in order

Merge stop 1 first. Then retarget stop 2 to `main` (most hosts do this automatically when the
base branch merges), rebase if needed, confirm it's still green, and merge. Repeat to the end.

If review changes an early stop, rebase the later branches onto it before continuing:

```bash
git switch train/2-<name> && git rebase train/1-<name>
```

## What good looks like

- Every PR can be read in one sitting and approved on its own merits.
- `main` is releasable after every merge.
- The last stop is small because the earlier ones did the heavy lifting.
- Nobody reviews forty files at six in the evening.
