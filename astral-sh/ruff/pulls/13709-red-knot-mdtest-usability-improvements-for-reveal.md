```yaml
number: 13709
title: "[red-knot] mdtest usability improvements for reveal_type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/mdtest-undefined-reveal
created_at: 2024-10-10T23:39:17Z
updated_at: 2024-10-11T00:33:55Z
url: https://github.com/astral-sh/ruff/pull/13709
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] mdtest usability improvements for reveal_type

---

_@carljm_

## Summary

Fixes #13708.

Silence `undefined-reveal` diagnostic on any line including a `# revealed:` assertion.

Add more context to un-silenced `undefined-reveal` diagnostics in mdtest test failures. This doesn't make the failure output less verbose, but it hopefully clarifies the right fix for an `undefined-reveal` in mdtest, while still making it clear what red-knot's normal diagnostic for this looks like.

## Test Plan

Added and updated tests.

---

_Label `red-knot` added by @carljm on 2024-10-10 23:39_

---

_Review requested from @MichaReiser by @carljm on 2024-10-10 23:39_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-10 23:39_

---

_Comment by @github-actions[bot] on 2024-10-10 23:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-10-11 00:21_

Nice, thank you!

---

_Merged by @carljm on 2024-10-11 00:33_

---

_Closed by @carljm on 2024-10-11 00:33_

---

_Branch deleted on 2024-10-11 00:33_

---
