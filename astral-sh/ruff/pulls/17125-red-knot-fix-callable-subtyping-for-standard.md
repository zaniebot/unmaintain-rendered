```yaml
number: 17125
title: "[red-knot] Fix callable subtyping for standard parameters"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-subtype-fix
created_at: 2025-04-01T16:52:20Z
updated_at: 2025-04-01T18:07:37Z
url: https://github.com/astral-sh/ruff/pull/17125
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Fix callable subtyping for standard parameters

---

_@dhruvmanila_

## Summary

This PR fixes a bug in callable subtyping to consider both the positional and keyword form of the standard parameter in the supertype when matching against variadic, keyword-only and keyword-variadic parameter in the subtype.

This is done by collecting the unmatched standard parameters and then checking them against the keyword-only / keyword-variadic parameters after the positional loop.

## Test Plan

Add test cases.


---

_Label `bug` added by @dhruvmanila on 2025-04-01 16:52_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-01 16:52_

---

_Comment by @github-actions[bot] on 2025-04-01 16:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-04-01 17:16_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-01 17:16_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-01 17:16_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-01 17:16_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-01 17:16_

---

_@carljm approved on 2025-04-01 18:06_

Nice catch!

---

_Merged by @dhruvmanila on 2025-04-01 18:07_

---

_Closed by @dhruvmanila on 2025-04-01 18:07_

---

_Branch deleted on 2025-04-01 18:07_

---
