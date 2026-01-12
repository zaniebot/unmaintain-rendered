```yaml
number: 22212
title: Implement a fixer for E402
type: pull_request
state: open
author: PeterJCLaw
labels: []
assignees: []
draft: true
base: main
head: e402-fixer
created_at: 2025-12-26T19:05:54Z
updated_at: 2025-12-26T22:52:25Z
url: https://github.com/astral-sh/ruff/pull/22212
synced_at: 2026-01-12T15:57:44Z
```

# Implement a fixer for E402

---

_@PeterJCLaw_

## Summary

This implements a fixer for E402, as suggested at https://github.com/astral-sh/ruff/issues/6514.

This enables users to move imports up to the top of the file when they are found lower down. This is similar to isort's `float-to-top` logic, though not quite the same. As suggested by @ntBre on that issue, the expected path here is for people to enable unsafe-fixes to access this feature, potentially marking this fix "safe" in their config, rather than to implement an `float-to-top` option in the isort config.

Internally this means that there are separate fix edits for each import found to be out of place rather than a single one (the latter being how isort tends to work).

The current state of this is a _first draft_. It's working, I've done some manual testing and I have copied over a lot of the handling from the isort-like `organize_imports` function, though I may have missed things.

I'm also fairly new to rust and to this project so e.g: I don't know if e.g: the code is the best way to spell some of the concepts. I'm very happy to refactor things if there's better ways, including if e.g: we want to share more code between this and the isort crate -- I've deliberately not done that so far (purely to get to something which works).

This remains a bit work-in-progress, so I've included a test project I'm using for testing against though obviously that would be removed before this gets anywhere near merging. (I might also tidy the history a bit too)

Fixes https://github.com/astral-sh/ruff/issues/6514

## Test Plan

Manual testing so far, see various demo files currently committed. I'd like to add some tests and will look into that next. It looks like it's mostly snapshot based testing. Do we just add a new test function in `crates/ruff_linter/src/rules/pycodestyle/mod.rs`?

Noting them down so they're written, test cases:
- interaction with editor organise-imports logic -- with and without the E402 fix being accepted as safe
- variations on import spelling
- variations of other statements near the import, including on the same line (e.g: peruse the isort tests and pick relevant looking ones)


---

_Comment by @astral-sh-bot[bot] on 2025-12-26 19:14_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---
