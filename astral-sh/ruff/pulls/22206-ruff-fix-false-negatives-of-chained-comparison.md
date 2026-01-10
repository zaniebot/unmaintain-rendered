```yaml
number: 22206
title: "[`ruff`] Fix false negatives of `chained-comparison` (PLR1716)"
type: pull_request
state: open
author: Bnyro
labels: []
assignees: []
base: main
head: boolean-chained-comparison-order
created_at: 2025-12-26T13:41:41Z
updated_at: 2025-12-26T13:43:15Z
url: https://github.com/astral-sh/ruff/pull/22206
synced_at: 2026-01-10T16:36:18Z
```

# [`ruff`] Fix false negatives of `chained-comparison` (PLR1716)

---

_Pull request opened by @Bnyro on 2025-12-26 13:41_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
All the following if cases have the same semantic meaning:
```py
x = 5

if 4 < x and x < 7:
    ...

if x > 4 and x < 7:
    ...

if x > 4 and 7 > x:
    ...
```
However, the `boolean-chained-comparison` rule currently only spots the first case, even though all of them could be simplified by comparison chaining.

This PR implements a detection mechanism for the missing cases, however it does not implement quick fixes for them since this turned out to be quite difficult.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
I've added new test case snapshots and compared the behavior to `pylint`.

---
