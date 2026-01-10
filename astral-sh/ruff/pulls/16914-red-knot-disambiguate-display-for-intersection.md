```yaml
number: 16914
title: "[red-knot] Disambiguate display for intersection types"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-intersection-callable-display
created_at: 2025-03-22T12:23:38Z
updated_at: 2025-03-24T23:04:08Z
url: https://github.com/astral-sh/ruff/pull/16914
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] Disambiguate display for intersection types

---

_Pull request opened by @MatthewMckee4 on 2025-03-22 12:23_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #16912 

Create a new type `DisplayMaybeParenthesizedType` that is now used in Union and Intersection display

## Test Plan

Update callable annotations


---

_Review requested from @carljm by @MatthewMckee4 on 2025-03-22 12:23_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-03-22 12:23_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-03-22 12:23_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-03-22 12:23_

---

_Comment by @github-actions[bot] on 2025-03-22 12:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @AlexWaygood on 2025-03-22 21:35_

---

_@carljm approved on 2025-03-23 14:18_

Thank you!

---

_Merged by @carljm on 2025-03-23 14:18_

---

_Closed by @carljm on 2025-03-23 14:18_

---

_Branch deleted on 2025-03-24 23:04_

---
