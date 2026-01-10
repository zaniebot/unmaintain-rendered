```yaml
number: 17297
title: "[red-knot] Fix false positives on `types.UnionType` instances in type expressions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/uniontype-type-expression
created_at: 2025-04-08T17:37:38Z
updated_at: 2025-04-09T17:33:19Z
url: https://github.com/astral-sh/ruff/pull/17297
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Fix false positives on `types.UnionType` instances in type expressions

---

_Pull request opened by @AlexWaygood on 2025-04-08 17:37_

## Summary

For an implicit type alias like `X = int | str`, don't emit a false positive if `X` is then used in a type annotation.

## Test Plan

Added a test


---

_Label `red-knot` added by @AlexWaygood on 2025-04-08 17:37_

---

_Comment by @github-actions[bot] on 2025-04-08 17:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-04-09 17:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-09 17:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-09 17:27_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-09 17:27_

---

_@carljm approved on 2025-04-09 17:32_

Nice!

---

_Merged by @AlexWaygood on 2025-04-09 17:33_

---

_Closed by @AlexWaygood on 2025-04-09 17:33_

---

_Branch deleted on 2025-04-09 17:33_

---
