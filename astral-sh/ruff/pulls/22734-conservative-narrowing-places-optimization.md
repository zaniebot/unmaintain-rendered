```yaml
number: 22734
title: Conservative narrowing places optimization
type: pull_request
state: open
author: ericmarkmartin
labels:
  - ty
assignees: []
draft: true
base: main
head: synthetic-narrow-definitions
created_at: 2026-01-19T19:51:57Z
updated_at: 2026-01-19T20:03:03Z
url: https://github.com/astral-sh/ruff/pull/22734
synced_at: 2026-01-19T20:31:01Z
```

# Conservative narrowing places optimization

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add helpers to compute a conservative upper bound on the set of places a predicate might actually narrow, and use this to save work when computing narrowing constraints.

For now just a draft PR to see perf numbers.

## Test Plan

<!-- How was it tested? -->
Existing tests pass


---

_Label `ty` added by @AlexWaygood on 2026-01-19 20:03_

---
